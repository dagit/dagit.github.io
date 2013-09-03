---
title: OpenCL + Language.C.Quote
tags: haskell, opencl
---

Here is a simple introduction to combining Haskell, OpenCL, and
Language.C.Quote into one coherent bundle. Our goal is to make a simple Haskell
program that embeds an OpenCL program and runs it on the GPU.

Let's start with `cabal init`. Make sure to add the following packages to your
`build-depends`. I'm going to make an executable for this sample. My
`example01.cabal` looks like this:

```
name:                example01
version:             0.1.0.0
license-file:        LICENSE
build-type:          Simple
cabal-version:       >=1.8

executable example01
  main-is:             example01.hs
  build-depends:       base
                     , language-c-quote
                     , OpenCL
                     , mainland-pretty
```

Next, we're going to copy `example01.hs` from the OpenCL distribution and modify
it slightly.

Here is the original:
[https://github.com/IFCA/opencl/blob/master/examples/example01.hs](https://github.com/IFCA/opencl/blob/master/examples/example01.hs)

Here is our modified version (and stripped down to remove license, it was BSD3):

``` haskell
{-# LANGUAGE QuasiQuotes #-}
module Main where
{- Copyright (c) 2011 Luis Cabellos -}
import Control.Parallel.OpenCL
import Foreign( castPtr, nullPtr, sizeOf )
import Foreign.C.Types( CFloat )
import Foreign.Marshal.Array( newArray, peekArray )
import Language.C.Quote.OpenCL
import Text.PrettyPrint.Mainland

programSource :: String
programSource = show $ ppr [cunit|
__kernel void duparray(__global float *in, __global float *out)
{
  int id = get_global_id(0);
  out[id] = 2*in[id];
}
|]

main :: IO ()
main = do
  putStrLn "Compiling:"
  putStrLn programSource
  -- Initialize OpenCL
  (platform:_) <- clGetPlatformIDs
  (dev:_) <- clGetDeviceIDs platform CL_DEVICE_TYPE_ALL
  context <- clCreateContext [CL_CONTEXT_PLATFORM platform] [dev] print
  q <- clCreateCommandQueue context dev []
  
  -- Initialize Kernel
  program <- clCreateProgramWithSource context programSource
  clBuildProgram program [dev] ""
  kernel <- clCreateKernel program "duparray"
  
  -- Initialize parameters
  let original = [0 .. 20] :: [CFloat]
      elemSize = sizeOf (0 :: CFloat)
      vecSize = elemSize * length original
  putStrLn $ "Original array = " ++ show original
  input  <- newArray original

  mem_in <- clCreateBuffer context [CL_MEM_READ_ONLY
                                   ,CL_MEM_COPY_HOST_PTR]
                                   (vecSize, castPtr input)  
  mem_out <- clCreateBuffer context [CL_MEM_WRITE_ONLY]
                                    (vecSize, nullPtr)

  clSetKernelArgSto kernel 0 mem_in
  clSetKernelArgSto kernel 1 mem_out
  
  -- Execute Kernel
  eventExec <- clEnqueueNDRangeKernel q kernel [length original] [1] []
  
  -- Get Result
  eventRead <- clEnqueueReadBuffer q mem_out True 0 vecSize (castPtr input)
                                                            [eventExec]
  
  result <- peekArray (length original) input
  putStrLn $ "Result array = " ++ show result

  return ()
```

The important part is that we replaced the string version of the OpenCL program
(`programSource`) with a quasiquoter version. We use `cunit` from
`Language.C.Quote.OpenCL` to parse the OpenCL program as a Haskell data type.
Then we can use `ppr` from `Text.PrettyPrint.Mainland` plus `show` to run it
back into a string. We could also do more interesting things. We could use
Haskell as a macro language or we could use antiquotation to insert Haskell
values into the program. Essentially, Haskell becomes our meta-language for
working with OpenCL programs.

**Note:** Each platform may require slightly different flags to cabal to find
your native OpenCL. On windows, I installed NVIDIA's OpenCL implementation, so
I add:

    --extra-lib-dirs="C:\Program Files\NVIDIA Corporation\OpenCL"

when running `cabal install` or `cabal-dev install`.

