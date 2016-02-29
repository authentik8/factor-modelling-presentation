# Factor Modelling and Benchmarking of Hedge Funds

## Introduction

Hedge fund returns are typically characterised in terms of:
- __Alpha__: Returns arising from the specific skill of the hedge fund manager
- __Beta__ : Returns arising from exposure to systematic risks (i.e. their 'Betas').

For this paper, Alpha is defined as:

> "_The part of the return that cannot be explained by exposure to systematic risk factors in global capital markets_".

It is not generally easy to separate out alpha and beta in an actively managed investment strategy, and for hedge funds it is also difficult to distinguish the two.

If a specific return is available to only a handful of investors and cannot be expressed systematically, this is most likely __alpha__.

Returns expressible in a systematic fashion but involving non-conventional techniques & instruments is possibly beta, termed __alternative beta__ to reflect the non-standard nature of the method used to obtain it.

----------------------------------------------------------

The ideal model sought in this paper is a _general equilibrium_ model relating HF returns to their _systematic exposures_ represented by directly observable prices in financial markets, similarly to __CAPM__ for equity markets.


## Factor Modelling

- Suggested by _W. Sharpe_ in 1992 in an attempt to describe active management strategies for equity mutual funds.

- Aims to characterise an active investment style as a _linear combination_ of a set of asset class _indices_.  
 This has the effect of reducing active investment strategy performance to a combination of passive strategies.

### Applied to Hedge Funds

Previous authors (Fung & Hsieh) produced a multi-factor regression of HF returns against 8 asset class indices:
 - US Equity
 - Non-US Equity
 - EM Equity
 - US Govt Bonds
 - Non-US Govt Bonds
 - 1M EuroDollar Deposit Rate
 - Gold
 - Trade-weighted USD value

The most simple HF factor model is of the form:

```maths
  HF Excess Return = α + Σ(β_i * F_i) + ε

  where:
    α   : HF Manager Alpha
    β_i : Coefficient of exposure to factor i
    F_i : Market factor i
```
Note that hedge fund alpha can only be __inferred__ by measuring and subtracting out the various betas multiplied by their respective factors from the returns.

As a result of this, alpha obtained __depends on the chosen risk factors__, and so can be fictively high if a relevant risk factor is excluded from the model, i.e. some of the alpha would in fact be unaccounted beta. Therefore, the model proposed earlier should in fact be:

```maths

  HF Excess Return = α + Σ(β_i * F_i) + Σ(β_j * F_j) + ε

  where:
    α   : HF Manager Alpha
    β_i : Coefficient of exposure to factor i
    F_i : Market factor i

    β_j : Coefficient of *unmodelled* factor j
    F_i : Un-modelled factor j

```

## Problems with HF Indices

Another significant problem associated with regression analysis of HF returns arises from the indices used for the analysis: hedge fund databases and their resultant indices are subject to a number of biases that skew returns & thus values obtained for alpha.

HFs typically don't publicise returns information, therefore industry analysis relies mostly on aggregate returns provided by a number of indices, differentiating hedge fund performance accross the various different strategy sectors. However, index returns depend highly on decisions by the provider such as asset weighting and fund selection, problems facing any index but exacerbated by the diverse, dynamic & opaque nature of the HF world.

An index for a more traditional asset class is constructed to be the return of the _market portfolio_, i.e. an asset-weighted combination of all investable assets in the class (or a proxy to it).

CAPM gives this to be the combination of assets with the optimal risk-return tradeoff in market equilibrium. As such, they capture a _clearly defined risk premium available to investors willing to expose themselves to the systematic risk of the asset class_, a model missing for the world of hedge funds.  
**CHECK THIS**

The standard way to construct HF index is to use the average performance of a set of managers, although these inherit a umber of problems present in the returns data stored in the databases underlying them:

### Survivorship

Unsuccessful managers will typically leave the industry, removing their fund from the representative index ex-post. Further exacerbating the problem, many databases purge information on funds that have ceased operation, resulting in upwards bias in performance data - surviving funds are likely to have outperformed the now-defunct ones.

Collectively, this bias is thought to contribute ***2-4%*** of upwards bias.

### Backfilling

Similar to survivorship, backfilling also introduces an upwards bias: new managers will typically only begin submitting data to HF databases after a period of good performance. This historical performance is then inserted into the database, introducing further distortion when looking at historical data.

### Selection

Unlike equity & bond indices, which use publicly available information in their construction, HF index providers rely on managers to volunteer accurate return data. This introduces severe "_self-selection_" bias to the index, as it skews the index towards a given set of managers on a going-forward basis.

Further, every HF Index draws its performance information from a different source, each with relatively few funds in common with the others (resulting in completely un-homogeneous sampling & therefore significant performance deviations between the different indices).  
***MAKE A GRAPH TO SHOW THIS***.

### Autocorrelation

Less-liquid HF strategies (such as Distressed Debt) can suffer from time-lag in valuations. This expresses itself in a smoothing of monthly returns, underestimation of volatility, and significant autocorrelation in the return series.

### Investable / Non-Investable

In order for creating an investable vehicle to track an index, the underlying managers must have sufficient capacity to accept new investments. This introduces a large selection bias, as it immediately follows that at-capacity (closed) hedge funds must be excluded from such an index.  
***GRAPH OF UNDER-PERFORMANCE***

## Back to Modelling HF Returns

### Simple Primer

Consider an equally-weighted combination of 3 strategies, each designed to capture a particular _beta factor_ (risk premia):
- A trend-following model on 25 futures - __SFGII INDEX__
- The BXM Index - CBT covered call writing strategy (write monthly ATM calls on existing equity positions) - __BXM INDEX__
- The Credit Suisse High-Yield Bond Index - __CSHY INDEX__

 Comparing these returns to the HFR Composite Hedge Fund Index (__HFRIFWI INDEX__), the HFR Fund of Funds Index (__HFRIFOF) INDEX__ & the S&P500 (__SPX INDEX__) yields some interesting results:  

| Strategy                | Performance (%) | Volatility (%)| Sharpe Ratio  |
| ------------------------|-----------------|---------------|-------------- |
| Our strategy            | 10.1            | 5.6           | ~1            |
| HFR Composite Index     | 11.1            | 7.1           | 0.9           |
| HFR Fund of Funds Index | 7.2             | 5.9           | 0.5           |

On a risk-adjusted basis, this simple strategy outperforms both of the hedge fund indices - the authors suggest this acts as a clear indicator of the role _risk premia_ play in hedge fund returns overall.  

### Regression of HF returns against systematic risk factors

Despite the problems raised earlier with HF indices, ultimately they are the only real choice available to us that contain the time series return information we need. The authors suggest that the non-investable version of an index is more likely to accurately represent the risk dynamcis of a given risk factor, since most of the problems described affect the absolute performance figure, rather than the risk profile.

That is to say, the problems described have more of an effect on the alpha value obtained, although the underling beta sensitivities are typically quite similar. This is demonstrated in the mean values of the histograms shown in _Fig. 3_, (obtained from data for 483 managers) when compared against the regression values for the HFR Index shown in _Tbl. 1_.

#### Interpreting the results

The `AR(1)` factor (1 month lagged time series of the dependent variable) plays a large part in a number of hedge fund strategies, including those with less-liquid, harder-to-price assets and others with valuation somewhat dependent on internal modelling, such as:
  - Convertible Arbitrage
  - Distressed Debt
  - Fixed Income Arbitrage

As mentioned earlier, this is likely due to delays in accurate pricing for portfolios, producing an autocorrelative effect. Futher, this suggests that simple risk measures such as Sharpe ratio, volatility, correlation, etc. may significantly underestimate the true market risk in HF strategies, meaning that the `AR(1)` factor may in fact measure some _lagged beta_.

Generally, this model captures a large portion of HF returns, with relatively high R^2 values (approx. 60% on average). However, the factor models are much more expressive for some strategies and not others.

The factor model has some degree of explanatory power for Long/Short Equity & Event-Driven strategies, but does a much poorer job for Equity Market Neutral, Merger Arbitrage & Managed Futures.

Performing a stability test based on the cumulative sum of normalised residuals confirms that the obtained factor regression models are statistically stable.

We can also test for model stability by plotting obtained factor sensitivities over a rolling time window - shown in _Fig. 5_.

# Better Benchmarks

From the
