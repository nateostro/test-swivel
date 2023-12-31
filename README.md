[![Build Status](https://travis-ci.com/Swivel-Finance/gost.svg?token=mHzJQzb11WHSPwztZw8B&branch=main)](https://travis-ci.com/Swivel-Finance/gost)

##### Smart contract testing with Geth via the Golang ABIGEN

```
                _______
               /______/\
               \__::::\/
      _______      ______       ______       _________
     /______/\    /_____/\     /_____/\     /________/\
     \::::__\/__  \:::_ \ \    \::::_\/_    \__.::.__\/     ,--.
      \:\ /____/\  \:\ \ \ \    \:\/___/\      \::\ \      |  oo|
       \:\\_  _\/   \:\ \ \ \    \_::._\:\      \::\ \     |  ~~|  o  o  o  o  o  o  o  o  o  o
        \:\_\ \ \    \:\_\ \ \     /____\:\      \::\ \    |/\/\|
         \_____\/     \_____\/     \_____\/       \__\/
```

## Getting Started

This project contains the Swivel Smart Contracts and Libraries which have been compiled to their `abi` and `bin` components, those then transformed into
golang bindings via the Geth `abigen` tool. You can see the commands used to perform these tasks in the `Makefile`.

Tests are located in `/pkg/*testing/`. For example, the unit tests for the Swivel.sol contract will be located in `/pkg/swiveltesting`.

Existing tests, and any newly created, are always run via `go test [-v] ./...` from the project root.

If you are only here to view the existing tests, and maybe pull this repo and run it you can stop here. If you are a developer writing new tests or simply
an interested party who realizes the _mind blowing_ superiority of testing your smart contracts this way - read on!

#### Notes on /build

The `/build` directory exists to house our 2 deployed contracts (and 2 deployable child contracts) with all of their dependencies and compiled artifacts. Those being not only the `.sol` files but the
`.bin`, `.abi` and `.go` files as well. These compiled artifacts will be different from what is in the `/test` directories, as those are compiled with mocks in place (these are not).

Note that the files here should be considered "alpha" stage as what is present [here](https://github.com/Swivel-Finance/swivel/tree/main/contracts) should be considered "stable".

### Geth

Gost only depends on Geth itself. We _do_ list `solc` as being necessary, but that isn't _exactly_ true.
If you are in possesion of the `abi` and `bin` files of any smart contract (regardless of language used) the
Makefile's `compile_*` type steps can be made for them.

As this Golang project is done with modules, you don't need to specifically `go get` anything. All dependencies will be fetched on your first test run.

### Solidity Compiler

First, assure that `solc` is in your $PATH. If not, [make it so](https://docs.soliditylang.org/en/v0.8.0/installing-solidity.html). As stated above,
this is assuming your smart contracts are `.sol` and you do not already have the `.abi` and `.bin` files. Add new Makefile rules for other languages
(Vyper for example) and compilers if you are so inclined.

### Geth ABIGEN

Second, you will need `abigen` in your $PATH. You can, for example, follow the instructions [here](https://github.com/ethereum/go-ethereum).
Note that you only need to use the `devtools` rule from the go-ethereum Makefile. i.e. `make devtools` will suffice.

### Project Structure

As always we use the [project layout](https://github.com/golang-standards/project-layout) guidelines. This interpretation places the
Solidity files into `test/` with their compiled artifacts going into aptly named subdirectories for ease of use with
golang's package conventions (see Makefile). Some notes on the important residents of `/test/`:

- swivel/
  - Swivel.sol. The current Swivel smart contract
  - Abstracts.sol. The external interfaces declared by Swivel.sol
  - Sig.sol. Imbeddable library used by Swivel.sol
  - Hash.sol. Imbeddable library used by Swivel.sol
  - swivel.go. Autogenerated Geth bindings for use in tests.
- marketplace/
  - MarketPlace.sol The current MarketPlace smart contract
  - Abstracts.sol. The external interfaces declared by MarketPlace.sol
  - VaultTracker.sol. A mock implementation of the VaultTracker smart contract. Used to build and test MarketPlace in isolation
  - ZcToken.sol. A mock implementation of the ZcToken smart contract.
  - marketplace.go. Autogenerated Geth bindings.
- vaulttracker/
  - VaultTracker.sol. The current VaultTracker smart contract
- tokens/
  - ZcToken.sol. An Open Zeppelin clone of an extended Erc20 with mint, burn and permit.

#### The Makefile

This is the way.

Steps for setting-up, tearnig-down and testing the individual pieces of the repo are here.

#### Compiling and Testing Your Contracts

- Compiling:
  - `make all`

If you wish to run the steps separately or run into any errors that need debugging, see the Makefile for the entire list of available commands.

- Testing:
  - `go test ./...` from root (as stated). Add the -v flag if you expect to see any logging you may be doing.
  - You can of course test at the package level: `go test ./path/to/package`
