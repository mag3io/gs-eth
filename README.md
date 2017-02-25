# gs-eth
Getting started with Ethereum

agenda:

* Some interesting reading to start with
* Installation (docker based)
* Your first contract (based on ethereum tutorials)
* Refactor it with truffle
* Implement the two last tutorials of ethereum

# Documentation

[The white page](https://github.com/ethereum/wiki/wiki/White-Paper)
[Ethereum Homestead Documentation](http://www.ethdocs.org/en/latest/)
[Go Etherum Wiki](https://github.com/ethereum/go-ethereum/wiki)
[Solidity Documentation](https://solidity.readthedocs.io/en/latest/#)

# Installation

## The CLI

[Ethereum implementation in Go](https://github.com/ethereum/go-ethereum/wiki)

### Standalone installation

```sh
brew update
brew upgrade
brew tap ethereum/ethereum
brew install ethereum
```

This is an extract from the [documentation](https://www.ethereum.org/cli)

As an alternative look to [Browser Solidity](https://ethereum.github.io/browser-solidity)

### Docker based Installation

Everything [here](https://hub.docker.com/r/ethereum/client-go/)

Get images with geth client in Golang:
```ssh
docker pull ethereum/client-go:alpine
```

## Start node in dev mode
Start node in dev mode using geth client with RPC:
//TOOD add rpc explanation
```ssh
docker run -d --name eth-dev-node -p 8545:8545 -p 30303:30303 ethereum/client-go:alpine --dev --rpc
```

## Attach to running node
`docker exec -it eth-dev-node ./geth attach ipc:/tmp/ethereum_dev_mode/geth.ipc`

## Create account
In the geth console execute:
* `eth.accounts` should display [] meaning no accounts
* `personal.newAccount('A_PASSWORD')` where you choose the value for A_PASSWORD. This should output the address of this account.
* `eth.accounts` or `personal.listAccounts`
* `eth.getBalance(SOME_ADDRESS)` where SOME_ADDRESS is one of an account. Should display 0 for the moment since you have to mine to get some ehter.


## Start mining
Set mining beneficiary account:
* `miner.setEtherbase(SOME_ADDRESS)` where SOME_ADDRESS is the one of your account.
* `eth.coinbase` will display the address of the account which will be credited while mining.

Start mining:
* `miner.start()` to start mining. Since in dev-mode you run a private network you will mine easily and so getting faster ether.
Note that `miner.start()` have an optional parameter to indicate number of threads used for mining.

Wait for a minute then `miner.stop()`.
You can now check the balance.

## Create your first contract

### First contract

[Greeter contract](https://www.ethereum.org/greeter)

#### Using Solidity web ui
[Solidity web](hhttps://ethereum.github.io/browser-solidity)


## A dev environment

### Truffle
Truffle [howtos](http://truffleframework.com/docs/getting_started/installation)

#### Installation
Install Truffle:
* `npm install -g truffle`

Install test RPC client:
* [EthereumJS TestRPC](https://github.com/ethereumjs/testrpc)
* `npm install -g ethereumjs-testrpc`

Run test client:
* `testrpc`

#### Init truffle project structure
Create truffle_example folder, go in and run :
* `truffle init`

this will create initial **project structure** :
* `contracts` : all contracts
* `migrations` : contract deployments
* `build` :  built artifacts
* `test` :  tests

#### Contract compilation
To compile contracts from `contracts` fodler : 
* `truffle compile`

#### Contract deployment
Migrations are used to deploy contracts.

To run migrations from `migrations` folder:
* `truffle migrate`

#### Testing
To run all tests from `test` folder:
* `truffle test`

##### Tests in  JavaScript (.es .es6 .jsx .sol)
[Mocha](http://mochajs.org/) test framework is used behind,  `describe()` functions
can be replaced by `contract()` to hook-up clean contract state testing environment before each test.

##### Tests in  Solidity
Less verbose than JS tests & obviously DSL is more targeted 
to the very specific task than generic Mocha library.
 Provides as well  : 
 * an easy access to all deployed contracts & accounts
 * assertion library
 * pre/post-test hooks

#### Interaction with contracts
Reading data from network is named `call'.
Writing data is called a `transaction`.

##### Transaction
Every Tx could be an operation of: 
* sending money to another account
* executing a contract's function
* registering a new contract

ALl transactions:
* Cost gas (Ether)
* Change the state of the network
* Aren't processed immediately
* Won't expose a return value (only a transaction id).

##### Call
Can execute code on the network but wothout permanent data changes!
* Are free (do not cost gas)
* Do not change the state of the network
* Are processed immediately
* Will expose a return value (hooray!)

#### Contract abstractions
Wrappers around contracts to make interactions with Ethereum contract from JS easier.
Truffle has own abstraction module : [truffle-contract](https://github.com/trufflesuite/truffle-contract)

Basically `sendCoin`, `getBalanceInEth` and `getBalance` are exposed for interactions.
All calls, transactions and fired events could be nicely executed and chaine using promises.

Contracts can be as well deployed and as a part of the test.

#### Package management ETHPM
#### ETHPM
[EHTPM](https://www.ethpm.com/)  is a pakage registry.

Packages are treated asn npm depenndencies ([ethpm.json](http://truffleframework.com/docs/getting_started/packages-npm#package-management)).

Command `truffle install package@version` will install referenced package.  

To pusblish  see more documentation [here](http://truffleframework.com/docs/getting_started/packages-ethpm#ropsten-ropsten-ropsten)

#### npm

NPM can be used as well to publish ETH artifacts, mores infos can be found [here](http://truffleframework.com/docs/getting_started/packages-npm#package-management)

## The private net

Follow the steps from [here](https://souptacular.gitbooks.io/ethereum-tutorials-and-tips-by-hudson/content/private-chain.html)


`--datadir "./.ethereum-private" --genesis CustomGenesis.json --nodiscover --maxpeers 0`

# Questions
* no gas is paid to execute a contract

# Useful links
https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options
https://github.com/ethereum/go-ethereum/wiki/geth
https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console

https://ethereum.gitbooks.io/frontier-guide/content/getting_a_client.html
https://souptacular.gitbooks.io/ethereum-tutorials-and-tips-by-hudson/content/private-chain.html

https://github.com/ConsenSys/truffle

https://ipfs.io/
https://golem.network/

http://blogchaincafe.com/les-consensus-proof-of-work-vs-proof-of-stake