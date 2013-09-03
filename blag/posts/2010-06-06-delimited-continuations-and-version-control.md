---
title: Delimited Continuations and Version Control
tags: continuations, darcs, imported
---

Delimited continuations give us a way to create markers that we can jump back
to.  We can construct the future of the computation, work with the computation
so far, or abort the current continuation and go down a new path (create a new
future).

These primitives have a natural correspondence with version control systems that snapshot the world like
SVN. &nbsp;Focusing on SVN for a moment:
<ul>
<li>The current continuation is the transformation of repository state that you're working on. &nbsp;It's the diff you're creating.</li>
<li>For commits, each revision created by commit is a marker, so we model this with <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">reset</span><span class="Apple-style-span" style="font-family: inherit;">.</span></li>
<li>Checking out an older revision, or reverting changes, corresponds to <span class="Apple-style-span" style="font-family: inherit;">a </span><span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">shift</span><span class="Apple-style-span" style="font-family: inherit;">. &nbsp;We discard the current continuation and move back to a marker created by a specific </span><span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">reset</span><span class="Apple-style-span" style="font-family: inherit;">.</span></li>
<li><span class="Apple-style-span" style="font-family: inherit;">Updating consists of having the client copy learn the current state of the continuation on the server and applying it to the local copy.</span></li>
<li>Starting a branch corresponds to a <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">reset</span><span class="Apple-style-span" style="font-family: inherit;">.</span></li>
<li><span class="Apple-style-span" style="font-family: inherit;">Merging two branches is a bit trickier I suspect. &nbsp;I haven't worked out all the details sufficiently to convince myself I have it right, but here is how I think this case works. &nbsp;The merge first </span><span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">shift</span><span class="Apple-style-span" style="font-family: inherit;">s to each of the markers and then combines those two continuations into one future. &nbsp;The part that seems weird to me about this, is that I haven't really seen examples of delimited continuations were the continuation of two different markers (prompts) were combined.</span></li>
</ul>
<div>
Now, if you accept the above it gives us some intuition to build on. &nbsp;Although my correspondence is terribly informal at the moment, if we took some time to make it formal by working out enough details to model it in, say, Haskell, then we'd have a nice formal backing for how SVN works. &nbsp;I think the above model applies equally well to git, but I'm not confident with git's model.</div>
<div>

</div>
<div>
One insight the above gives, is that the way merging works is not described by the continuations in general. &nbsp;It's up to the exact combining function to determine the merge. &nbsp;We know that an automatic merge can fail in practice due to things like conflicts between the changes in the branches. &nbsp;So, in the SVN implementation considerable work has gone into implementing logic for creating the proper continuation.</div>
<div>

</div>
<div>
Now, in general when a merge, or update, is performed human intervention may be required. &nbsp;This typically happens when the changes are in conflict. &nbsp;What this means for our model is that in general the creation of the continuation requires knowledge outside of our model. &nbsp;What does that correspond to? &nbsp;Well, it's essentially saying that calculating the correct continuation to resolve the merge is <b>non-deterministic</b>!</div>
<div>

</div>
<div>
In other words, this continuation view of version control gives us a rigorous way to talk about our intuition. &nbsp;Of course we can easily tell that merging is going to require human intervention without needing to study delimited continuations, but this framework of reasoning now gives us a more mathematical way to say it.</div>
<div>

</div>
<div>
The next question is: &nbsp; How does the delimited continuation model of vcs apply to Darcs?</div>
<div>

</div>
<div>
So far I'm not sure. &nbsp;I suspect, without working through the details, that in Darcs, every time you record a patch multiple <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">reset</span><span class="Apple-style-span" style="font-family: inherit;">s happen, instead of just one. &nbsp;The model really breaks down for Darcs because you can seemingly visit points in "history" that did not exist when the patches were created, but they are valid repository states.</span></div>
<div>
<span class="Apple-style-span" style="font-family: inherit;">
</span></div>
<div>
<span class="Apple-style-span" style="font-family: inherit;">For example, imagine a repository that only exists locally. &nbsp;You create a sequence of patches. &nbsp;Now, take a patch in the middle that can be commuted to the end of the patch sequence. &nbsp;Doing so has not created a new repository state; so far this is fine with the above model as no new markers need to be created. &nbsp;Suppose we remove the patch from the end of the patch sequence. &nbsp;This is exactly how darcs unpull works. &nbsp;The funny thing is, we've now created a state that never existed previously. &nbsp;So in the delimited continuation model, what marker did we just </span><span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">shift</span><span class="Apple-style-span" style="font-family: inherit;">&nbsp;to?</span></div>
<div>
<span class="Apple-style-span" style="font-family: inherit;">
</span></div>
<div>
<span class="Apple-style-span" style="font-family: inherit;">I don't know the answer yet, but I think it's an interesting question. &nbsp;I suspect there are multiple "correct" answers, but that only some answers will yield elegant and robust models here.</span></div>

