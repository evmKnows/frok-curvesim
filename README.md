[![image](https://img.shields.io/pypi/v/curvesim.svg)](https://pypi.org/project/curvesim/)
[![image](https://img.shields.io/pypi/l/curvesim.svg)](https://pypi.org/project/curvesim/)
[![image](https://img.shields.io/pypi/pyversions/curvesim.svg)](https://pypi.org/project/curvesim/)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![CI](https://github.com/curveresearch/curvesim/actions/workflows/CI.yml/badge.svg)](https://github.com/curveresearch/curvesim/actions/workflows/CI.yml)
[![Docs](https://readthedocs.org/projects/curvesim/badge/?version=latest)](https://curvesim.readthedocs.io/en/latest)
![badge](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/chanhosuh/3da3c072e081f4509ebdd09c63e6ede5/raw/curvesim_coverage_badge.json)


# Curvesim
Curvesim simulates Curve pools with optimal arbitrageurs trading against them to determine reasonable risk and reward parameters, such as amplitude (A) and fee, given historical price and volume feeds.

Users can re-use simulation components to simulate custom strategies and generate custom metrics.  Pool objects enable simpler integration with Curve pools for both manual and automated analytics usage.

### Forked from https://github.com/curveresearch/curvesim
### Shoutout to [@nagakingg](https://github.com/nagakingg) for helping us tune the parameters for the Curve V2 AMM

# Curve v2 Pool Parameter Selection for frok.ai

## Introduction

As part of the frok.ai project, we are exploring the deployment of liquidity into a Curve v2 pool. To ensure optimal pool performance, it is crucial to select appropriate parameters based on the expected price dynamics of the assets we intend to include.

We analyzed three assets: FET, AIOZ, and PEPE. These assets were chosen as they represent the types of assets under consideration for our pool - specifically, volatile cryptocurrencies exhibiting a combination of choppy and trending price action. By understanding the optimal parameters for these assets, we can make a well-informed decision about the parameters to use for our pool.

## Methodology

To determine the optimal parameters, we conducted simulations using historical price and volume data for each asset. The simulations were performed using the Curvesim library, which enables testing of different parameter configurations and measurement of their impact on key metrics such as liquidity density and average fee.

The key parameters we focused on were:

- Out fee: The maximum fee charged when the pool is fully imbalanced
- Gamma: Controls the overall breadth of the bonding curve
- Fee_gamma: Controls how quickly fees increase with greater imbalance

For each asset, we ran simulations across a range of values for these parameters. The results were then analyzed to identify the parameter set that maximized liquidity density.

## Results

The simulation results for each asset can be found in the following files:

- ## [AIOZ Results](results/html/aioz_summary_grids.html)
  ![AIOZ Results](/results/images/aioz-results.png)

- ## [FET Results](results/html/fet_summary_grids.html)
  ![FET Results](/results/images/fet-results.png)

- ## [PEPE Results](results/html/pepe_summary_grids.html)
  ![PEPE Results](/results/images/pepe-results.png)

Across all three assets, we observed fairly consistent optimal parameters:

- During choppy or flat periods, the pool achieved 8-20x the liquidity density of a comparable Uniswap v2 pool, with fees around 25-50 bps
- During periods of rapid price change, the pool's liquidity density was equivalent to a Uniswap v2 pool, with fees up to 100 bps
- The average fee across all periods was around 60-70 bps

The results for FET and PEPE were particularly similar, likely due to the comparable mix of choppy and trending price action in their historical data. The optimal parameters for FET also fell within the range of good values for AIOZ, although the pattern was less identical.

## Recommendation

Based on these results, we recommend using the following parameters for our Curve v2 pool:

- Out fee: 1
- Gamma: 0.00003
- Fee_gamma: 0.0000086

These values are based on the parameters that maximized liquidity density for FET, as they also performed well for PEPE and AIOZ.

By using these parameters, we anticipate our pool to provide:

- Significantly higher capital efficiency than a constant product pool during flat markets
- Comparable efficiency to a constant product pool during rapidly moving markets
- Fees that automatically adjust based on market conditions, optimizing revenue for LPs
