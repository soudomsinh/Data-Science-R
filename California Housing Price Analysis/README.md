## Summary

California is one of the most volatile housing markets in the US. Housing prices have reached record highs and then crashed, leaving many residents and homeowners overwhelmed by their mortgages. This market volatility has made it difficult for first-time home buyers to enter the market and has caused many families to move out of the state in search of a more stable housing market.

In this project, I intend to present data via table, plot, and map, and I  also intend to cover codes such as summary, nrow, ncol, and so on to show the dataset's information. The goal of this project is to find the relationship between each variable and visualize it. I will analyze data on the California housing market to understand what has caused the market to be volatile. I  will examine data on home prices, population, total room, location, households and their income, migration patterns and many more to try to identify trends and factors that have contributed to the market instability. I  hope that this analysis will provide some insight into what policy changes, if any, could be made to help stabilize the California housing market. The presentation includes data visualizations of a box plot, a map, a bar plot, and two scatter plots.

The map depicts locations in California with house values for each block indicated by color. The map's goal is to provide a sense of the data set. The map is color_coded based on the range of house prices in California. The color ranges from blue to red with blue being the least expensive and red being the most expensive. As represented in the map, the least expensive area for housing prices in California is the INLAND metropolitan region of California. Houses with average prices are mostly situated along the coast of California that is not too far from the ocean.

The bar plot focuses on the house value by each level, with a percentage for each region. There are two scatter plots that demonstrate some mathematical relationship between the house value and the other two variables. The first scatter plot is about house values and household income, and the second is about facet wrap by ocean proximity. The second one depicts house values and house age in relation to ocean proximity. At the end, the conclusion is based on the information from those visualizations and group discussion.

Finally, I  created a table of statistical summary and box plot of California housing prices to see the big picture of data distribution. In summary, the statistical analysis table shows us that the average price of houses in California is 206.90 with a minimum of 15.00 and a maximum of 500.00(in thousand USD). However, the box plot gives us a better insight into the housing market in California. The box plot shows us that island is the most expensive area based on ocean proximity from the OCEAN_PROXIMITY variable in the data set. The housing prices on island are above the statistical mean of the statistical summary.

Generally, the data set has been quite informative about California's housing market in 1990. I  got this data set from Kaggle, so it's very likely that before it was used in this project, the data set had already been cleaned and manipulated. It raises doubts about possible missing values, the real skew of the median features, and how data was categorized using the OCEAN_PROXIMITY. However, the data set is still very useful, and it serves as a great illustration for exploratory analysis. With a little work in R, I  gained a good understanding of the California housing market.

The map in this analys is to show that the greater housing values I re mostly concentrated around California's major metropolitan areas like San Francisco, San Diego, and Los Angles, and some vacation areas like Lake Tahoe and Catalina Island. The OCEAN_PROXIMITY variable can be one of the factors for determining the values of houses in California; hoI ver, it is probably not a reliable indicator of the house value. In the 0-200k group of house value, the inland house has the highest percentage, then compared to other classes of house value, most of the inland houses are in the 0-200k group. If comparing houses in the area smaller than 1h drive in each group, I  found the percentage is close in each group of house values. The householder's income and house value have an approximately positive relationship, but there is a difference in distribution. There seem to be no statistical relationships betI en house age and house values. It might be because the maximum house age is 50, which is not a long time as a building age.


## Presentation

The presentation can be found [on Github](https://docs.google.com/presentation/d/12f2xh5SafPjQClul5EDFCsrc1de9qYNGGKRnJ-9Elig/edit#slide=id.g1840cd26eb2_1_12).

The leaflet map can be found at this link [on Github](presentation/Leaflet.html)

## Data 

The California housing data set was derived from Kaggle I bsite, and contains information from the 1990 California census. This data set has 20640 observations and 10 variables. This data set includes information on population, total number of bedrooms, the price of the homes, and the location of the home. 

## References

This data was initially featured in the following paper:
Pace, R. Kelley, and Ronald Barry. "Sparse spatial autoregressions." Statistics & Probability Letters 33.3 (1997): 291-297.it also in 'Hands-On Machine learning with Scikit-Learn and TensorFlow' by Aurélien Géron.
Aurélien Géron wrote:
This dataset is a modified version of the California Housing dataset available from:
Luís Torgo's page (University of Porto). The dataset can be found here
https://www.kaggle.com/datasets/camnugent/california-housing-prices
link to more information about california housing price dataset can be found here https://developers.google.com/machine-learning/crash-course/california-housing-data-description


