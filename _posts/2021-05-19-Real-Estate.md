---
layout: post
title: "Data Visualization - Real Estate"
description: ""
categories: [DataVisualization]
tags: [Spatial Analysis, Data Visualization]
redirect_from:
  - /2021/05/19/
---

## Data Visualization


### Real Estate
Over the past decade, King County apartment buildings within a mile of specialty grocery stores like Trader Joe’s and Whole Foods appreciated faster. To further interpret the real estate market in King County, the public real estate [datasets](https://kingcounty.gov) are analyzed through the data-scientific workflow and used to produce the following data visualizations.

#### Figure 1: Scatterplot Chart of kc_house_data (Preview Plot)
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119275885-9f553b00-bbe5-11eb-993c-42dc2107a96b.png">

- Figure one summarizes relationships between different property characteristics and the sale prices. For example, the Preview Plot in the Matrix Corner View updates to show a larger view of the scatterplot of price and bathroom. The relationship between the number of bathrooms and the price does not exhibit a strong linear relationship suggesting that the number of bathrooms does not affect the sale price of homes.

#### Figure 2: Scatterplot Chart of kc_house_data (Pearson's r)
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119276153-42f31b00-bbe7-11eb-883f-a2b67b121512.png">

- Figure two quantifies the strength of the linear relationship between variables or how much influence one variable has on another. An absolute value of Pearson's r close to one indicates a strong positive linear relationship, whereas values close to zero indicate a weak linear relationship.

#### Map 1: Standardized Residual Map
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119276821-37552380-bbea-11eb-90a1-884c383bcb33.png">

- Map one shows the dark green and dark purple circles indicating a large mismatch between the predicted sale price of the homes and the actual sale price of the homes applying the Generalized Linear Regression (GLR) model. The darker green points seem to cluster around bodies of water. The regression model is systematically underestimating the sale price of the houses close to water bodies. It looks as though small changes to the size of the living space may result in bigger changes to the price of a house by a water body compared to a house that is inland.

#### Figure 3: Relationship between Variables
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119276922-c19d8780-bbea-11eb-9cbd-d8d488e01b53.png">

- The Relationship between Variables chart displays predictions performed by GLR. In figure three, green colors indicate an underestimation of the sale price of the home. Purple indicates an overestimation, where the predicted price is above the actual price of the house.

The generalized Linear Regression (GLR) model shows a significant relationship between the sqft_living variable and price. However, the GLR model underestimates house values for houses that are near bodies of water. So, waterbody data is added to improve the GLR model.

#### Figure 4: Distribution of Standardized Residuals (w/o Waterbody)
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119277100-aed78280-bbeb-11eb-8260-281549a6d666.png">

#### Figure 5: Distribution of Standardized Residuals (w/ Waterbody)
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119277108-bac34480-bbeb-11eb-9ed7-6414d0a42f07.png">

Figures four and five indicate the estimation error was not improved by adding distance to lakes. There are at least two possible reasons that adding the distance features did not improve the GLR model. First, the distance features calculated in GLR are Euclidean, or straight-line, distances. Since most travel in this area is along with the road network, it may be that straight-line distances are not a reasonable representation of the road travel distance from the houses to the lakes. Second, the relationship between the size of the living space and the sale price of the home near water bodies may not be linear. It may be that GLR is an overly simple model for this scenario. Therefore, the simple linear relationships modeled by GLR may not apply in this dataset. The regionalized General Linear Regression model will be applied to run separate GLR analyses for each region (Map two).

#### Map 2: Spatially Constrained Multivariate Clustering
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119277821-cd3f7d00-bbef-11eb-8f0d-3c7929beedb6.png">

#### Figure 6: Spatially Constrained Multivariate Clustering Box-Plots
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119277840-e1837a00-bbef-11eb-9f37-47b24fc909cd.png">

The colors in figure six match the colors of the clusters on map two. Blue, green, yellow, brown, and purple clusters are above the third quartile for price and sqft_living. Blue corresponds to a cluster where living space is smaller compared to green and brown, but the price is higher. This may indicate a desirable part of town. On the map, this corresponds to an area to the east of Lake Washington. In this cluster, living space size may not be the main driving factor for the sale price of the home. The green region, located on an island in Lake Washington, corresponds to houses with larger living spaces compared to blue clusters but at a lower price. The pink cluster is cheaper than the red and grey clusters, with the average living space size the same as the red cluster in the below-third-price-quartile regions. This may indicate that one can get a cheaper house for the same living space size in the pink cluster. This may also indicate why the linear model did not work. Then, the Geographically Weighted Regression (GWR) will be used to model house prices.

#### Map 3: local_rlns_sqft_living_vs_price 
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119277855-f52ee080-bbef-11eb-9e52-cab33895e34b.png">

#### Map 4: valuation_sqft_living_gwr
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119277864-07a91a00-bbf0-11eb-88ee-75517aa03cab.png">

#### Figure 7: Distribution of Standardized Residual (valuation_sqft_living_gwr)
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119277981-9e75d680-bbf0-11eb-8c18-44da1c282237.png">

- Figure seven shows the majority of the points that have standardized residuals close to 0. The GWR model makes fewer over- and underestimations and has fewer locations with large residuals compared to the GLR model. So, the GWR model captures price variations better compared to the GLR model.

#### Map 5: valuation_sqft_living_gwr_SQFT_LIVING
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119277999-adf51f80-bbf0-11eb-8cca-f2699147beb2.png">

- Map five indicates the local regression coefficients are positive. Around both large lakes, the sale price of the home raster has a higher slope with respect to the size of the living space, indicating that a small change in living space in homes close to water corresponds to a much greater increase in price compared to inland areas. This is expected as the sale price in these areas is greatly impacted by the view, a variable not captured with the size of living space.

According to the statistics, the Adjusted R-squared is 0.87 in the Geographically Weighted Regression (GWR) model. But, how can the GWR model be improved further? Thus, the Forest-Based Classification and Regression model will be applied to improve the GWR model.

#### Figure 8: Validation R2
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119278040-eb59ad00-bbf0-11eb-9373-3d4f4c7d6d5a.png">

- Figure eight shows the average accuracy of the Forest-Based Classification and Regression model is 0.79.

#### Figure 9: Distribution of Variable Importance
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119278057-fb718c80-bbf0-11eb-8ff5-bd511ec758c3.png">

- Figure nine shows the two most important variables are sqft_living and grade. They appear highest on the Y (Importance) axis. Here, importance corresponds to the number of times a tree split is performed based on the variable in the entire forest model. Higher numbers indicate a higher number of tree splits based on a variable, indicating that the variable’s impact on the forest model result is high. This chart indicates that grade and sqft_living switch their importance rank between different runs of the model. Distance to a large lake is the third-most influential predictor in the model.

Now, The forest-based model's Adjusted R-squared is still lower than the GWR model's with one variable. To further improve the model, the predictor variables with low importance are removed. So they won't be randomly selected for a particular tree at the expense of more important explanatory variables. Then, Adjusted R-squared changes to 0.946 with high accuracy after the model includes only top important variables.

#### Figure 10: Prediction Interval
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119278182-a4b88280-bbf1-11eb-809b-dad513d1037f.PNG">

- Figure ten shows the uncertainty bounds of the prediction, with the blue line being the actual prediction. Note that uncertainty bounds rapidly widen for homes priced at more than $1,000,000. The reason for that is the small sample size for such expensive homes. Note that for homes more expensive than $1,500,000, the uncertainty bounds are even bigger as there are even fewer samples in this price range. This plot is a useful way to show the uncertainty relating to the predictions given a training sample.

#### Map 6: Optimized Hot Spot Analysis
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119278218-d3cef400-bbf1-11eb-93cb-6f30b902e9c7.png">

- Map six shows that uncertainty tends to be higher in the southern half of the dataset and lower in the northern half and indicates that sale price predictions in the northern portion of King County, Washington, are less prone to change by random changes to the training data.

In conclusion, both Geographically Weighted Regression (GWR) and Forest-Based Classification and Regression (FBCR) models have acceptable Adjusted R-squared. Which model should be used when predicting the house prices in King County? GWR is better suited to capture spatial variations in relation to price. GWR also works well to develop a local model for price, where the predicted house price is reasonable for the neighborhood. However, due to multicollinearity, the grade variable is not a predictor for GWR. In contrast, FBCR models the impact of the condition of the new houses by using analogs from all of King County, Washington. This model results in higher home prices, which may make sense if the grade of the structures is very high and the developer is considering listing them for a significantly higher price than other houses in the neighborhood. Uncertainty analysis in FBCR shows that prices of expensive homes over $1 million may need to be re-assessed. The GWR model shows reasonable values for the Redmond, Washington, area but does not factor in the condition of the new homes. So, neither method is inherently superior to the other. GWR and FBCR address different needs of valuation.

#### Figure 11: Distribution of GWR (price per square foot), FBCR (price per square foot)
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119278275-3b853f00-bbf2-11eb-9b1e-e498ab1811e8.png">

- Figure eleven shows the price per square foot estimates from the GWR and FBCR models.

#### Figure 12: Prediction Interval
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/119278297-56f04a00-bbf2-11eb-8c00-564390cd0dc5.png">

- Figure twelve indicates that the uncertainty range is approximately $400,000 for all homes except ones with prices above $1 million. The model shows that small changes to training data from King County can result in substantial changes to the predicted sale price of the home. Unlike GLR or GWR, FBCR does not extrapolate. If the maximum price in the training data is $1.2 million in training, any price the model predicts above that will have high uncertainty. Also, since there are relatively few of the highest-priced houses, the uncertainty for these sorts of houses will be high.
