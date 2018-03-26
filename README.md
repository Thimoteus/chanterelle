# chanterelle

<img src=https://github.com/f-o-a-m/purescript-web3/blob/master/purescript-web3-logo.png width="75">

_a build tool with example application_

## Parking DAO

The Parking DAO is a a set of smart contracts that encapsulate a `User`, representing an account which is granted permission to park in certains geographical zones, and a `ParkingAnchor`, representing an account which has the ability to accept payment for parking in certain zones. 
These accounts are deployed by a central authority called the `ParkingAuthority`, which is a governing contract in charge of account management. The `ParkingAuthority` also contains the logic for altering account permissions.

For more information about how to use the contracts, see [this README](https://github.com/f-o-a-m/chanterelle/blob/master/sequence-diagrams/README.md), or look at the contracts in the `/contracts` directory. You can find the tests in `/test` that verify their behaviour.

## Build/Deploy Overview

This repo is meant to be a templated complement to `truffle`. You can clone it and use it for development, deployment and testing of solidity smart contracts. It uses `truffle` as a build tool, taking advantage of it's bundled compiler. However it allows for deployment and testing to be written in purescript, hopefully freeing us from some of the nonsense we ocassionally have to deal with when prototyping and deploying with `truffle`. We also recommend [cliquebait](https://github.com/f-o-a-m/cliquebait) as a replacement for `testrpc`. 

### Build

In order to build the project, including all of the purescript files generated by purescript-web3-generator for each abi
the artifacts folder `build/contracts`, run

```bash
> make install
```

If you write a new solidity contract, or edit an old one, run

```bash
> make compile-contracts
```

This will compile all new contracts including any existing contracts which have changed. It will also regenerate their 
corresponding purescript files, which means you will give you the benefit of type-errors when you have broken your existing application. Note that the solidity code will still compile, even if the purescript code doesn't.

### Deploy

Currently deployment is done via the `main` function in `src/Main.purs`. Your deploy script can be arbitrarily complicated,
using whatever purescript libraries and web3 calls you want. The deployment configuration for each contract (including 
the filepath for the build artifact and and necessary arguments for deployment) is kept in `src/ContractConfig.purs`.

When you are ready to deploy, you can run

```bash
> make deploy
```

You should see the logs from the deployment printed to the console, and the address for the recently deployed contract should
be written to the corresponding build artifact's "networks" object under the key corresponding to the networkId of the
network you were using. I.e., it follows `truffle`'s existing pattern.

### Test

You can write whatever tests you want in purescript to include in `test/Main.purs`. Each tests takes a global config
from the deployment as an argument passed from `main`. When you want to run the tests, run

```bash
> make test
```
