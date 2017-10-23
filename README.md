# initial_coin-offering

## Overview

Let's build our own smart contract using Ethereum! This will be a simple code using [OpenZeppelin](https://openzeppelin.org/) and [Truffle](http://truffleframework.com/) that creates a smart contract ICO for DemoCoin on a testchain (called RPC) of the [Ethereum](https://www.ethereum.org/) blockchain.

## Steps

In terminal run these commands in order

```
$ npm install -g ethereumjs-testrpc
$ npm install -g truffle
$ mkdir my-ico && cd my-ico
$ truffle init
$ npm install zeppelin-solidity
```

Install npm [here](https://www.npmjs.com/) if you don't have it already.

Then run the test chain & configure Truffle
```
$ testrpc -u 0
$ truffle compile
$ truffle migrate
$ truffle console
```

Then buy some DemoCoin via the Truffle Console

```
// The account that will buy DEM tokens.
> account1 = web3.eth.accounts[1]
'0xddac5d057c79facd674bc95dfd9104076fd34d6b'
// The address of the DEM token instance that was created when the crowdsale contract was deployed
// assign the result of DemoCoinCrowdsale.deployed() to the variable crowdsale
> DemoCoinCrowdsale.deployed().then(inst => { crowdsale = inst })
> undefined
> crowdsale.token().then(addr => { tokenAddress = addr } )
> tokenAddress
'0x87a784686ef69304ac0cb1fcb845e03c82f4ce16'
> demoCoinInstance = DemoCoin.at(tokenAddress)
Now check the number of DEM tokens account1 has. It should have 0
demoCoinInstance.balanceOf(account1).then(balance => balance.toString(10))
'0'
// Buying DEM tokens
> DemoCoinCrowdsale.deployed().then(inst => inst.sendTransaction({ from: account1, value: web3.toWei(5, "ether")}))
{ tx: '0x68aa48e1f0d0248835378caa1e5b2051be35a5ff1ded82878683e6072c0a0cfc',
  receipt:
   { transactionHash: '0x68aa48e1f0d0248835378caa1e5b2051be35a5ff1ded82878683e6072c0a0cfc',
     transactionIndex: 0,
     blockHash: '0xb48ceed99cf6ddd4f081a99474113c4c16ecf61f76625a6559f1686698ee7d57',
     blockNumber: 5,
     gasUsed: 68738,
     cumulativeGasUsed: 68738,
     contractAddress: null,
     logs: [] },
  logs: [] }
undefined
// Check the amount of DEM tokens for account1 again. It should have some now.
> demoCoinInstance.balanceOf(account1).then(balance => account1DemTokenBalance = balance.toString(10))
'5000000000000000000000'
// When we created our token we made it with 18 decimals, which the same as what ether has. That's a lot of zeros, let's display without the decimals:
> web3.fromWei(account1DemTokenBalance, "ether")
'5000'
```
