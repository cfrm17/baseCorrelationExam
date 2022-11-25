# Base Correlation Test


In general, there is still no market consensus on how to apply the market implied correlation information to bespoke tranches, in which the composition of the collateral pool is different from those indexes. Since the model was approved, there are several new developments in both modeling and market practice. There are three major changes to the GSP model:

1.	The GSP mapping methodology, named scaling, has been extended to allow the calibration of a parameter for a better fitting of MarkIt results. The bespoke base correlations are also calibrated to the base correlations with different indexes and different maturities, when the composition and maturities differ from those indexes.

2.	The computation precision of the semi-closed form normal copula model has been increased [1]. The number of Gauss-Hermite integration points should be 320. Alternatively, one could use the default choice (put zero in cell “H15”), given as a function of correlation. A related memo is attached as Appendix II.

3.	The restrictions on the Monte Carlo (MC) simulation for CDOs have been enforced [2], which also applie to the base correlation toolbox. For CDO trade, the minimum number of MC simulation path is 500,000.

One of the main reasons we have confidence in the GSP model is the outcome of MarkIt practice of the CDO bespoke trades. We have also noticed that Bear Sterns has proposed a methodology to value bespoke tranches. The mapping methodology is exactly the same as “Loss Ratio”. However, as verified by the MarkIt an extended scaling method with scaling factor , as described in the next section, proves to be a better way. 

If a bespoke portfolio has a well-diversified North America names with same trade definition of the CDX5 and same credit spread level, it would be reasonable to use the base correlation implied by CDX5.  If there is a difference, for example, in the credit spread level, somehow an adjustment of the base correlation can be made via a mapping methodology. This is based on the belief that the base correlations correspond to different level of risk in the capital structure of the collateral pool. As far as we know, all the mapping methodologies are a kind of normalization or scaling to the base correlation curve. The expected loss of the entire collateral pool is independent of the correlation hence serves as a good candidate of serving this purpose. 

For the same reason the mapping has certain uncertainties for the bespoke collateral pool with respect to homogeneity of credit quality, portfolio diversification, and portfolio mixture issue. Those are open questions and subject to future research. Our current approach is as good as we can get and verified by the MarkIt results. More importantly, we are fully aware of the uncertainty imbedded in the mapping and a model reserve has been set up since the method was introduced. Hence at the present time there is no restrictions on those issues and, as market evolves, we will re-access the materiality and improve our model as necessary.

•	Mapping Methodology

The following procedure is involved in finding the base correlation values for any tranche with a bespoke collateral pool:

1.	Calculate the Base Correlation Curves for the Indexes 

At the present time in the market there are quoted curves with different indexes (CDX and iTraxx) and maturities (3y, 5y, 7y, 10y). The base correlation for CDX is assumed to be   with   the detachment points of the base tranches and  the maturities of the on-the-run indexes. The base correlation curves for iTraxx   are denoted the same as the CDX except that the detachment points are different ( for iTraxx). 

2.	Calculate Mapped Base Correlation Curves for a Target Bespoke Portfolio. 

Assume a bespoke trade with maturity  . Its underlying collateral pool is calibrated to all the market quoted base correlation curves using a scaling method to find the mapped equivalent base correlation curves. For example, when mapped to the CDX series with maturity  , the mapped attachment point   reads:

(1)	 ,

where   is the notional amount of the CDX.   and   are the expected losses of the entire CDX portfolio and the bespoke portfolio, respectively, which are independent of correlation.  is defined as a scaling factor. 

Note that Eq.(1) is exactly the same as the initial scaling method if we choose  . On the other hand, if we choose  , there is no scaling effect considered.  At the present time, GSP chooses  . It has been found that the GSP prices submitted to MarkIt are competitive among the participants because of this adjustment.

3. Calculate the Base Correlation Values Weighted by Term

For each equivalent base correlation curves, we could calculate the base correlation value for any bespoke tranche. If the tranche is defined as the attachment  and detachment  , the mapped base correlations for this tranche using the CDX curves are denoted as   and  . The base correlation calibrated to the CDX   is found by a linear interpolation of maturities . The same procedure is done to the iTraxx and the base correlation calibrated to iTraxx is assumed to be  .

4.	The base correlation values weighted by indexes

Assume the target trade with a bespoke collateral pool, in which there are P% obligors that belong to the North America and 1-P% belongs to the Europe.

Then the base correlation calibrated to the whole indexes market reads:

(2)	 

  and   are the calibrated base correlation values used for the MTM of the bespoke tranche with the attachment  and detachment  .

Note that the adjustment to the maturity and composition seems straightforward, although it is subjective in term of modeling. The same approach has been proposed by Bear Sterns [4].


The implementation of the model is easy to verify because, if we set alpha to be one, it converges to the old approved method. If alpha is set to be zero, the calibrating base correlation curve is reproduced. The test results are not shown in the report.

The second test shows the MarkIt result for the bespoke trade. As the third test, the average credit spread and volatilities for all the current GSP collateral pools are calculated and compared with those for the index pools.


We are confident that, therefore, the changes made to the model are appropriate as long as enough risk control is enforced.

First, the issue is identified by plotting the conditional loss probability of  ( ) for the test trade with different correlations in Figure 1. It can be seen that, when correlation becomes large, the conditional default probability function is no longer a smooth function and the precision of the Gauss-Hermite integration tends to deteriorate. There will be no solution if the correlation is exactly one hence the problem is fundamentally imbedded in the one-factor semi-closed form solution. The limit of a correlation, which can be employed to predict a reliable solution, is needed.

Moreover, it should be noted that the number of iteration is proportional to the number of integration points over the latent variable. Subsequently the efficiency of the semi-closed form solution is undermined, if the number of integration points is set to be too large. 

In the GSP model a maximum correlation value 0.9 and 20 points Gauss-Hermite integration are chosen. In order to assess the choices, we run a test by using different number of Gauss-Hermite points for the test trade. We also run a 1000-point equally spaced abscissas for the latent variable and MC simulation.

 

the iteration is performed quarterly and the integration over time is carried out by linear interpolation of the expected loss function. Assuming the correlation be 0, 0.3, and 0.7, we export the actual quarterly expected loss function for each tranche of the test trade. The detailed results are given in Appendix VI and the case of correlation being 0.3 is plotted in Figure 2. We can see that it is quite linear over time within a quarter; hence the linear interpolation is acceptable. 

The second test is to find the change of the value of the test trade by increasing the number of iteration within a quarter to 2, 5, 10, and 100. The results for the first tranche are shown in Table 6. It is shown that the change is very small, when the number of iterations increased from 1 to 10. However, in this case, the simulation time would increase by almost 10 times.

 

Most of the trades have similar levels of credit spreads and volatility to that of the index. This can be shown in Figure 2, in which the volatility of the 5y credit spread for both all the collateral pools and indexes are plotted. Two trades named “CDX_4_JUN10_HY4_360-140516” and “CDX_4_JUN10_HY4_360-140730” have large credit spread level and volatility because they have CDX4 high yield portfolio (see https://finpricing.com/lib/FiZeroBond.html. They are directly calibrated to the CDX4_HY market quotes. We also notice that there are few trades (13), which have a collateral pool less than 30 obligors. A further research on the limitation of number of obligors and the diversification of the bespoke portfolio is needed.

Note that this analysis is just a simple estimation. There exist some measures on the portfolio diversification such as diversity scores of a portfolio, which need more information on the obligors in the collateral pool. An analysis leading to a robust and efficient control on the how good the mapping is remains an open question and is subject to future research. As far as we know, there is no current market consensus on any adjustments to the mapping to account for these potential uncertainties.

Therefore we believe that our current approach is as good as we can get and the uncertainties on mapping are not a serious concern for most of the GSP outstanding pools.

In this report the GSP base correlation model has been re-reviewed.  It has been found that the latest changes are reasonable and reflect the latest progresses in this fast changing field of credit derivative modeling. 

The mapping methodology, named scaling, has been extended so that a scaling factor can be adjusted. The bespoke base correlations are also calibrated to the base correlations with different indexes and different maturities, when the composition and maturities of the bespoke trade differ from those indexes.  It has been found that the scaling factor 0.5 provides us competitive results of MarkIt. 

As far as we know, there is no generally accepted methodology of mapping the base correlation curve to a bespoke collateral pool. The mapping is based on the assumption that the probability distribution of the losses can reasonably be assumed to the same as for CDX or iTraxx. Hence there are certain uncertainties for a bespoke pool with respect to homogeneity of credit quality, portfolio diversification, and portfolio mixture issue. Those are open questions and subject to future research. As far as we know, there is no current market consensus on any adjustments to the mapping to account for these potential uncertainties, and the Markit results seem to verify this.


Figure 1 5Y Credit Spread Standard Deviations for GSP Outstanding Trades and Indexes
 

Our test also shows that, in terms of the credit spread level and its distribution, most GSP outstanding bespoke pools are close to the index pools, suggesting that those limitations would not be a serious concern. Moreover, a model reserve has been set up that reflects this uncertainty.

There are two technical improvements to the model. The computation precision of the semi-closed form normal copula model has been increased. The restrictions on the Monte Carlo simulation for CDOs have been enforced, which is 500,000. 


