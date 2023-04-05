# Automated market makers

> AMM's are the technology behind liquidity pools that allow assets to be traded 24/7 and keep the defi ecosystem liquid.

AMM's are currently unique to Ethereum

*From gemini.com:*

- "*Automated market makers (AMMs) are part of the decentralized finance (DeFi) ecosystem. They allow digital assets to be traded in a permissionless and automatic way by using liquidity pools rather than a traditional market of buyers and sellers. AMM users supply liquidity pools with crypto tokens, whose prices are determined by a constant mathematical formula. Liquidity pools can be optimized for different purposes, and are proving to be an important instrument in the DeFi ecosystem*"

For context, on a traditional exchange, buyers and sellers create an offer of an asset and the price they are willing to buy/sell at. AMM is different and does not rely on the interaction between buyers and sellers. Not one entity controls the system and anyone can build new solutions and participate, keeping the economy alive. At its core, its just a shared pot of tokens.

> *Liquidity: how fast an asset can be sold for cash without affecting the market price*

*from gemini.com*

<h5>Liquidity Provider (LP)

*In the crypto ecosystem, a liquidity provider (LP) is a user who deposits tokens into a liquidity pool. In return for supplying liquidity, users are typically awarded LP tokens that represent the share of the liquidity pool the user owns. These users are usually incentivized to hold LP tokens to receive a percentage of trading fees and other crypto rewards.*

- The price of the liquidity tokens is determined by a certain mathematical formula. By changing the formula, liquidity pools are able to be optimized for different purposes.

> Anyone who owns of any ERC-20 token can become a liquidity provider by supplying to a given AMM liqudity pool. Providers normally earn fees for giving the token.

<h4> Mathematical formula

> tokenA_balance(p) * tokenB_balance(p) = k

or as generalized by Uniswap: 

> x * y = k

The constant k means that there is a constant balance of assets that determines the price of tokens in a pool. This means that if an AMM has ether and bitcoin, every time ETH is bought, the price of ETH goes up, as there is an inbalance. Conversely, the BTC price goes down as there is more of it.
