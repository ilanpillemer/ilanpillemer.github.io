---
layout: post
title: "ethereum ftw!"
summary: "exploring ethereum for the sake of the future"
tags: [fun, profit]
author: ilan
---
## Where to begin...
So to begin I need to install and learn some stuff beginning here
- https://ethereum.org/cli
- https://ethereum.gitbooks.io/frontier-guide/content/cli.html

### Solidity...
So you need to write your Ethereum contracts in a language that can be compiled to the EVM.
- http://solidity.readthedocs.io/en/develop/installing-solidity.html
- [emacs plugin](https://github.com/ethereum/emacs-solidity)
### Argh!
- https://github.com/ethereum/go-ethereum/issues/3793
### Add test account to geth
```
Ilans-MacBook-Pro-2:~ ilanpillemer$ geth account import ./.test_priv_key_ethereum 
WARN [09-01|23:36:03] No etherbase set and no accounts found as default 
Your new account is locked with a password. Please give a password. Do not forget this password.
Passphrase: 
Repeat passphrase: 
Address: {a0447afd72c9945faa830277f75e963839cf57c1}
...
geth attach ipc://Users/ilanpillemer/Library/Ethereum/testnet/geth.ipc
```
and remember there are different keystores for the mainchain and testnet.
and remember you need to wait for the testnet to sync to a recent enough block to see your correct balance
####
and wait...(it took way longer than I expected)
- you can check if the chain is synced with the following command
```
Date.now() / 1000 - web3.eth.getBlock('latest').timestamp
```
It should return a number less than 60s....

### Hello World!
- https://www.ethereum.org/greeter
- https://github.com/ethereum/go-ethereum/wiki/Contract-Tutorial

### If I was a rich man....
... then I would could spend gas and experiment on the main chain.
But I am not; so need to work on a test chain.
- https://github.com/ethereum/ropsten

### Getting a life cycle into play
I will need a way to write and run contracts locally before deploying to a testnet and eventually to the mainnet.
* https://github.com/ethereumjs/testrpc
