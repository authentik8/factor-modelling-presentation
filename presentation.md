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

### Autocorrelation

Less-liquid HF strategies (such as Distressed Debt) can suffer from time-lag in valuations. This expresses itself in a smoothing of monthly returns, underestimation of volatility, and significant autocorrelation in the return series.

### Investable / Non-Investable

In order for creating an investable vehicle to track an index, the underlying managers must have sufficient capacity to accept new investments. This introduces a large selection bias, as it immediately follows that at-capacity (closed) hedge funds must be excluded from such an index.

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

As mentioned earlier, this is likely due to delays in accurate pricing for portfolios, producing an auto-correlative effect. Further, this suggests that simple risk measures such as Sharpe ratio, volatility, correlation, etc. may significantly underestimate the true market risk in HF strategies, meaning that the `AR(1)` factor may in fact measure some _lagged beta_.

Generally, this model captures a large portion of HF returns, with relatively high R^2 values (approx. 60% on average). However, the factor models are much more expressive for some strategies and not others.

The factor model has some degree of explanatory power for Long/Short Equity & Event-Driven strategies, but does a much poorer job for Equity Market Neutral, Merger Arbitrage & Managed Futures.

Performing a stability test based on the cumulative sum of normalised residuals confirms that the obtained factor regression models are statistically stable.

We can also test for model stability by plotting obtained factor sensitivities over a rolling time window - shown in _Fig. 5_.

# Better Benchmarks


The main question emerging from this factor modelling exercise is whether the models & the factor exposures they suggest can be used to create better hedge fund benchmarks:
- Aim to replicate particular hedge fund strategies
- Constitute investable alternatives to hedge fund indices

Ultimately the goal is to __accurately separate systematic risk exposure from true manager alpha__ (which shouldn't be part of a benchmark).

A number of papers (including one published by Bridgewater Associates) have attempted to do exactly this, across Managed Futures, Merger Arbitrage, Fixed Income Arbitrage & Long / Short Equity. In this case, the performance of strategies investing directly in the factor exposures (Replicaating Factor Strategy, RFS) taken from the regression is given, i.e.
```maths
Return(t) = Σ(β_i * F_i(t))

where:
  Return(t) : Strategy return at time t
  β_i       : Co-efficient of exposure to factor i
  F_i(t)    : Return of factor i at time t
```

Claim to avoid in-sample over-fitting by calculating factors for RFS on a rolling look-forward basis, i.e. data to determine factor level at time t was determined by the previous 5 years of data, ending with the previous month.

## Results

For the years 2003-2005, cumulative RFS returns are often superior to the returns of HF indices, especially when compared to the investable versions (with the sole exception of the  Distressed strategy).

The authors suggest that this potentially provides an illustration as to the source of hedge fund returns:

- A long-only manager has two main sources of return: Market risk (i.e. Beta) and his "alpha".
- The authors claim that a HF will hedge away all or most of the broad market exposure, e.g. through shorts, derivatives, etc.
- Naively, this results in a "pure alpha" product with low expected returns and low expected risk, which is then amplified via leverage to produce an attractive standalone investment.
- The use of leverage to scale up the alpha product reveals sources of all sorts of risk that were previously hidden - "_beta in alpha's clothing_".

The authors estimate that up to __80%__ of HF returns originate from Beta exposure, with the balance accounted for by alpha (or indeed other not-yet-modelled risk factors).

### Strategy-Specific Notes

#### Long-Short Equity

- Wilshire spread is difference between Wilshire 750 Index & Wilshire 1750 index, intended to proxy the risk-premium captured by investing in smaller-cap stocks.
- L/S returns in fact have a highly non-linear profile, similar in many ways to convertible bonds:
  - Less participation on the upside
  - Protection on the downside to some extent
  - More expressed losses in the event of a larger downturn (when convertibles lose their bond floor)
- Substitution of an equity factor with a convertible bond index therefore substantially improves the modelling accuracy of this model.
- CPPI component reflects that HFs will typically decrease exposure in falling markets and increasing it in rising ones
- RFS strategy performs relatively well alongside the HFRI index - Fig. 17 shows decreasing alpha over time and thus increased predictive power of the factor model, whilst it outperforms the HFRX investable index.

#### Equity Market Neutral

- Given funds following this style aim for zero exposure to equity market factors, it's unsurprising that this strategy only has a small exposure to broad equity markets
- Also carries sensitivity to UMD momentum factor & value factor
- Along with Managed Futures, the factor model shown here has the lowest degree of explanatory power, suggesting that simple non-linear models fall short of providing high degrees of explanatory power.
- Part of this may be due to the very different sub-styles that fall into this category:
  - System-based approach buying undervalued stocks & selling overvalued stocks according to value & momentum-based analysis
  - Short-term approach based on statistical analysis of relative performance deviation of similar stocks.
- RFS strategy somewhat underperforms HFRI index reflecting positive alpha identified in factor analysis, although outperforms the HFRX index significantly.


#### Short-Selling

- Main exposure of this strategy is (self-evidently) negative exposure to the equity market
- Best modelled with the same factor as the L/S manager: a convertible bond index
- High alpha value ~4-5%, for a number of possible reasons:
  - Many investors are restricted from selling short
  - Must be high in order for the strategy to generate any profits at all, since short-equity starts with an expected return of -4:7%


#### Event Driven

- Large exposure to broad equity market, small cap stocks & HY bond market
- AR(1) possibly due to:
  - Liquidity risk
  - Potential lagged pricing of underlying securities
- Cumulatively explains 80% of event-driven performance
- Highest alpha factor of approx. 5% p.a.
- RFS strategy returns about 2/3 of HFRI index return but again, significantly outperforms the HFRX & S&P investable indices.


#### Distressed Securities

- Simple set of exposures to credit, equity (largely small-cap) & liquidity risk
- Largest factor is AR(1), reflecting low liquidity prevalent in this strategy, due to a lack of regular pricing and valuation
- Closely represents the return stream of private equity investment
- High level of alpha (3-4% p.a.)
- Outperformed by both the HFRI & HFRX indices


#### Merger Arbitrage

- Proposed in another paper that this strategy has high conditional correlations:
  - High correlation to equity market to equity markets when it declines
  - Low correlation when stocks trade up or sideways
- This corresponds well to a correlation profile of a sold put option on equities, and in fact directly to a sold put option on announced merger deals.
- Short put profile is reflected in high exposure to BXM factor: shorting put options gives limited upside but full participation on downside
- Appropriate on a number of levels:
  - Obvious exposure to merger deals breaking up
  - When stock markets decline sharply, deals are more likely to breaking
  - Market declines will also reduce likelihood of bids being revised up & reduce competition for acquisition targets
- Overall exposure to equity market comes from correlation between event risk & the market rather than individual position level exposures.

#### General Relative Value

- Covers both the Fixed Income Arbitrage and Convertible Arbitrage strategies.
- Three types of systematic exposure:
  - Capitalize on price spreads between two or more related financial instruments, capturing a risk premia such as:
    - Credit risk
    - Interest rate term structure risk
    - Liquidity risk
    - Exchange rate risk
  - Provide liquidity and price transparency in complex financial instruments (typically employing proprietary valuation models)
    - Liquidity risk
    - "Complexity premia"
  - Short some form of volatility, as RV HF managers have a preference for negatively skewed return distributions: "__picking up pennies in-front of a steam roller__".

##### Fixed Income Arbitrage

- Exposed to a mixture of:
  - Liquidity risk
  - Credit risk
  - Term structure risk
- Example strategies include:
  - Credit barbell: long short-term debt of lower credit quality, short long-term government debt of high credit quality
  - Yield curve spread trades
  - On-the-run vs. off-the-run treasury bond positions
- High exposures to credit risk, convertible bonds & EM bond securities
- High AR(1) significance lagged pricing of underlying securities & thus liquidity risk
- Heaviest losses occur in "flight-to-quality" scenarios, & thus strategy bears return profile similiar to short OOTM options: risk of significant losses but otherwise steady returns.

##### Convertible Arbitrage

- Exposed to a variety of different risk factors:
  - Credit risk
  - Equity market risk
  - Equity volatility risk
  - Liquidity risk
- HY factor, convertible & equity factors make an appearance in factor weightings, as does the `AR(1)` term.
  - Autocorrelation term signals a lack of consistent & timely pricing of underlying convertible securities
  - Reflects exposure to liquidity & valuation risk
- Two distinct disciplines within this space:
  - Option-based: Buy the convertible bond, sell short the underlying equity and maintain a delta hedge (__gamma trading__)
  - Credit-oriented: Make an explicit assessment of the issuer's creditworthiness & take on overpriced credit risk
- These two strategies have very different exposure & factor profiles:
 - Credit oriented clearly has a significant exposure to credit risk, the option-based method does not.
 - Increased volatility helps the option-based strategy, but widening credit spreads have a negative impact.
- The two trading styles are not currently differentiated in indices, making it difficult to capture their factor sensitivities independently.


#### Global Macro

- Do better in strong bond markets, indicated by strong correlation with bond market index factor
- Other factors include trend-following(sGFI) & non-linear exposure to the broader equity market.
- R^2 value comes out quite low for this strategy, due to the wide coverage of this strategy: Global Macro includes probably the widest variety of trading approaches.
  - Probably best applied to on a manager-level, as it is the individual markets and particular investment techniques employed by a given manager that give the available risk premia & potential market inefficiencies.

####  Managed Futures

- Main speculative agents in the global futures markets, capturing what is referred to as the "__commodity hedging demand premium__"
- sFGI trading rule mentioned for __Global Macro__ comes into play:
  - Volatility-weighted combination of trend following strategies
  - Covers 25 liquid futures contracts, covering commodities, bonds & currencies
- Only strategy to display negative alpha (although not at a statistically significant level)
