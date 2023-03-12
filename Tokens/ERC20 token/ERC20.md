# ERC20

ERC-20 is the technical standart for <b>fungible tokens</b> created using the Ethereum blockchain. A fungible token is interchangeable wth another token- where the well-known <b>non-fungible</b> tokens (NFTs) are not interchangeable.
-

ERC-20 allows developers to create smart-contract-enabled tokens that can be used with other products and services. These tokens are a representation of an:
1. asset
2. right
3. ownership
4. access
5. cryptocurrency
6. or anything else that is not unique in and of itself but can be transferred

ERC-20 History
-
👉There was a problem with <b>interoperability</b> Each token contract can be completely different from the other. So if you want your token to be avaible on an exchange, the exchange has to write custom code to talk to your contract and allow people to trade. <br>
<br>
👉The same thing goes for wallet providers, supporting hundreds of tokens will be very complex and very time consuming, so that's why the community proposed a standart called ERC-20.

ERC-20 Contents
-
ERC-20 is a list of functions and events that must be implemented into a token for it to be considered ERC-20 compliant. The functions are:

1. TotalSupply: The total ```number of tokens``` that will ever be issued
2. BalanceOf The account balance of a token ```owner's account```
3. Transfer: Automatically executes ```transfers``` of a specified ```number of tokens``` to a ```specified address``` for transactions using the token
4. TransferFrom: Automatically executes transfers of a ```specified number``` of tokens from a ```specified address``` using the token
5. Approve: Allows a ```spender``` to ```withdraw``` a ```set``` number of tokens from a ```specified account```, up to a ```specific amount```
6. Allowance: Returns a ```set``` number of tokens from a ```spender``` to the ```owner```

The events that must be included in the token are:

Transfer: An event triggered when a ```transfer``` is ```successful```
Approval: A log of an approved event

The following functions are optional and are not required to be included, but they enhance the token's usability:

1. Token's name (optional)
2. Its symbol (optional)
3. Decimal points to use (optional) 


❗Warning: <br>
👉"Token" and "Cryptocurrency" are often used interchangeably; all cryptocurrencies are tokens, but not all tokens are cryptocurrencies. Tokens often represent assets and rights that are external to a blockchain. Token, in the context of ```ERC-20``` compliance, simply means a blockchain representation of something that meets the standards set by the Ethereum community to be considered a smart contract standard-compliant token.

What Does ERC-20 Mean?
-
ERC-20 is Ethereum Request for Comment, number 20. ERC-20 is the standard for smart contract tokens created using Ethereum.

What Is an ERC-20 Wallet?
-
An ERC-20 wallet is a wallet that lets you manage ERC-20 compliant tokens