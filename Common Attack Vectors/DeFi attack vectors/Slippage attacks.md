# Slippage attacks

Here we will discuss what is slippage and the different attacks that can occur when adequate measures are not taken

Firstly, let's talk about Slippage.
-
Slippage is the difference between the expected price of a trade and the price at which the trade is executed.

When it occurs?
-
Mostly in periods of high **volatility**

More formal description is:
> Slippage occurs when the bid/ask spread changes between the time a market order is requested and the time an exchange or other market-maker executes the order

So now we know what Slippage is, let's observe some attacks/errors related to it:

Lack of slippage parameter from user/Incorrect parameter
-

If there is a lack of the minimum amount of tokens you want to be returned from a swap, there is a potential sandwich attack.

*what is a asandwich attack*

Let's see an example:
```
function _savvyDeposit(address token , uint256 amount) internal {
    ISavvyPositionManager(savvy).depositBaseToken(token, amount, address(this), 0);
}
```

As the description of the issue states: If the buffer (SavvySage) is called during and unfavorable time, then
a large portion of deposited funds may be lost due to slippage because
deposit is called with 0 as the minimum out allowing any level of slippage

And here is the **proof of concept**:
-
Imagine that the market is having a huge volatility currently that you call the `deposit` function, It will call the `depositBaseToken` function with a 0 in the slippage parameter. Due to these conditions, your amount
deposited on the position manager will be very different from the amount that you introduced as an argument when you were calling `_savvyDeposit` and there is no way to control that from the user side.

Recommended Mitigation Steps
-

Implementing minimum return amount checks.
Adding a parameter that can be chosen by the transaction sender, then check that the actually received amount is above this parameter.

Let's look at another example:
```
function _swapLidoForWETH(uint256 amountToSwap) internal {
    IUniswapSwapRouter.ExactInputSingleParams
        memory params = IUniswapSwapRouter.ExactInputSingleParams({
            tokenIn: address(ldo),
            tokenOut: address(weth),
            fee: UNISWAP_FEE,
            recipient: address(this),
            deadline: block.timestamp,
            amountIn: amountToSwap,
            amountOutMinimum: 0,
            sqrtPriceLimitX96: 0
        });
    uniswapRouter.exactInputSingle(params);
}
```

The swap is called with `amountOutMinimum: 0`, meaning that there is no slippage protection in this swap. This is again another example of code vulnerable to sandwich attacks that can be executed by MEV bots.

Protection
-

And again if you want to protect from these issues consider implementing slippage parameters into the swaps.

In this category also falls [this issue](https://code4rena.com/reports/2022-01-trader-joe/#m-09-createpair-expects-zero-slippage) where the function requires zero slippage because is likely to revert

Here are some examples that you can look at:
*The examples are ordered by date*

[II BadgerDAO Zaps contest 2021-01-05](https://code4rena.com/reports/2021-11-badgerzaps/#m-05-no-slippage-control-on-deposit-of-ibbtcvaultzapsol)

[I BadgerDAO Zaps contest 2021-01-05](https://code4rena.com/reports/2021-11-badgerzaps/#m-01-improper-implementation-of-slippage-check)

[Spartan Protocol 2021-09-16](https://code4rena.com/reports/2021-07-spartan/#h-07-missing-slippage-checks)

[I Mochi contest 2021-11-23](https://code4rena.com/reports/2021-10-mochi/#h-12-feepool-is-vulnerable-to-sandwich-attack)

[II Mochi contest 2021-11-23](https://code4rena.com/reports/2021-10-mochi/#m-02-regerralfeepool-is-vulnerable-to-mev-searcher)

[Vader Protocol 2022-02-10](https://code4rena.com/reports/2021-11-vader/#h-31-unused-slippage-params)

[Malt Finance 2022-02-14](https://code4rena.com/reports/2021-11-malt/#m-02-frontrunning-in-uniswaphandler-calls-to-uniswapv2router)

[Notional contest 2022-03-10](https://code4rena.com/reports/2022-01-notional/#m-02-snotesol_mintfromassets-lack-of-slippage-control)

[Behodler contest 2022-03-24](https://code4rena.com/reports/2022-01-behodler/#m-14-uniswaphelperbuyflanandburn-is-a-subject-to-sandwich-attacks)

[Vader Protocol 2022-05-03](https://code4rena.com/reports/2021-12-vader/#m-01-vaderpoolv2mintfungible-exposes-users-to-unlimited-slippage)

[Backd contest 2022-06-02](https://code4rena.com/reports/2022-04-backd/#m-04-cvxcrvrewardslocker-implements-a-swap-without-a-slippage-check-that-can-result-in-a-loss-of-funds-through-mev)

[Alchemix contest 2022-07-18](https://code4rena.com/reports/2022-05-alchemix/#m-04-yearntokenadapter-allows-a-maximum-loss-of-100-when-withdrawing)

[Rubicon contest 2022-08-01](https://code4rena.com/reports/2022-05-rubicon/#h-07-rubiconrouterswapentirebalance-doesnt-handle-the-slippage-check-properly)

[Badger-Vested-Aura 2022-08-02](https://code4rena.com/reports/2022-06-badger/#m-01-_harvest-has-no-slippage-protection-when-swapping-aurabal-for-aura--)

[Illuminate contest 2022-08-23](https://code4rena.com/reports/2022-06-illuminate/#m-12-sandwich-attacks-are-possible-as-there-is-no-slippage-control-option-in-marketplace-and-in-lender-yield-swaps)


[Connext Amarok 2022-10-17](https://code4rena.com/reports/2022-06-connext/#m-20-in-reimburseliquidityfees-of-sponservault-contract-swaps-tokens-without-slippage-limit-so-its-possible-to-perform-sandwich-attack-and-it-create-mev)

[Derby 2023-01](https://github.com/sherlock-audit/2023-01-derby-judging/issues/107)

[UXD 2023-01](https://github.com/sherlock-audit/2023-01-uxd-judging/issues/429)

[I GMX 2023-02](https://github.com/sherlock-audit/2023-02-gmx-judging/issues/138),
[II GMX 2023-02](https://github.com/sherlock-audit/2023-02-gmx-judging/issues/144),
[III GMX 2023-02](https://github.com/sherlock-audit/2023-02-gmx-judging/issues/159)

[Blueberry 2023-02](https://github.com/sherlock-audit/2023-02-blueberry-judging/issues/130)

[I Blueberry 2023-04](https://github.com/sherlock-audit/2023-04-blueberry-judging/issues/121),
[II Blueberry 2023-04](https://github.com/sherlock-audit/2023-04-blueberry-judging/issues/124)

Bare in mind that some of the examples require more effort to be grasped so don't get intimidated if you don't get them instantaneously


Lack of expiration deadline
-

We've already seen the the potential devastating effects of not letting the users specify a slippage parameter on their own

Another problem is not allowing users to set deadlines.

The lack of deadline can create critical loss of funds for any user initiating a swap

Let's look at an example 

Let us look into the heavily forked Uniswap V2 contract addLiquidity function implementation:

```
// **** ADD LIQUIDITY ****
function _addLiquidity(
	address tokenA,
	address tokenB,
	uint amountADesired,
	uint amountBDesired,
	uint amountAMin,
	uint amountBMin
) internal virtual returns (uint amountA, uint amountB) {
	// create the pair if it doesn't exist yet
	if (IUniswapV2Factory(factory).getPair(tokenA, tokenB) == address(0)) {
		IUniswapV2Factory(factory).createPair(tokenA, tokenB);
	}
	(uint reserveA, uint reserveB) = UniswapV2Library.getReserves(factory, tokenA, tokenB);
	if (reserveA == 0 && reserveB == 0) {
		(amountA, amountB) = (amountADesired, amountBDesired);
	} else {
		uint amountBOptimal = UniswapV2Library.quote(amountADesired, reserveA, reserveB);
		if (amountBOptimal <= amountBDesired) {
			require(amountBOptimal >= amountBMin, 'UniswapV2Router: INSUFFICIENT_B_AMOUNT');
			(amountA, amountB) = (amountADesired, amountBOptimal);
		} else {
			uint amountAOptimal = UniswapV2Library.quote(amountBDesired, reserveB, reserveA);
			assert(amountAOptimal <= amountADesired);
			require(amountAOptimal >= amountAMin, 'UniswapV2Router: INSUFFICIENT_A_AMOUNT');
			(amountA, amountB) = (amountAOptimal, amountBDesired);
		}
	}
}

function addLiquidity(
	address tokenA,
	address tokenB,
	uint amountADesired,
	uint amountBDesired,
	uint amountAMin,
	uint amountBMin,
	address to,
	uint deadline
) external virtual override ensure(deadline) returns (uint amountA, uint amountB, uint liquidity) {
	(amountA, amountB) = _addLiquidity(tokenA, tokenB, amountADesired, amountBDesired, amountAMin, amountBMin);
	address pair = UniswapV2Library.pairFor(factory, tokenA, tokenB);
	TransferHelper.safeTransferFrom(tokenA, msg.sender, pair, amountA);
	TransferHelper.safeTransferFrom(tokenB, msg.sender, pair, amountB);
	liquidity = IUniswapV2Pair(pair).mint(to);
}
```
the implementation has two point that worth noting,

the first point is the deadline check

```
modifier ensure(uint deadline) {
	require(deadline >= block.timestamp, 'UniswapV2Router: EXPIRED');
	_;
}
```

The transaction can be pending in mempool for a long time and can be executed in a long time after the user submit the transaction.

The problem is `createMarket`, which calculates the length and `maxPayout` by `block.timestamp` inside it.

```
// Calculate market length and check time bounds
uint48 length = uint48(params_.conclusion - block.timestamp); \
if (
    length < minMarketDuration ||
    params_.depositInterval < minDepositInterval ||
    params_.depositInterval > length
) revert Auctioneer_InvalidParams();

// Calculate the maximum payout amount for this market, determined by deposit interval
uint256 capacity = params_.capacityInQuote
    ? params_.capacity.mulDiv(scale, price)
    : params_.capacity;
market.maxPayout = capacity.mulDiv(uint256(params_.depositInterval), uint256(length));
```

After the market is created at wrong time, user can call purchase.
At purchaseBond(),
```
// Payout for the deposit = amount / price
//
// where:
// payout = payout tokens out
// amount = quote tokens in
// price = quote tokens : payout token (i.e. 200 QUOTE : BASE), adjusted for scaling
payout = amount_.mulDiv(term.scale, price);

// Payout must be greater than user inputted minimum
if (payout < minAmountOut_) revert Auctioneer_AmountLessThanMinimum();

// Markets have a max payout amount, capping size because deposits
// do not experience slippage. max payout is recalculated upon tuning
if (payout > market.maxPayout) revert Auctioneer_MaxPayoutExceeded();
```
payout value is calculated by `term.scale` which the market owner has set assuming the market would be created at desired timestamp.
Even, `maxPayout` is far bigger than expected, as it is calculated by very small length.

Impact
-
Even though the market owner can close the market at any time, malicious user can attack before the market close and steal unexpectedly large amount of payout Tokens.

[Badger Vested Aura 2022-08-02](https://code4rena.com/reports/2022-06-badger/#m-01-_harvest-has-no-slippage-protection-when-swapping-aurabal-for-aura--)

[I Canto v2 contest 2022-10-18](https://code4rena.com/reports/2022-12-caviar/#m-01-missing-deadline-checks-allow-pending-transactions-to-be-maliciously-executed)

[II Canto v2 2022-10-18](https://code4rena.com/reports/2022-06-canto-v2/#m-01-stableswap---deadline-do-not-work)

[Papr contest 2023-01-31](https://code4rena.com/reports/2022-12-backed/#m-01-missing-deadline-checks-allow-pending-transactions-to-be-maliciously-executed)

[Caviar contest 2023-01-26](https://code4rena.com/reports/2022-12-caviar/#m-01-missing-deadline-checks-allow-pending-transactions-to-be-maliciously-executed)

Incorrect Slippage Calculation
-

Sometimes it happens that even though a function has slippage parameter, it's not properly calculated

The issue can arise simply from wrong value of a variable up to miscalculations when passing value to some parameter of a function.

Let's take a look:

In the [Rage Trade Contest 2022-10 in Sherlock](https://github.com/sherlock-audit/2022-10-rage-trade-judging/issues/39)

```
WithdrawPeriphery accidentally uses an incorrect value for MAX_BPS which will allow for much higher slippage than intended.

uint256 internal constant MAX_BPS = 1000;
```

BPS is typically 10,000 and using 1000 is inconsistent with the rest of the ecosystem contracts and tests. The result is that slippage values will be 10x higher than intended.

The result will be loss of funds of users, due to MEV.

Or let's see this example where one division `ERC5095.withdraw()/redeem()` from the [Illuminate 2023-01](https://github.com/sherlock-audit/2023-01-illuminate-judging/issues/16) adds extra underlying slippage on top of what the user requests

```
uint128 returned = IMarketPlace(marketplace).sellPrincipalToken(
    underlying,
    maturity,
    shares,
    Cast.u128(a - (a / 100))
);
```

At the end of withdrawal/redemption, the user will end up receiving less underlying than they asked for, due to slippage.


Other examples are:

[Vader Protocol 2021-06-14](https://code4rena.com/reports/2021-04-vader/#h-15-wrong-slippage-protection-on-token---token-trades)

[Redacted Cartel 2022-05-11](https://code4rena.com/reports/2022-02-redacted-cartel/#m-05-wrong-slippage-check)

[Notional 2023-02](https://github.com/sherlock-audit/2023-02-notional-judging/issues/10),
[Notional 2023-02](https://github.com/sherlock-audit/2023-02-notional-judging/issues/18)

[Blueberry 2023-04](https://github.com/sherlock-audit/2023-04-blueberry-judging/issues/131)

*Keep in mind that some protocols are using different precision values and in case of miscalculation the slippage parameter may be ineffective and lead to precision loss errors. So these kind of issues can also occur*

Hard-coded Slippage May Freeze User Funds
-

Some protocols decide to hard-code low slippage to protect users from being exploited by MEV. Although their their decision is well-intentioned, the result may lead to freezing users funds during periods of hugh volatility.

Let's see an example:
```
require(withdrawAmount >= _amount.percentMul(99_00), Errors.VT_WITHDRAW_AMOUNT_MISMATCH);
```

As you can see here it's set a hardcoded slippage control of 99%.

The **recommended Mitigation Steps** are the following:
There are different ways to set the slippage.

The first one is to let users determine the maximum slippage they’re willing to take.(we already discussed that)

```
require(withdrawAmount >= _minReceiveAmount, Errors.VT_WITHDRAW_AMOUNT_MISMATCH);
```
The second one is have a slippage control parameters that’s set by the operator.
```
uint256 receivedETHAmount = CurveswapAdapter.swapExactTokensForTokens(
    _addressesProvider,
    _addressesProvider.getAddress('STETH_ETH_POOL'),
    LIDO,
    ETH,
    yieldStETH,
    maxSlippage
);
```
```
function setMaxSlippage(uint256 _slippage) external onlyOperator {
    maxSlippage = _slippage;
    //@audit This action usually emit an event.
    emit SetMaxSlippage(msg.sender, slippage);
}
```

This example can be found in the [Sturdy contest 2022-06-29](https://code4rena.com/reports/2022-05-sturdy/#h-01-hard-coded-slippage-may-freeze-user-funds-during-market-turbulence)

Other examples are:
[Alchemix contest 2022-07-18](https://code4rena.com/reports/2022-05-alchemix/#m-13-transmuterbuffers-_alchemistwithdraw-use-hard-coded-slippage-that-can-lead-to-user-losses)

[Backd contest 2022-06-02](https://code4rena.com/reports/2022-04-backd/#m-11-position-owner-should-set-allowed-slippage)

Not checking the final amount
-

If you don't check the received amounts with the shared amounts, this can lead to loss of funds.

To explain what I mean, let's see an example from [Illuminate 2022-10](https://github.com/sherlock-audit/2022-10-illuminate-judging/issues/30)
Here the function deposit doesn't check if received shares is less then provided amount. In some cases this leads to lost of funds.
```
function deposit(address r, uint256 a) external override returns (uint256) {
    if (block.timestamp > maturity) {
        revert Exception(
            21,
            block.timestamp,
            maturity,
            address(0),
            address(0)
        );
    }
    uint128 shares = Cast.u128(previewDeposit(a));
    Safe.transferFrom(IERC20(underlying), msg.sender, address(this), a);
    // consider the hardcoded slippage limit, 4626 compliance requires no minimum param.
    uint128 returned = IMarketPlace(marketplace).sellUnderlying(
        underlying,
        maturity,
        Cast.u128(a),
        shares - (shares / 100)
    );
    _transfer(address(this), r, returned);
    return returned;
}
```

While calling market place, you can see that slippage of 1 percent is provided.

```
uint128 returned = IMarketPlace(marketplace).sellUnderlying(
            underlying,
            maturity,
            Cast.u128(a),
            shares - (shares / 100)
        );
```


But this is not enough in some cases.

For example we have `ERC5095` token with short maturity which provides `0.5%` of interests.
`userA` calls deposit function with `1000` as base amount. He wants to get back `1005` share tokens. And after maturity time earn `5` tokens on this trade.

But because of `slippage` set to `1%`, it's possible that the price will change and user will receive `995` share tokens instead of `1005`, which means that user has lost `5` base tokens.

Here is another example from [Blueberry contest 2023-04](https://github.com/sherlock-audit/2023-04-blueberry-judging/issues/126)


Minting/Burning exposes users to unlimited slippage
-

Protocols that allow users to transfer foreign tokens to native tokens to the protocol are "swapping" foreign tokens for native tokens (protocol's tokens). In other words a slippage parameter can be bypassed so that the user is exposed to unlimited slippage

To give you a perspective, here in the [Mute contest 2023-04-27](https://code4rena.com/reports/2023-03-mute/#h-02-attacker-can-front-run-bond-buyer-and-make-them-buy-it-for-a-lower-payout-than-expected)

The MuteBond contract contains a feature in which after each purchase the epochStart increases by 5% of the time passed since epochStart, this (in most cases) lowers the bond’s price (i.e. buyer gets less payout) for future purchases.

An attacker can exploit this feature to front-run a deposit/purchase tx and lower the victim’s payout.

And the result is that the user ends up buying bond for a lower payout than intended.

The **Recommended Mitigation Steps** are adding a minimal payout parameter so that users can specify the expected amount payout. And also it's important that the transaction **should revert** if the actual payout is lower than expected.

Here are examples with the code base of [Vader Protocol contest | 2022-02-10 | H-01](https://code4rena.com/reports/2021-11-vader/#h-01-minting-and-burning-synths-exposes-users-to-unlimited-slippage), [H-22](https://code4rena.com/reports/2021-11-vader/#h-22-mintsynth-and-burnsynth-can-be-front-run)