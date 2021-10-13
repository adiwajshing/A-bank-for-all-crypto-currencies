# A bank for all crypto currencies
In the previous Quest ()  we looked at how to create a bank account that actually allows you to earn an interest. You could deposit Ethers into the smart bank account and after some time withdraw it along with an additional interest rate. 

This smart bank account, though completely functional, allows us to deposit only Ethers. In this quest we’ll explore how you can deposit one currency and withdraw another. We will also understand how the crypto currencies actually work and why they are a big deal.
## Getting some crypto currency other than ETH
You might have heard of many crypto currencies like Ethers, Bitcoin, Litecoin and many more. There are a lot of crypto currencies out there. However, there is a class of crypto currencies that are built on the ethereum network. These are called ERC20 tokens. Bitcoin is not an ERC20 token. Some examples of ERC20 tokens are DAI, MATIC.

An ERC20 Token is nothing but an implementation of a standard crypto currency smart contract. In this quest we will be looking at a crypto token called DAI, one of the most popular ERC20 tokens.

First, let’s look at what a smart contract for a crypto currency (ERC20) looks like.

[https://ropsten.etherscan.io/address/0xad6d458402f60fd3bd25163575031acdce07538d\#code](https://ropsten.etherscan.io/address/0xad6d458402f60fd3bd25163575031acdce07538d#code)

This is the link to the contract of the DAI token’s code.

I want you to look at the lines 137 - 167. That’s about 30 lines of code. Just a few lines of code that powers an entire crypto currency. DAI, a crypto currency handled by a ERC20 token contract, with a few lines of code has a market cap of more than $5B at the time of this writing!

![abcd](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/dcc9b6b1-e38c-4245-a5c4-02ae6600990a.jpg)

I recommend you to read the lines 120-167, which is the meat of the contract. You should be able to follow what is happening there. We’ve written similar code already in our smart contract, if not more complex!

You’ll see a contract called StandardToken that extends/implements other contracts or interfaces like BasicToken and ERC20 interface that have been declared in the same file.
## Let’s get some dai
We can exchange one crypto currency to another using what are called exchanges. There are many exchanges out there. Some of the popular ones are Uniswap, Coinbase and Binance. 

We will be using Uniswap here because it is a decentralized exchange. Meaning, unlike Coinbase and Binance, there is no company that is running this exchange. It is completely run by a collection of a few smart contracts. 

We already own one cryptocurrency that is ETH, let’s convert that into another crypto currency called DAI.

Head over to uniswap website 

[https://app.uniswap.org/\#/swap](https://app.uniswap.org/#/swap)

In the first dropdown, select ETH and in the second select DAI. We will be giving ETH and receiving DAI in return. 

![](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/7a7bbbe4-d3e8-4011-90e9-d56ad97e8fee.jpg)

Hit Swap.

Your meta mask will open up. Make sure you are on the Ropsten network when Metamask opens up. Confirm the transaction

Once the transaction is complete, you’ll see something like this. 

![](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/e6bc51c4-5319-479f-a54c-94d751c86bea.jpg)

We need to add DAI to metamask. What this means is that Metamask doesn’t know or care about all the crypto currencies that exist. You and I can launch a crypto currency right now with 30 lines of code that we saw. Anybody can create a contract and start a crypto currency.

If the currency adheres to the ERC20 standards, Metamask will be able to identify it and you’ll be able to manage your coins from the same metamask wallet.

![](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/89c6cf08-b38e-417e-8008-8b5ea554836e.jpg)

Select Add token.

Now that the token is added, you’ll be able to your DAI balance in your Metamask wallet under the tab “Assets”

![](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/9604db3a-7769-4622-b635-dd85939f2e20.jpg)

Just like you own ETH, now you also own DAI!
## Accepting ERC20 tokens (DAI etc) into our bank
Now that we have DAI in our wallet, let’s try to transfer those DAI to our smart bank account.

How do we accept DAI?

We know the DAI Erc20 token is just a smart contract. We have already seen how to add balance by sending ETH to our smart account. We use the payable key word.

However, payable allows only ETH to be sent to the function. We cannot send an ERC20 token like DAI to a payable function. We’ll have to do something else. 

The reason we can’t send an ERC20 token to a function just like ETH is because ETH is the native currency of Ethereum. So Solidity natively supports ETH. However, DAI and other ERC20 tokens are mere smart contracts. We can only interact with the smart contract just like how we interacted with the compound smart contract in the previous quest.

So, let’s do just that. Like what we had done in Compound example, we’ll have to define the interface and initialize an object with the appropriate smart contract address.

![](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/e211a070-4ca4-493c-b1ff-3a98fa3b5650.jpg)

![](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/237d4ddf-0910-4d64-9fe3-b47e3161e665.jpg)

Explore the code of the DAI ERC20 token on Etherscan. Copy paste the signatures of the functions we’d want to use into the IERC20 interface. Then we will initialize the IERC20 object. But we want to accept ANY ERC20 token, not just DAI. We’ll use a variable address when we initialize the ERC20 token object. So when we need to do a transaction with DAI, we’ll use DAI’s erc20 compliant smart contract address; when we want to use MATIC we’ll use MATIC’s erc20 compliant smart contract address.

[https://remix.ethereum.org/\#version=soljson-v0.8.4\+commit.c7e474f2.js&optimize=false&runs=200&gist=01e17eef46738e810a3dccf709a0f69c](https://remix.ethereum.org/#version=soljson-v0.8.4+commit.c7e474f2.js&optimize=false&runs=200&gist=01e17eef46738e810a3dccf709a0f69c)

Here are the steps we need to do,  intuitively 

1. We need to transfer ERC20 tokens to the Smart contract
2. The smart contract will convert the ERC20 tokens into ETH using Uniswap
3. We’ll deposit the ETH to Compound to receive interest
## A new way to transfer Tokens
The first step is to send tokens along with a function call. 

There are two ways to transfer an erc20 token.

The first is to use the transfer() function.Let us check the parameters of this function call. It is “To” and “amount”. So we’ll have to set the address of this smart bank account contract, and amount as the number of erc20 tokens we want to send. 

That will work perfectly fine, but where’s the function call? What what code should get executed immediately after this transfer? Immediately after the transfer we want to run some code. I.e. to convert it into ETH using Uniswap and then deposit to Compound. 

There is another way to transfer money. But this is a two step process. The first step is to call the approve() function. The owner of the ERC20 tokens tells Ethereum that they are approving a certain user/smart contract to use a portion of tokens they hold. 

So a user who wants to send money to a function call they will say, “Hey function, I’ve already approved you to spend 10 DAI from my account - go ahead and use it and execute whatever logic you want to execute there after”. 

The second step after the function has been called is to use the transferFrom() function. The transferFrom function transfers the function from the smart bank account contract. Security is taken care of by the ERC20 token smart contract. Approve can be executed only by the owner of the tokens. The transferFrom can be executed only by the user/contract that has been given the approval (aka allowance).

The “approval” will have to happen outside the smart contract. Until the approval is granted, the smart contract cannot do anything with the users tokens. We cannot convert them to ETH using uniswap in our contract, we can’t deposit them to Compound etc.

We will transfer all the money the user has approved into the account of our smart contract. We will then send to the uniswap smart contract for converting to ETH. 

![](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/1e3ce78b-f2a8-456d-9648-50c3796c0de1.jpg)

Also add a function to get the erc20 token balance of our smart contract, so that we can do a sanity check that everything is working. 

![](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/6ffd7a80-533b-4692-8544-330e33385c3c.jpg)

[https://remix.ethereum.org/\#version=soljson-v0.8.4\+commit.c7e474f2.js&optimize=false&runs=200&gist=1352efedf740d64ad494a3046fd198dd&evmVersion=null](https://remix.ethereum.org/#version=soljson-v0.8.4+commit.c7e474f2.js&optimize=false&runs=200&gist=1352efedf740d64ad494a3046fd198dd&evmVersion=null)

Deploy this on Ropsten and keep the contract address of this smart bank account contract handy. Make sure when you are deploying you select “Smart Bank account” in the dropdown contract, if it’s not already selected.

![](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/8fb111ba-c071-4e6b-a661-a2aff9e0191f.jpg)
## approving transfer of DAI
Now the first step is to approve the transfer of DAI. But this has to happen not on our smart contract but on the smart contract of DAI. We need to interact with the DAI smart contract directly.

To do that we’ll use a tool called [https://ethcontract.app](https://ethcontract.app) 

To use a contract you need two things. First is the address of the contract itself. The second is what is called the ABI. ABI is like an API specification for the web3 world. The ABI tells us what are the functions that are callable in this particular contract. 

So let’s find the address of the smart contract of DAI. We will use the ropsten network. Remember the addresses of smart contracts differ between testnet and mainnet.

A simple google search “ropsten dai etherscan” will give you the contract address on etherscan.

Here’s a direct link : [https://ropsten.etherscan.io/address/0xad6d458402f60fd3bd25163575031acdce07538d\#code](https://ropsten.etherscan.io/address/0xad6d458402f60fd3bd25163575031acdce07538d#code)

Copy the address from the top of the page. 

Then go to the Contract tab and look for ABI. Copy that too. 

Paste the Contract Address and the ABI in the appropriate boxes on [https://ethcontract.app](https://ethcontract.app)

![](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/9006f2bc-bbb7-47d1-8c90-6184cea6cf5b.jpg)

![](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/6f52fa83-22c1-4f01-adc6-2cf2a0e70ae7.jpg)

Then hit explore contract.

You will see all the functions that can be called in this contract. 

![](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/d168154c-d4da-4fa3-93d9-cd2f58b11c3a.jpg)

Direct Link : [https://tinyurl.com/ropstendai](https://tinyurl.com/ropstendai)

Look for “approve”

We need to approve our smart bank account contract to use DAI from our metamask account.

Open the approve function and enter the spender to be our smart contract address. We would have received this address after deploying our smart contract in one of the previous subquest. 

Under value, set the number of DAI you want to send. Make sure you open metamask and check how many tokens you actually have before you try approving them. You can approve only as many DAI as you own in your wallet, otherwise you’ll get an error.

Now you have approved our smart contract to use a certain number of DAI, you can go to the deployed contract and check what is the allowance that our contract has the right to from your wallet.

In the parameter give the Smart Contract of DAI ERC20 Contract address. Because we need to check how many DAI does our smart contract have allowance for.

![](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/f937fdeb-24fe-4ef2-b437-897b16bdb243.jpg)

Now that we’ve approved the smart contract to do whatever it wants with the DAIs that we have approved, let us write code so that we 

1. Transfer all the approved DAI into the account of our smart contract, from the metamask account
2. Send those to Uniswap to change to ETH
3. Send the ETH to Compound to start earning interest
## sending DAI to Uniswap
Now that we have transferred the DAIs from the user (msg.sender)’s account to the smart contract’s account using transferFrom(), we can now start looking at exchanging it for ETH before we can send it to compound.

For this we’ll integrate Uniswap, which is also a smart contract. 

Search for “Uniswap Router v2 Ropsten Etherscan”

[https://ropsten.etherscan.io/address/0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D\#code](https://ropsten.etherscan.io/address/0x681a4164703351d6aceba9d7038b573b444d3353#code)

Direct link ^

If we want to use a smart contract’s functions, we need their address and an interface in which we define what functions we want to call.

We’ll use only two functions here to convert erc20 tokens to eth and eth to erc20 tokens.

They are called 

swapExactETHForTokens (to convert eth to erc20 tokens)

swapExactTokensForETH (to convert erc20 tokens to eth)

Let’s as always, define the interface with the two functions above and initialize an object with the Uniswap Router contract address. You can look for the signatures of these two functions in the Contract tab on the above etherscan link.

![](https://qb-content-production.s3.ap-south-1.amazonaws.com/public/78dcb276-2464-4575-b984-756b538f3921/3254b72b-c5cf-460c-81a8-d6729922f4aa.jpg)



Let’s now go to addBalance function and add the code for converting erc20 tokens into eth. I.e. invoke swapExactTokensForETH with the right set of parameters.. 

It has a few parameters

- AmountIn : how many erc20 tokens we want to convert to eth
- AmountOutMin : How many eth do you expect at the end of this transaction? We can set this to be 0 for now.
- Path : array of addresses of tokens involved in transaction, one thing to keep in mind here is that pools for each consecutive pair of addresses must exist in uniswap ropsten network and have liquidity.   
To verify this you can go to uniswap.exchange and try swapping the tokens you are trying to swap from the contract.
- To : whom should the eth be transferred to? This will be the address of our smart contract
- Deadline: Uniswap will try to make this swap asap, but might get delayed, till when are you willing to wait for this swap to happen?

But before we ask Uniswap to convert DAI to ETH, we need to give approval to Uniswap so that it can do it’s thing with the DAI that is currently held by our contract. 

The user gave us permission to use the DAI from their metamask account. We used that approval to transfer the money into our smart contract’s account. Now we need to give approval from the smart contract who is now the owner of those DAI, to Uniswap so that it can do the appropriate computation - take DAI from us and transfer ETH to the account of our smart contract.

[https://remix.ethereum.org/\#version=soljson-v0.8.4\+commit.c7e474f2.js&optimize=false&runs=200&gist=23ea620edf730800148656d48171c5fc&evmVersion=null](https://remix.ethereum.org/#version=soljson-v0.8.4+commit.c7e474f2.js&optimize=false&runs=200&gist=23ea620edf730800148656d48171c5fc&evmVersion=null)

Make sure you have approved the DAI using ethcontract before interacting with the contract you have just deployed. Each time you deploy your contract, the address will change. You will need to approve the latest contract address using ethcontract.
## Try it on your own
Now that we’ve included Uniswap and converted DAI to eth can you :

1. Fill in the rest of the logic to send the eth to Compound in addBalance and store the number of cETH tokens 
2. We’ve not touched the withdraw function. Can you allow users to choose the currency in which they want to withdraw money too?
