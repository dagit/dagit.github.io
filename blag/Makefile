BLAG           := .cabal-sandbox/bin/blag
CABAL          := cabal

.PHONY: all clean deploy rebuild

all:
	$(BLAG) build

clean:
	$(BLAG) clean

deploy:
	$(BLAG) deploy

rebuild:
	$(BLAG) rebuild

blag:
	$(CABAL) install
