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
- you can check your ether in your account with
```
web3.fromWei(eth.getBalance(eth.coinbase), "ether")
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

And a way to run unit tests
* http://truffleframework.com/

And then discover this, which is showstopper for me.
* https://github.com/trufflesuite/truffle/issues/471

So look for a less controlling, more pluggable environment... perhaps this
* https://github.com/pipermerriam/populus

Not so sure about any of these test frameworks actually. Pausing for now on that as they do work, if kludgy.

### Compiling and Deploying

- https://ethereum.gitbooks.io/frontier-guide/content/creating_contract.html

```
primaryAddress = eth.accounts[0]
MyContract = eth.contract(abi);
contact = MyContract.new(arg1, arg2, ...,{from: primaryAddress, data: evmCode})
```
or
```
MyContract.new([arg1, arg2, ...,]{from: primaryAccount, data: evmCode}, function(err, contract) {
  if (!err && contract.address)
    console.log(contract.address); 
});
```

- compiling
```
solc, the Solidity commandline compiler.

This program comes with ABSOLUTELY NO WARRANTY. This is free software, and you
are welcome to redistribute it under certain conditions. See 'solc --license'
for details.

Usage: solc [options] [input_file...]
Compiles the given Solidity input files (or the standard input if none given or
"-" is used as a file name) and outputs the components specified in the options
at standard output or in files in the output directory, if specified.
Imports are automatically read from the filesystem, but it is also possible to
remap paths using the context:prefix=path syntax.
Example:
    solc --bin -o /tmp/solcoutput dapp-bin=/usr/local/lib/dapp-bin contract.sol

Allowed options:
  --help               Show help message and exit.
  --version            Show version and exit.
  --license            Show licensing information and exit.
  --optimize           Enable bytecode optimizer.
  --optimize-runs n (=200)
                       Estimated number of contract runs for optimizer tuning.
  --add-std            Add standard contracts.
  --libraries libs     Direct string or file containing library addresses. 
                       Syntax: <libraryName>: <address> [, or whitespace] ...
                       Address is interpreted as a hex string optionally 
                       prefixed by 0x.
  -o [ --output-dir ] path
                       If given, creates one file per component and 
                       contract/file at the specified directory.
  --overwrite          Overwrite existing files (used together with -o).
  --combined-json abi,asm,ast,bin,bin-runtime,clone-bin,compact-format,devdoc,hashes,interface,metadata,opcodes,srcmap,srcmap-runtime,userdoc
                       Output a single json document containing the specified 
                       information.
  --gas                Print an estimate of the maximal gas usage for each 
                       function.
  --standard-json      Switch to Standard JSON input / output mode, ignoring 
                       all options. It reads from standard input and provides 
                       the result on the standard output.
  --assemble           Switch to assembly mode, ignoring all options except 
                       --machine and assumes input is assembly.
  --julia              Switch to JULIA mode, ignoring all options except 
                       --machine and assumes input is JULIA.
  --machine evm,evm15,ewasm
                       Target machine in assembly or JULIA mode.
  --link               Switch to linker mode, ignoring all options apart from 
                       --libraries and modify binaries in place.
  --metadata-literal   Store referenced sources are literal data in the 
                       metadata output.
  --allow-paths path(s)
                       Allow a given path for imports. A list of paths can be 
                       supplied by separating them with a comma.

Output Components:
  --ast                AST of all source files.
  --ast-json           AST of all source files in JSON format.
  --ast-compact-json   AST of all source files in a compact JSON format.
  --asm                EVM assembly of the contracts.
  --asm-json           EVM assembly of the contracts in JSON format.
  --opcodes            Opcodes of the contracts.
  --bin                Binary of the contracts in hex.
  --bin-runtime        Binary of the runtime part of the contracts in hex.
  --clone-bin          Binary of the clone contracts in hex.
  --abi                ABI specification of the contracts.
  --hashes             Function signature hashes of the contracts.
  --userdoc            Natspec user documentation of all contracts.
  --devdoc             Natspec developer documentation of all contracts.
  --metadata           Combined Metadata JSON whose Swarm hash is stored 
                       on-chain.
  --formal             Translated source suitable for formal analysis.
	
```
* https://ethereum.stackexchange.com/questions/15435/how-to-compile-solidity-contracts-with-geth-v1-6
