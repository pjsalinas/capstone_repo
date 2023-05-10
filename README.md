# capstone_repo
Analysis by: [Pedro J. Salinas](mailto:pedrojsalinas@gmail.com)

#### Repo Contents
- [Notebooks Folder](https://github.com/pjsalinas/capstone_repo/tree/main/notebooks)
<!--
- [Exploratory Notebooks](#exploratory notebooks)
- [Report](#report)
- Project Presentation(#presentation)
- Project Data(#data)
- KC Website Scrapper(#scrapper)
- References(#references)
- Project Source Code(#source)
- Project Visuals(#visuals)
- Conda Environment(#conda)
-->

#### Table of Contents

-  [Introduction](#introduction)
-  [Project Overview](#project-overview)
-  [Conda Environment](#conda-environment)
-  [Data](#data)
-  [Data Preparation](#data-preparation)
-  [Modeling](#modeling)
-  [Results](#results)
-  [Next Steps](#next-steps)
-  [References](#references)


## Introduction
The real estate market in the United States is a complex and dynamic industry that involves the buying, selling, and renting of properties such as homes, commercial buildings, and land. It is an essential part of the American economy, and it has a significant impact on the overall economic health of the country.

The real estate market in the US is highly competitive, with numerous players such as real estate agents, brokers, developers, investors, and lenders. The market is also subject to various economic factors such as interest rates, inflation, employment, and consumer confidence, which can have a significant impact on the demand and supply of properties.

One of the key factors that differentiate the real estate market in the US from other countries is the prevalence of homeownership. The US has one of the highest homeownership rates in the world, with more than two-thirds of the population owning their homes. This has created a culture of homeownership and has led to the development of various government programs and incentives to support homeownership.

Another key feature of the real estate market in the US is its regional diversity. Real estate markets in different regions of the country can vary significantly in terms of pricing, demand, and supply. For example, the real estate market in major cities such as New York, San Francisco, and Los Angeles can be very different from that in smaller cities and rural areas.

Overall, the real estate market in the US is a complex and dynamic industry that plays a vital role in the country's economy. It is a constantly evolving industry that is influenced by various economic and social factors, and it requires careful attention and analysis to understand its complexities and make informed decisions.

## Project Overview
In this project, we will be analyzing house price data from King County, Washington State using a dataset of house sold during the period of 2020-2022 to predict the sale price of houses based on various factors such as location, square footage, number of bedrooms, and bathrooms, etc. We will explore the data using statistical techniques and feature engineering to prepare it for modeling. Finally, we will build and evaluate a regression model using various algorithms and techniques to obtain the best results.

#### MyBinder
To run the code of this repo, you can use [MyBinder](https://mybinder.org) website as in:
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/pjsalinas/capstone_repo.git/HEAD) to explore the analysis.

## Data
The dataset provided to us, contains information about houses sold during 2012 - 2022 in Kings County in Washington State. The data features historical and curent details about each house sold. Information about the price that the house was sold for, square footage of living areas, number of bathrooms, bedrooms, and specific locations, and other features that are specific for each house. Each feature was categorized and are easily identifables for the name of the columns, and each house was assigned a unique number, "id", for indentification purposes. The data indicates that there are 29182 home sales documented in this file.

#### Column Names and Descriptions for King County Data Set

* `id` - Unique identifier for a house
* `date` - Date house was sold
* `price` - Sale price (prediction target)
* `bedrooms` - Number of bedrooms
* `bathrooms` - Number of bathrooms
* `sqft_living` - Square footage of living space in the home
* `sqft_lot` - Square footage of the lot
* `floors` - Number of floors (levels) in house
* `waterfront` - Whether the house is on a waterfront
  * Includes Duwamish, Elliott Bay, Puget Sound, Lake Union, Ship Canal, Lake Washington, Lake Sammamish, other lake, and river/slough waterfronts
* `greenbelt` - Whether the house is adjacent to a green belt
* `nuisance` - Whether the house has traffic noise or other recorded nuisances
* `view` - Quality of view from house
  * Includes views of Mt. Rainier, Olympics, Cascades, Territorial, Seattle Skyline, Puget Sound, Lake Washington, Lake Sammamish, small lake / river / creek, and other
* `condition` - How good the overall condition of the house is. Related to maintenance of house.
  * See the [King County Assessor Website](https://info.kingcounty.gov/assessor/esales/Glossary.aspx?type=r) for further explanation of each condition code
* `grade` - Overall grade of the house. Related to the construction and design of the house.
  * See the [King County Assessor Website](https://info.kingcounty.gov/assessor/esales/Glossary.aspx?type=r) for further explanation of each building grade code
* `heat_source` - Heat source for the house
* `sewer_system` - Sewer system for the house
* `sqft_above` - Square footage of house apart from basement
* `sqft_basement` - Square footage of the basement
* `sqft_garage` - Square footage of garage space
* `sqft_patio` - Square footage of outdoor porch or deck space
* `yr_built` - Year when house was built
* `yr_renovated` - Year when house was renovated
* `address` - The street address
* `lat` - Latitude coordinate
* `long` - Longitude coordinate

Most fields were pulled from the [King County Assessor Data Download](https://info.kingcounty.gov/assessor/DataDownload/default.aspx).

The `address`, `lat`, and `long` fields have been retrieved using a third-party [geocoding API](https://docs.mapbox.com/api/search/geocoding/). In some cases due to missing or incorrectly-entered data from the King County Assessor, this API returned locations outside of King County, WA. If you plan to use the `address`, `lat`, or `long` fields in your modeling, consider identifying outliers prior to including the values in your model.

Using these features I also created two additional variables:
* `yr_old` - represent how old is a house either from the time it was built (`yr_built`)or from the time is was renovated (`yr_renovated`).
* `city` - the name of the city where each house is located

## Modeling
![Relation Between Price and sqft_living](https://github.com/pjsalinas/capstone_repo/blob/main/images/relationship_between_price_and_sqft_living.png?raw=true)

After reading the data, the first step was to clean it and change some of the column names for better readability. Once that was done, we selected the target variable, which in this case was `price`. We then got the correlation between the numerical variables to see which ones were most strongly related to `price`. It turned out that `sqft_living` had the highest correlation with `price`, so we used it as the independent variable for our baseline model.

![House Prices vs. SQFT of Living Space in Home](https://github.com/pjsalinas/capstone_repo/blob/main/images/scatter_and_regplot_price_vs_sqft_living.png?raw=true)

Next, we ran some other models using all the numerical features and also using dummies features. We then filtered the features based on their p-values after running the linear regression analysis, and the model with the highest Adjusted R-squared was selected. We then checked if the linear regression assumptions were met for this model.

![Statistical Distribution of Floors versus Price](https://github.com/pjsalinas/capstone_repo/blob/main/images/statistical_distribution_of_floors_versus_price.png?raw=true)

However, after inspecting the `price` vs `floors` boxplot, we noticed that some outliers were not collaborating with the linear regression analysis. We ran two more models after eliminating these outliers, and once again checked whether the assumptions were met. Overall, this process involved a careful and thorough analysis of the data, followed by a systematic approach to selecting and refining our models.

## Results
Although we used a careful and thorough approach to selecting and refining our linear regression model, we found that it was not able to accurately predict the price of a house in King County. This was primarily due to the fact that some of the linear regression assumptions were not met.

One of the key assumptions of linear regression is that the relationship between the dependent variable (in this case, `price`) and the independent variables (such as `sqft_living` and `floors`) is linear. However, after inspecting the `price` vs `floors` boxplot, we found that there were some outliers that were not collaborating with the linear regression analysis. This violated the assumption of linearity and affected the accuracy of our predictions.

In addition, there were other assumptions that were not met, such as the assumption of normality (i.e. that the residuals are normally distributed) and the assumption of homoscedasticity (i.e. that the variance of the residuals is constant across all levels of the independent variables). These violations also contributed to the poor performance of our model.

Overall, it's important to remember that linear regression is a powerful tool for predicting the relationship between variables, but it relies heavily on the assumptions being met. In cases where the assumptions are not met, the accuracy of the model can suffer, and alternative methods may need to be considered.

## Next Steps
If the assumptions of the linear regression model are not met and it's not able to accurately predict house prices, there are several possible next steps to consider:

1. Explore alternative modeling techniques: There are many other modeling techniques that can be used to predict house prices, such as decision trees, random forests, support vector machines, and neural networks. These methods may be better suited for the data and may be able to capture more complex relationships between the variables.

2. Collect more data: Sometimes, the poor performance of a model can be attributed to a lack of data or a lack of diversity in the data. Collecting more data, or collecting data from different sources, may help to improve the accuracy of the model.

3. Transform the data: If the assumptions of the linear regression model are not met, it may be possible to transform the data to meet these assumptions. For example, we could transform the `floors` variable by taking the logarithm of its values to achieve a more linear relationship with `price`.

4. Remove outliers: Outliers can have a significant impact on the performance of a model. Removing outliers may help to improve the accuracy of the model and reduce the impact of violations of the linear regression assumptions.

5. Consult with domain experts: Sometimes, the insights and knowledge of domain experts can help to identify important features that were not considered in the original analysis. This can help to improve the accuracy of the model and ensure that it's capturing all relevant factors.

## References
- [Capitol Impact](https://www.ciclt.net/sn/clt/capitolimpact/gw_ziplist.aspx?FIPS=53033) - King County Zip codes, and City names
- [Eryk Lewinson](https://medium.com/towards-data-science/verifying-the-assumptions-of-linear-regression-in-python-and-r-f4cd2907d4c0) - Verifying     the Assumptions of Linear Regression in Python and R
