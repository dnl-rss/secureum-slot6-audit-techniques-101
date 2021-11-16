The following assignment demonstrates the usage of slither on uniswap v3.

### Setup

First, one must install slither:

```sh
$ pip3 install --user slither-analyzer
```

Uniswap V3 is compiled with solc `version =0.7.6`. Therefore, we must install the `solc-select` tool, along with the specific solc version, and then set the compiler to that version.

```sh
$ pip3 install solc-select
$ solc-select install 0.7.6
$ solc-select use 0.7.6
```

Now, clone the Uniswap V3 repo:

```sh
git clone https://github.com/Uniswap/v3-core.git
cd v3-core
```

Run slither on the contracts folder:

```sh
slither ./contracts
```

### Results

`analyzed (97 contracts with 75 detectors), 522 result(s) found`

[results](./slither-results.txt) are stored for review

The findings :
- High Risk (4)
- Medium Risk (85)
- Low Risk (50)
- Informational (386)

#### Compiler Warnings

Several contracts trigger a compiler warding that their size exceeds 24576 bytes, and may not be deployable to mainnet. This is likely a false positive, as Uniswap V3 is indeed deployed on mainnet.

```
Compilation warnings/errors on ./UniswapV3Pool.sol:
Warning: Contract code size exceeds 24576 bytes (a limit introduced in Spurious Dragon). This contract may not be deployable on mainnet. Consider enabling the optimizer (with a low "runs" value!), turning off revert strings, or using libraries.
```

#### High Risk Findings

The 4 High Risk findings are actually only 2, because one of those findings was repeated thrice.

One of those findings relates the the `uninitialzied-state` of the UniswapV3Pool. This is finding is probably ignored because Uniswap, by design, does not initialize liquidity pools; its liquidity providers do.

```
UniswapV3Pool.observations (UniswapV3Pool.sol#99) is never initialized. It is used in:
	- UniswapV3Pool.snapshotCumulativesInside(int24,int24) (UniswapV3Pool.sol#158-233)
	- UniswapV3Pool.observe(uint32[]) (UniswapV3Pool.sol#236-252)
	- UniswapV3Pool.increaseObservationCardinalityNext(uint16) (UniswapV3Pool.sol#255-267)
	- UniswapV3Pool.initialize(uint160) (UniswapV3Pool.sol#271-289)
	- UniswapV3Pool._modifyPosition(UniswapV3Pool.ModifyPositionParams) (UniswapV3Pool.sol#306-372)
	- UniswapV3Pool._updatePosition(address,int24,int24,int128,int24) (UniswapV3Pool.sol#379-453)
	- UniswapV3Pool.swap(address,bool,int256,uint160,bytes) (UniswapV3Pool.sol#596-788)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#uninitialized-state-variables
```

The second finding is that a `public` function `enableFeeAmount` should be `external`, because it is never called internally. This may not affect the security posture of the contract, but is a gas optimization.

```
enableFeeAmount(uint24,int24) should be declared external:
	- UniswapV3Factory.enableFeeAmount(uint24,int24) (UniswapV3Factory.sol#61-72)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#public-function-that-could-be-declared-external
```

#### Medium Risk Findings

There are several math warnings, as multiplication is applied on the result of a division operation. However, these several of these warnings are for the included `FullMath` library, which is designed to handle such operations appropriately. `Oracle` and `Tick` operations should consider whether multiplication after division could produce unexpected behavior.

```
FullMath.mulDiv(uint256,uint256,uint256) (libraries/FullMath.sol#14-106) performs a multiplication on the result of a division:
	-denominator = denominator / twos (libraries/FullMath.sol#67)
	-inv = (3 * denominator) ^ 2 (libraries/FullMath.sol#87)

...

Oracle.observeSingle(Oracle.Observation[65535],uint32,uint32,int24,uint16,uint128,uint16) (libraries/Oracle.sol#245-287) performs a multiplication on the result of a division:
	-(beforeOrAt.tickCumulative + ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / observationTimeDelta) * targetDelta,beforeOrAt.secondsPerLiquidityCumulativeX128 + uint160((uint256(atOrAfter.secondsPerLiquidityCumulativeX128 - beforeOrAt.secondsPerLiquidityCumulativeX128) * targetDelta) / observationTimeDelta)) (libraries/Oracle.sol#275-285)
Tick.tickSpacingToMaxLiquidityPerTick(int24) (libraries/Tick.sol#44-49) performs a multiplication on the result of a division:
	-minTick = (TickMath.MIN_TICK / tickSpacing) * tickSpacing (libraries/Tick.sol#45)
Tick.tickSpacingToMaxLiquidityPerTick(int24) (libraries/Tick.sol#44-49) performs a multiplication on the result of a division:
	-maxTick = (TickMath.MAX_TICK / tickSpacing) * tickSpacing (libraries/Tick.sol#46)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#divide-before-multiply
```

There are several warnings about reentracy, indicating all external calls and state variable updates following the call. Developers/reviewers should check that each of these external calls is safe.

```
Reentrancy in UniswapV3Pool._modifyPosition(UniswapV3Pool.ModifyPositionParams) (UniswapV3Pool.sol#306-372):
	External calls:
	- position = _updatePosition(params.owner,params.tickLower,params.tickUpper,params.liquidityDelta,_slot0.tick) (UniswapV3Pool.sol#319-325)
		- (tickCumulative,secondsPerLiquidityCumulativeX128) = observations.observeSingle(time,0,slot0.tick,slot0.observationIndex,liquidity,slot0.observationCardinality) (UniswapV3Pool.sol#396-404)
	- (slot0.observationIndex,slot0.observationCardinality) = observations.write(_slot0.observationIndex,_blockTimestamp(),_slot0.tick,liquidityBefore,_slot0.observationCardinality,_slot0.observationCardinalityNext) (UniswapV3Pool.sol#341-348)
	State variables written after the call(s):
	- liquidity = LiquidityMath.addDelta(liquidityBefore,params.liquidityDelta) (UniswapV3Pool.sol#361)
	- (slot0.observationIndex,slot0.observationCardinality) = observations.write(_slot0.observationIndex,_blockTimestamp(),_slot0.tick,liquidityBefore,_slot0.observationCardinality,_slot0.observationCardinalityNext) (UniswapV3Pool.sol#341-348)

...

Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-1
```

There are several warnings about uninitialized local variables. The danger here is that the variable is silently initialized as zero, and then inadvertently used. Looking at the first of these messages the uninitialized boolean `flippedUpper` is silently set as `false`. The control flow logic of the function `_updatePosition` seems to be aware of this default `false` value for `flippedUpper`, unless its value is set to `true` by the function operations (via call to `ticks.update`). The recommended practice for such variables is to explicitly declare `bool flippedUpper = false;` to prevent mistakes from occurring.

```
UniswapV3Pool._updatePosition(address,int24,int24,int128,int24).flippedUpper (UniswapV3Pool.sol#393) is a local variable never initialized
UniswapV3Pool.mint(address,int24,int24,uint128,bytes).balance0Before (UniswapV3Pool.sol#478) is a local variable never initialized
...
TickBitmap.nextInitializedTickWithinOneWord(mapping(int16 => uint256),int24,int24,bool).bitPos_scope_1 (libraries/TickBitmap.sol#65) is a local variable never initialized
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#uninitialized-local-variables
```

```solidity
/// @dev Gets and updates a position with the given liquidity delta
/// @param owner the owner of the position
/// @param tickLower the lower tick of the position's tick range
/// @param tickUpper the upper tick of the position's tick range
/// @param tick the current tick, passed to avoid sloads
function _updatePosition(
    address owner,
    int24 tickLower,
    int24 tickUpper,
    int128 liquidityDelta,
    int24 tick
) private returns (Position.Info storage position) {
    position = positions.get(owner, tickLower, tickUpper);

    uint256 _feeGrowthGlobal0X128 = feeGrowthGlobal0X128; // SLOAD for gas optimization
    uint256 _feeGrowthGlobal1X128 = feeGrowthGlobal1X128; // SLOAD for gas optimization

    // if we need to update the ticks, do it
    bool flippedLower;
    bool flippedUpper;
    if (liquidityDelta != 0) {
        uint32 time = _blockTimestamp();
        (int56 tickCumulative, uint160 secondsPerLiquidityCumulativeX128) =
            observations.observeSingle(
                time,
                0,
                slot0.tick,
                slot0.observationIndex,
                liquidity,
                slot0.observationCardinality
            );

        flippedLower = ticks.update(
            tickLower,
            tick,
            liquidityDelta,
            _feeGrowthGlobal0X128,
            _feeGrowthGlobal1X128,
            secondsPerLiquidityCumulativeX128,
            tickCumulative,
            time,
            false,
            maxLiquidityPerTick
        );
        flippedUpper = ticks.update(
            tickUpper,
            tick,
            liquidityDelta,
            _feeGrowthGlobal0X128,
            _feeGrowthGlobal1X128,
            secondsPerLiquidityCumulativeX128,
            tickCumulative,
            time,
            true,
            maxLiquidityPerTick
        );

        if (flippedLower) {
            tickBitmap.flipTick(tickLower, tickSpacing);
        }
        if (flippedUpper) {
            tickBitmap.flipTick(tickUpper, tickSpacing);
        }
    }
```

### Low Severity Findings

`UniswapV3Pool.swap` makes external calls inside a loop. The dangers here is that one failed call can revert the entire loop.

```
UniswapV3Pool.swap(address,bool,int256,uint160,bytes) (UniswapV3Pool.sol#596-788) has external calls inside a loop: (cache.tickCumulative,cache.secondsPerLiquidityCumulativeX128) = observations.observeSingle(cache.blockTimestamp,0,slot0Start.tick,slot0Start.observationIndex,cache.liquidityStart,slot0Start.observationCardinality) (UniswapV3Pool.sol#699-706)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation/#calls-inside-a-loop
```

Several variables are potentially used before declaration. This would cause an error.

```
Variable 'UniswapV3Pool.swap(address,bool,int256,uint160,bytes).step (UniswapV3Pool.sol#642)' in UniswapV3Pool.swap(address,bool,int256,uint160,bytes) (UniswapV3Pool.sol#596-788) potentially used before declaration: state.amountSpecifiedRemaining != 0 && state.sqrtPriceX96 != sqrtPriceLimitX96 (UniswapV3Pool.sol#641)
Variable 'TickBitmap.nextInitializedTickWithinOneWord(mapping(int16 => uint256),int24,int24,bool).wordPos (libraries/TickBitmap.sol#52)' in TickBitmap.nextInitializedTickWithinOneWord(mapping(int16 => uint256),int24,int24,bool) (libraries/TickBitmap.sol#42-77) potentially used before declaration: (wordPos,bitPos) = position(compressed + 1) (libraries/TickBitmap.sol#65)
Variable 'TickBitmap.nextInitializedTickWithinOneWord(mapping(int16 => uint256),int24,int24,bool).bitPos (libraries/TickBitmap.sol#52)' in TickBitmap.nextInitializedTickWithinOneWord(mapping(int16 => uint256),int24,int24,bool) (libraries/TickBitmap.sol#42-77) potentially used before declaration: (wordPos,bitPos) = position(compressed + 1) (libraries/TickBitmap.sol#65)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#pre-declaration-usage-of-local-variables
```

There are more reentracy warnings, with lower confidence.

```
Reentrancy in UniswapV3Pool.flash(address,uint256,uint256,bytes) (UniswapV3Pool.sol#791-834):
	External calls:

  ...

	State variables written after the call(s):

  ...

...

Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-2
```

A third class of reentrancy warning warns of events emitted following an external call.

```
Reentrancy in UniswapV3Pool.burn(int24,int24,uint128) (UniswapV3Pool.sol#517-543):
	External calls:

  ...

	Event emitted after the call(s):

  ...

Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-3
```

Several operations utilized timestamp for comparisons, which is dangerous.

```
UniswapV3Pool.initialize(uint160) (UniswapV3Pool.sol#271-289) uses timestamp for comparisons
	Dangerous comparisons:
	- require(bool,string)(slot0.sqrtPriceX96 == 0,AI) (UniswapV3Pool.sol#272)

...

Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#block-timestamp
```

### Informational

- use of assembly
- multiple versions of solidity used by interfaces and libraries
- several library functions are never used
- older versions of solidity are allowed by interfaces and libraries
- low level calls
- variable names are too similar
- literals are used with too many digits
