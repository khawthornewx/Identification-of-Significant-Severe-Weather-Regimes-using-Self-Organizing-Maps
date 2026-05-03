<div align='center'>
  
# **Identification-of-Significant-Severe-Weather-Regimes-using-Self-Organizing-Maps** #
*KELVIN T. HAWTHORNE, <sup>a</sup> LEXI GUTZWILER <sup>a</sup>*

*<sup>a</sup> Northern Illinois University, Department of Earth, Atmosphere, and Environment*

 

*Conducted in partial completion of the course requirements for EAE 483/583, Data Science for the Geosciences*

*Professor: Dr. Alex M. Haberlie*
</div>

**ABSTRACT** This study uses a Self-Organizing Map (SOM), an unsupervised machine learning technique, to identify recurring significant severe weather environments using daily mean Convective Available Potential Energy (CAPE) and Convective Inhibition (CIN) from the North American Regional Reanalysis (NARR). Practically Perfect Hindcast (PPH) probabilities derived from Storm Prediction Center reports are used for validation. Results show that the SOM successfully distinguishes environments associated with varying levels of significant severe weather potential, particularly across the Great Plains and Deep South/Ohio Valley regions. However, some high-impact events are not well captured because important ingredients such as vertical wind shear are not included. Overall, the results demonstrate both the usefulness and limitations of machine learning for identifying severe weather regimes.

**SIGNIFICANCE STATEMENT**
This study uses machine learning to group similar severe weather environments across the United States using atmospheric instability and convective inhibition. The results show that machine learning can help identify patterns associated with significant severe weather, but also highlight that no single set of variables can perfectly predict severe storms because many atmospheric ingredients must work together.

## **I. Introduction and Geoscience Problem** ##

  Forecasting significant severe weather is one of the most challenging problems in meteorology. Significant severe weather is defined as hail ≥ 2 inches in diameter, damaging winds ≥ 70 mph, and tornadoes rated ≥ EF2. Environments that produce these types of hazards can vary substantially from case to case and are modulated by season, region, and even time of day. Forecasters often rely on an ingredients-based methodology, which focuses on whether key environmental ingredients, such as moisture, instability, lift, and vertical wind shear, are present and overlapping (Doswell 1987; Johns and Doswell 1992; Doswell et al. 1996). This approach is useful because it emphasizes the marriage of ingredients rather than relying on single-parameter forecasting of severe convective storms.
  
Even within the ingredients-based framework, challenges remain in understanding how these ingredients interact across different events. Previous work has shown that environments supporting severe weather can appear similar while producing very different outcomes, highlighting the importance of both synoptic and mesoscale processes (Johns and Doswell 1992). In addition, forecasting rare, high-impact events is inherently difficult because of limitations in predictability and the influence of smaller-scale variability (Hitchens et al. 2013). This means that simply identifying favorable environments is not always sufficient, and there is a need for methods that can better organize and interpret the range of conditions associated with significant severe weather.

To help address these challenges, this study uses machine learning techniques to sort and extract different regimes supportive of significant severe weather production. Two environmental indices, extracted from reanalysis data, are analyzed over a three-year period. Bias-adjusted, probabilistic hindcasts based on SPC storm reports are used to validate significant severe weather occurrence. Given the complexity of these environments, pattern-recognition techniques provide a useful way to organize the data and identify recurring atmospheric structures. These approaches allow for a more holistic view of how environmental conditions vary across events rather than focusing on individual variables in isolation. This is especially important for severe weather environments, which often do not follow a single, well-defined pattern.

  ## **II. Data and Methods** ##

*a. Datasets*

To identify environments indicative of significant severe weather, the North American Regional Reanalysis (NARR) is used to extract environmental parameters. NARR is a 32-km reanalysis dataset based on the discontinued Eta Model. The temporal record spans from 1979 to March 2026 and contains daily and monthly mean data. For the purposes of this project, daily mean Convective Available Potential Energy (CAPE) and Convective Inhibition (CIN) are extracted as yearly NetCDF files and merged into a broader dataset suitable for model preprocessing and training.

The validation dataset for this project is the Practically Perfect Hindcast (PPH), which provides gridded representations of observed severe weather reports by applying spatial smoothing to Storm Prediction Center (SPC) severe weather reports. This approach reduces the impacts of reporting biases, spatial clustering, and population effects while retaining the underlying signal of severe weather occurrence (Sobash et al. 2011; Gensini et al. 2020). The PPH dataset has been widely used in severe weather climatology and machine learning applications because it provides a continuous, grid-based representation of severe weather likelihood that is more directly comparable to environmental datasets than raw point observations.


*b. Environmental identifiers/proxies*

Two environmental proxies are used to help cluster and later identify significant severe weather regimes. The first variable is CAPE, which is a measure of atmospheric instability. Because instability is required for convective storm formation, this variable is essential in identifying days on which significant severe weather occurs. Prior work has found that higher instability is generally associated with a greater likelihood of convective storm formation. CAPE is calculated as a daily mean for the entire three-year period evaluated in this model. One shortcoming of using a daily mean, rather than a metric such as the daily maximum, is that it smooths what may be the most significant value of atmospheric instability on a given day. Still, daily mean CAPE provides insight into the types of regimes that exist.

The second environmental proxy is CIN. Unlike CAPE, which quantifies atmospheric instability, CIN measures the suppression of atmospheric instability. CIN represents the resistance of the atmosphere to a rising parcel because the parcel is cooler than the surrounding environment. This causes the parcel to either sink back to its original position or remain stagnant rather than continue rising. Strong CIN indicates an environment that is generally less conducive to thunderstorm formation, although this is highly dependent on several external factors, such as convective battering and synoptic-scale forcing for ascent. Like CAPE, CIN is calculated in the simulations used in this project as a daily mean rather than the more preferable daily minimum.

While other environmental parameters are certainly important for producing significant severe weather, limited computational resources and time required a simpler, bivariate approach. If computational resources and time had permitted, additional variables such as 700–500-hPa lapse rates (as a proxy for elevated mixed layer presence) and/or vertical wind shear could also have been evaluated.


*c. Self-Organizing Maps (SOMs)* 

A self-organizing map (SOM) is used in this study to identify and group similar severe weather environments into a set of representative regimes (Kohonen, 1982). SOMs are an unsupervised machine learning technique that reduces high-dimensional data into a lower-dimensional grid while preserving the underlying structure of the dataset (Fig. 1; Kohonen, 1982). In this application, environmental variables are used as inputs, allowing the SOM to cluster similar atmospheric setups without imposing any predefined categories.

<p align="center">
  <img src="Figures/Figure 1.jpg" width="700">
  <br>
  <em>Fig. 1 Input features are seleced and fed into the self-organizing map that then clusters each feature into “nodes” that represent a mean of all cases in that node. Figure from Kohonen, 1982. The same process is applied to this project where atmospheric variables are input features, and they are organized by like case</em>
</p>

Each node in the SOM represents a characteristic environment, with nearby nodes corresponding to similar patterns and more distant nodes representing increasingly different regimes. This structure makes SOMs particularly useful for meteorological applications, as they provide a physically interpretable way to organize complex environmental variability. Rather than identifying a single “type” of severe weather environment, the SOM highlights a spectrum of regimes that capture different combinations of moisture, forcing, and stability.
By applying a SOM to the CONUS404-derived environmental fields, this study aims to identify recurring patterns associated with significant severe weather and provide a structured framework for interpreting how these environments vary across space and time.

*d.	Model Configuration*

Each SOM is tuned with a set of parameters that control how the model learns and clusters the feature input data. For this project, our parameters were selected arbitrarily through an iterative process, each time evaluating the visual practicality of the SOM output. At the concluding iteration (used in this study) we select a learning rate of 1, a sigma of 2, and 4000 n_iterations. The SOM grid is 5x5, which will result in 25 unique nodes. 
Preprocessing is done using the sklearn built in “StandardScalar” package which flattens data and converts it to z-score. Once the data is converted to z-score, the data is fed into the som training, which is coded using the sklearn “MiniSom” package. The output is then plotted by reconverting the data back to unstandardized values and plotted as a 5x5 group of subplots containing mean environmental inputs, each containing n number of dates from throughout the period. 
Once the environmental SOM nodes are plotted, we then plot the mean practically perfect nodes by extracting the dates from the environmental nodes and applying the same dates from the practically perfect dataset to the corresponding nodes. The resultant plot is a similar 5x5 group of subplots with mean practically perfect probabilities. These can then be compared against the environmental SOM nodes, and subsequent analysis can be conducted. 

  ## **III. Results** ##

The resulting environmental 5x5 SOM output shows ascending volatility in CAPE and CIN magnitude from the bottom row to the top row (Fig 2). Since we do not mask the oceans, high values of CAPE over warm and humid water surface results in a SOM where mean CAPE value increases from the bottom leftmost node to the top rightmost node (Fig 3). CIN has a more heterogenous distribution throughout the SOM, with the highest mean CIN value residing in the top left corner, generally increasing towards the bottom rightmost corner (Fig 3). 

<p align="center">
  <img src="Figures/Figure 2a.png" width="700">
  <br>
  <em>Fig. 2a</em>
</p>

<p align="center">
  <img src="Figures/Figure 2b.png" width="700">
  <br>
  <em>Fig. 2b</em>
</p>

<p align="center">
  <img src="Figures/Figure 3.jpg" width="700">
  <br>
  <em>Fig. 3</em>
</p>

The practically perfect SOM matches well to the environmental SOM in that the lowest occurrence of significant severe weather occurs in the bottom row of nodes while the highest occurrence of significant severe weather occurs in the top row. The best node for significant severe weather occurs at node 1,3 and this matches the most significant severe practically perfect probability (Fig 4). Node 1,3 shows a bifurcated maximum in significant severe practically perfect probabilities with the first being over the central and southern Great Plains while the second extends from the Deep South to the Ohio Valley. The environmental node from 1,3 shows high CAPE with >-50 J/kg of CIN over the Deep South and Ohio Valley sector while the Great Plains sectors show high CAPE and <-50 J/kg of CIN. Regardless, both regions produced significant severe, indicating that the model is picking up on two different types of significant severe weather setups. The first type of set-up, which aligns with the Central Great Plains, is likely indicative of supercellular events where the largest contribution to significant severe reports is hail and tornadoes (Davenport 2021; Ashley et al. 2023; Homeyer et al. 2023). The second type of set-up, which is in the Deep South and Ohio Valley, is likely showing more QLCS, MCS, and derecho type setups where much of the contribution is likely from significant tornadoes and wind (Trapp et al. 2005; Sherburn and Parker 2014; Ashley et al. 2019; Haberlie and Ashley 2021; Kaminski et al. 2024). 

<p align="center">
  <img src="Figures/Figure 4.png" width="700">
  <br>
  <em>Fig. 4</em>
</p>

To further investigate the bifurcated regions and the inference that there are two different “types” of outbreaks, a mean is taken from every environmental node and every practically perfect node (Fig 5). This is essentially serving as a “climatology” for the entire period. This climatology also demonstrates the two primary hotspots, and the eastern hotspot shows higher CAPE with weaker CIN while the western hotspot shows higer CAPE and stronger CIN. While both regimes produce nearly equal to significant severe weather, the methods from which they produce these significant severe perils are different. As mentioned before, the Plains hotspot likely shows supercell associated events while the Deep South and Ohio Valley hotspot likely shows QLCS/MCS associated events. Future work should physically run the total significant severe probabilities for each individual peril to confirm this hypothesis.

<p align="center">
  <img src="Figures/Figure 5.png" width="700">
  <br>
  <em>Fig. 5</em>
</p>

While the SOM did a sufficient job at classifying the various regimes and related practically perfect hindcast probabilities, it still has certain situations where it falls short. We extracted the maximum practically perfect event from the entire period and output the associated CAPE/CIN mean for that day (Fig 6). The results show that while the practically perfect probabilities were nearly maxed out, the environment was relatively low in CAPE. Upon further review of this case, which offered on April 12, 2020, across the Deep South, the relatively lower values of CAPE were compensated by extremely strong vertical wind shear. These types of cases, referred to as “high shear-low cape” are common in the Deep South and during the cool or transition season (Sherburn and Parker 2014). Significant severe weather is still allowed to occur because the amount of vertical wind shear is strong enough to induce tilting and rotating of updrafts, even though they tend to be more low-topped and less robust. This allows us to produce significant severe perils, typically tornadoes, despite the low thermodynamic energy available.  Since this project only clustered based on CAPE and CIN, it did not account for events where vertical wind shear may dominate and thus, despite higher practically perfect probabilities, they are assigned to comparatively lower practically perfect probability and weaker environmental nodes (Fig 6).

<p align="center">
  <img src="Figures/Figure 6.png" width="700">
  <br>
  <em>Fig. 6</em>
</p>

  ## **IV. Conclusion** ##

A multitude of research conducted over a long period of time has found that the development of significant severe thunderstorms and their associated hazards relies on the overlap of many ingredients (e.g., Doswell 1987; Johns and Doswell 1992; Bluestein 1993; Doswell and Schultz 2006; Markowski and Richardson 2010). Pattern recognition can help identify specific regimes and their association with varying frequencies of significant severe weather, but no collection of indices can perfectly predict atmospheric processes. There is no magic bullet in severe storm forecasting.

This study concludes that the overlap of CAPE and CIN can bring us closer to identifying regimes conducive to significant severe weather. Even so, the absence of boundary layer moisture, vertical wind shear, and other critical ingredients results in certain high-end severe cases being missed. Even more important than the overlap of ingredients is the concept of the marriage between them. Thunderstorms in general, let alone those that produce significant severe hazards, rely on a near-perfect alignment of ingredients. This is not something that a SOM, or any machine learning process, can accurately measure.

While CAPE remains one of the strongest indicators of thunderstorm development because it serves as a proxy for atmospheric instability, there is substantial evidence that CIN also plays a role in the production of significant severe hazards in certain environments. Environments with little CIN tend to over-convect, which can lead to lower hazard production compared to more discrete storm modes. Conversely, linear storm modes can produce more intense hazards in environments with little CIN. This again highlights the idea that the marriage of these ingredients, along with many additional variables not discussed here, is what ultimately leads to the production of significant severe weather. Furthermore, these relationships may vary by region, season, and even time of day.

Despite these limitations, future work may use this workflow to address the same research questions with a more extensive set of variables or over a longer climatological period. Additionally, other machine learning techniques (e.g., Random Forests) may better predict regimes rather than provide only broad classifications. Overall, there remains substantial opportunity for future work examining severe convective storms using machine learning techniques. However, the most important prerequisite is recognizing that no methodology will produce perfect results given current observational and data limitations.

## **References** ##
Ashley, W. S., A. M. Haberlie, and V. A. Gensini, 2023: The future of supercells in the United States. Bull. Amer. Meteor. Soc., 104, E1–E21, doi:10.1175/BAMS-D-22-0027.1.

Ashley, W. S., A. M. Haberlie, and J. Strohm, 2019: A climatology of quasi-linear convective systems and their hazards in the United States. Wea. Forecasting, 34, 1605–1631, doi:10.1175/WAF-D-19-0014.1.

Bluestein, H. B., and S. S. Parker, 1993: Modes of isolated, severe convective storm formation along the dryline. Mon. Wea. Rev., 121, 1354–1372, doi:10.1175/1520-0493(1993)121<1354:MOISCS>2.0.CO;2.

Chalmers, Z. A., and M. D. Parker, 2026: Variability in high-shear, low-CAPE QLCS environments in the southeastern U.S. Wea. Forecasting, in press, doi:10.1175/WAF-D-25-0137.1.

Davenport, C. E., 2021: Environmental evolution of long-lived supercell thunderstorms in the Great Plains. Wea. Forecasting, 36, 2187–2209, doi:10.1175/WAF-D-21-0042.1.

Doswell, C. A., 1987: The distinction between large-scale and mesoscale contribution to severe convection: A case study example. Wea. Forecasting, 2, 3–16, doi:10.1175/1520-0434(1987)002<0003:TDBLSA>2.0.CO;2.

Doswell, C. A., H. E. Brooks, and R. A. Maddox, 1996: Flash flood forecasting: An ingredients-based methodology. Wea. Forecasting, 11, 560–581, doi:10.1175/1520-0434(1996)011<0560:FFFAIB>2.0.CO;2.

Gensini, V. A., A. M. Haberlie, and P. T. Marsh, 2020: Practically perfect hindcasts of severe convective storms. Bull. Amer. Meteor. Soc., 101, E1259–E1278, doi:10.1175/BAMS-D-19-0321.1.

Haberlie, A. M., and W. S. Ashley, 2019: A radar-based climatology of mesoscale convective systems in the United States. J. Climate, 32, 1591–1606, doi:10.1175/JCLI-D-18-0559.1.

Hitchens, N. M., H. E. Brooks, and M. P. Kay, 2013: Objective limits on forecasting skill of rare events. Wea. Forecasting, 28, 525–534, doi:10.1175/WAF-D-12-00113.1.

Homeyer, C. R., E. M. Murillo, and M. R. Kumjian, 2023: Relationships between 10 years of radar-observed supercell characteristics and hail potential. Mon. Wea. Rev., 151, 2609–2632, doi:10.1175/MWR-D-23-0019.1.

Johns, R. H., and C. A. Doswell, 1992: Severe local storms forecasting. Wea. Forecasting, 7, 588–612, doi:10.1175/1520-0434(1992)007<0588:SLSF>2.0.CO;2.

Kaminski, K., W. S. Ashley, A. M. Haberlie, and V. A. Gensini, 2024: Future derecho potential in the United States. J. Climate, 38, 3–26, doi:10.1175/JCLI-D-23-0633.1.

Kohonen, T., 1982: Self-organized formation of topologically correct feature maps. Biol. Cybern., 43, 59–69, doi:10.1007/BF00337288.

Mesinger, F., and Coauthors, 2006: North American regional reanalysis. Bull. Amer. Meteor. Soc., 87, 343–360, doi:10.1175/BAMS-87-3-343.

Sherburn, K. D., and M. D. Parker, 2014: Climatology and ingredients of significant severe convection in high-shear, low-CAPE environments. Wea. Forecasting, 29, 854–877, doi:10.1175/WAF-D-13-00041.1.

Sobash, R. A., J. S. Kain, D. R. Bright, A. R. Dean, M. C. Coniglio, and S. J. Weiss, 2011: Probabilistic forecast guidance for severe thunderstorms based on the identification of extreme phenomena in convection-allowing model forecasts. Wea. Forecasting, 26, 714–728, doi:10.1175/WAF-D-10-05046.1.

Trapp, R. J., G. J. Stumpf, and K. L. Manross, 2005: A reassessment of the percentage of tornadic mesocyclones. Wea. Forecasting, 20, 680–687, doi:10.1175/WAF864.1.

## **Supplemental Workflow Information** ##
# *Packages Used:* #
**Xarray** for gridded data interrogation
**Numpy** for numeric manipulation
**Matplotlib** for simple data visualization
**Sklearn StandardScalar** for SOM preprocessing
**Sklearn RobustScalar** for SOM preprocessing
**Sklearn MinMaxScalar** for SOM preprocessing
**Sklearn MiniSom** for SOM Training
**Cartopy** for plotting gridded data geographically

