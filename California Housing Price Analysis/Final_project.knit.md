---
title: "Final_project"
author: "Sengdao_&_Alice "
date: "2022-10-19"
output: html_document
---


```r
#install.packages("skimr")
#install.packages("leaflet")
#install.packages("tidyverse")
#install.packages("ggplot2")
#install.packages("sf")
#install.packages("psych")
# install.packages("maps")
# install.packages("mapdata")
#install.packages("maps")
#install.packages("mapdata")
#install.packages("htmltools")
#install.packages("ggcorrplot")
#install.packages("RColorBrewer")
```


```r
library(leaflet)
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.4.0      ✔ purrr   0.3.5 
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
## ✔ readr   2.1.3      ✔ forcats 0.5.2 
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library(skimr)
library(maps)
```

```
## 
## Attaching package: 'maps'
## 
## The following object is masked from 'package:purrr':
## 
##     map
```

```r
library(mapdata)
library(ggplot2)
library(sf)
```

```
## Linking to GEOS 3.8.0, GDAL 3.0.4, PROJ 6.3.1; sf_use_s2() is TRUE
```

```r
library(psych)
```

```
## 
## Attaching package: 'psych'
## 
## The following objects are masked from 'package:ggplot2':
## 
##     %+%, alpha
```

```r
library(htmltools) 
library(ggcorrplot)
library(RColorBrewer)
```


```r
California_housing <- read_csv("data/California_housing .csv")
```

```
## Rows: 20640 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): ocean_proximity
## dbl (9): longitude, latitude, housing_median_age, total_rooms, total_bedroom...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
# states <- map_data("states") # or state
# cali <- states %>%
#   filter(region == "california")
# link to more info about california housing price dataset https://developers.google.com/machine-learning/crash-course/california-housing-data-description
```



```r
states <- st_as_sf(map("state", plot = F, fill = TRUE))

Cali <- states[4,] # To get california
```



```r
glimpse(California_housing)
```

```
## Rows: 20,640
## Columns: 10
## $ longitude          <dbl> -122.23, -122.22, -122.24, -122.25, -122.25, -122.2…
## $ latitude           <dbl> 37.88, 37.86, 37.85, 37.85, 37.85, 37.85, 37.84, 37…
## $ housing_median_age <dbl> 41, 21, 52, 52, 52, 52, 52, 52, 42, 52, 52, 52, 52,…
## $ total_rooms        <dbl> 880, 7099, 1467, 1274, 1627, 919, 2535, 3104, 2555,…
## $ total_bedrooms     <dbl> 129, 1106, 190, 235, 280, 213, 489, 687, 665, 707, …
## $ population         <dbl> 322, 2401, 496, 558, 565, 413, 1094, 1157, 1206, 15…
## $ households         <dbl> 126, 1138, 177, 219, 259, 193, 514, 647, 595, 714, …
## $ median_income      <dbl> 8.3252, 8.3014, 7.2574, 5.6431, 3.8462, 4.0368, 3.6…
## $ median_house_value <dbl> 452600, 358500, 352100, 341300, 342200, 269700, 299…
## $ ocean_proximity    <chr> "NEAR BAY", "NEAR BAY", "NEAR BAY", "NEAR BAY", "NE…
```


```r
California_housing <- California_housing %>% 
mutate(house_value_thousand = median_house_value / 10^3) 
head(California_housing)
```

```
## # A tibble: 6 × 11
##   longitude latitude housing_m…¹ total…² total…³ popul…⁴ house…⁵ media…⁶ media…⁷
##       <dbl>    <dbl>       <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
## 1     -122.     37.9          41     880     129     322     126    8.33  452600
## 2     -122.     37.9          21    7099    1106    2401    1138    8.30  358500
## 3     -122.     37.8          52    1467     190     496     177    7.26  352100
## 4     -122.     37.8          52    1274     235     558     219    5.64  341300
## 5     -122.     37.8          52    1627     280     565     259    3.85  342200
## 6     -122.     37.8          52     919     213     413     193    4.04  269700
## # … with 2 more variables: ocean_proximity <chr>, house_value_thousand <dbl>,
## #   and abbreviated variable names ¹​housing_median_age, ²​total_rooms,
## #   ³​total_bedrooms, ⁴​population, ⁵​households, ⁶​median_income,
## #   ⁷​median_house_value
```



```r
California_housing %>% 
  ggplot(aes(x= house_value_thousand))+
  geom_histogram()+
  labs(x = " Median house value (in thoudsand USD)",
       y = "")
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="Final_project_files/figure-html/unnamed-chunk-7-1.png" width="672" />

```r
California_housing %>% 
  ggplot(aes(x= house_value_thousand))+
  geom_density()+
  labs(x = " Median house value (in thoudsand USD)",
       y = "")
```

<img src="Final_project_files/figure-html/unnamed-chunk-7-2.png" width="672" />

```r
California_housing %>% ggplot( aes(x= house_value_thousand, fill= ocean_proximity))+
  geom_histogram(aes(y=..density..), alpha=0.5,
  position = "identity")+
  geom_density(alpha=.3)+
  facet_grid(ocean_proximity~.)+
  theme_bw()
```

```
## Warning: The dot-dot notation (`..density..`) was deprecated in ggplot2 3.4.0.
## ℹ Please use `after_stat(density)` instead.
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="Final_project_files/figure-html/unnamed-chunk-7-3.png" width="672" />


```r
par(mfrow=c(2,3))
hist(California_housing$house_value_thousand, breaks = 20, col="skyblue", main="House values in California", xlab = "Price range")
hist(California_housing$median_income, breaks = 20, col="skyblue", main="Median income in California", xlab = "income ...")
hist(California_housing$housing_median_age, breaks = 20, col="skyblue", main="housing_median_age in California", xlab = "Age range")
hist(California_housing$latitude, breaks = 20, col="skyblue", main="Latitude", xlab = "")
hist(California_housing$longitude, breaks = 20, col="skyblue", main="Longitude", xlab = "")
```

<img src="Final_project_files/figure-html/unnamed-chunk-8-1.png" width="672" />




```r
California_housing %>% 
  ggplot(aes(x= ocean_proximity, y= house_value_thousand))+
  geom_boxplot()+
  geom_hline(aes(yintercept = mean(house_value_thousand)), color = "blue", linetype = "dashed", lwd = 1)+
  stat_summary(fun = mean, geom = "point", col = "red") +  # Add points to plot
  stat_summary(fun = mean, geom = "text", col = "red",     # Add text to plot
               vjust = 1.5, aes(label = paste("Mean:", round(..y.., digits = 1)))) +
  labs(title = "box plot of house values in California",
       subtitle = "by ocean proximity",
       x = "ocean proximity",
       y= " house values in thoudsand USD")
```

```
## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
## ℹ Please use `linewidth` instead.
```

<img src="Final_project_files/figure-html/unnamed-chunk-9-1.png" width="672" />

```r
ggsave("myboxplot.png")
```

```
## Saving 7 x 5 in image
```


```r
mytable <- California_housing %>% 
  summarise(avg_house_values = mean(house_value_thousand),
            avg_house_age = mean(housing_median_age),
            avg_income = mean(median_income),
            Max_house_values = max(house_value_thousand),
            Max_house_age = max(housing_median_age),
            Max_income = max(median_income),
            Min_house_values = min(house_value_thousand),
            Min_house_age = min(housing_median_age),
            Min_income = min(median_income),
            Median_house_values = median(house_value_thousand),
            Median_house_age = median(housing_median_age),
            Median_income = median(median_income),)

glimpse(mytable)
```

```
## Rows: 1
## Columns: 12
## $ avg_house_values    <dbl> 206.8558
## $ avg_house_age       <dbl> 28.63949
## $ avg_income          <dbl> 3.870671
## $ Max_house_values    <dbl> 500.001
## $ Max_house_age       <dbl> 52
## $ Max_income          <dbl> 15.0001
## $ Min_house_values    <dbl> 14.999
## $ Min_house_age       <dbl> 1
## $ Min_income          <dbl> 0.4999
## $ Median_house_values <dbl> 179.7
## $ Median_house_age    <dbl> 29
## $ Median_income       <dbl> 3.5348
```


```r
mytable %>% 
  pivot_longer(avg_house_values: Median_income, names_to = "Names", values_to = "Value") 
```

```
## # A tibble: 12 × 2
##    Names                 Value
##    <chr>                 <dbl>
##  1 avg_house_values    207.   
##  2 avg_house_age        28.6  
##  3 avg_income            3.87 
##  4 Max_house_values    500.   
##  5 Max_house_age        52    
##  6 Max_income           15.0  
##  7 Min_house_values     15.0  
##  8 Min_house_age         1    
##  9 Min_income            0.500
## 10 Median_house_values 180.   
## 11 Median_house_age     29    
## 12 Median_income         3.53
```



```r
describe(California_housing)
```

```
##                      vars     n      mean        sd    median   trimmed
## longitude               1 20640   -119.57      2.00   -118.49   -119.52
## latitude                2 20640     35.63      2.14     34.26     35.51
## housing_median_age      3 20640     28.64     12.59     29.00     28.49
## total_rooms             4 20640   2635.76   2181.62   2127.00   2294.56
## total_bedrooms          5 20433    537.87    421.39    435.00    471.44
## population              6 20640   1425.48   1132.46   1166.00   1256.51
## households              7 20640    499.54    382.33    409.00    441.20
## median_income           8 20640      3.87      1.90      3.53      3.65
## median_house_value      9 20640 206855.82 115395.62 179700.00 192773.00
## ocean_proximity*       10 20640      2.17      1.42      2.00      1.96
## house_value_thousand   11 20640    206.86    115.40    179.70    192.77
##                            mad      min       max     range  skew kurtosis
## longitude                 1.90  -124.35   -114.31     10.04 -0.30    -1.33
## latitude                  1.82    32.54     41.95      9.41  0.47    -1.12
## housing_median_age       14.83     1.00     52.00     51.00  0.06    -0.80
## total_rooms            1181.63     2.00  39320.00  39318.00  4.15    32.62
## total_bedrooms          240.18     1.00   6445.00   6444.00  3.46    21.98
## population              652.34     3.00  35682.00  35679.00  4.94    73.53
## households              223.87     1.00   6082.00   6081.00  3.41    22.05
## median_income             1.58     0.50     15.00     14.50  1.65     4.95
## median_house_value   101409.84 14999.00 500001.00 485002.00  0.98     0.33
## ocean_proximity*          1.48     1.00      5.00      4.00  1.02    -0.45
## house_value_thousand    101.41    15.00    500.00    485.00  0.98     0.33
##                          se
## longitude              0.01
## latitude               0.01
## housing_median_age     0.09
## total_rooms           15.19
## total_bedrooms         2.95
## population             7.88
## households             2.66
## median_income          0.01
## median_house_value   803.22
## ocean_proximity*       0.01
## house_value_thousand   0.80
```




```r
California_housing %>% 
  ggplot(aes(x = longitude,
             y = latitude,
             color = house_value_thousand))+
  geom_point(alpha = 1)+
  scale_color_gradient(low="dodgerblue1", high="firebrick1")+
  labs(title = "Location of the houses with it value",
       subtitle = "in California Sate",
       x = "Longitude", y = "Latitude",
       colour = "Median house value 
       in thoudsand")
```

<img src="Final_project_files/figure-html/unnamed-chunk-13-1.png" alt="This is the map of CA with median house value and their locations, and color shows the value of house, from red to blue value become less. The size of points mean the distance between ocean and house. In the map, the higher house values concentrate around California metropolitan area like San Francisco, Los Angeles, San Diego to a lesser extent Sacramento." width="672" />

```r
ggsave("calimap1.png")
```

```
## Saving 7 x 5 in image
```

```r
California_housing %>% 
  ggplot(aes(x = longitude,
             y = latitude,
             color = ocean_proximity))+
  geom_point(alpha = 0.5)+
  labs(title = "Location of the houses with its value",
       subtitle = "in California Sate",
       x = "Longitude", y = "Latitude",
       colour = "Ocean proximity 
            from the house")
```

<img src="Final_project_files/figure-html/unnamed-chunk-13-2.png" alt="This is the map of CA with median house value and their locations, and color shows the value of house, from red to blue value become less. The size of points mean the distance between ocean and house. In the map, the higher house values concentrate around California metropolitan area like San Francisco, Los Angeles, San Diego to a lesser extent Sacramento." width="672" />

```r
ggsave("calimap2.png")
```

```
## Saving 7 x 5 in image
```

```r
us_map<-subset(map_data("state"), region == "california")
cali_map <- ggplot(data = us_map, mapping = aes(x = long, y = lat)) + 
  coord_fixed(1.3) + 
  geom_polygon(colour="black", fill="white")

cali_house_map = cali_map+
  geom_point(data = California_housing, aes(x= longitude, 
                                            y= latitude, 
                                            color= house_value_thousand, 
                                            size = ocean_proximity), alpha=0.4)+
  theme()+
  xlab("Longitude")+ylab("Latitude")+
  ggtitle("Map of California: Ocean proximity and House Value")+
  scale_color_gradient(low="dodgerblue1", high="firebrick1")+
  geom_text(x=-122.5, y=37.5, label= "San Francisco")+
  geom_text(x=-118.2, y= 34.0, label = "Los Angeles")+
  geom_text(x=-121.4, y= 38.5, label = "Sacramento")+
  geom_text(x=-117.1, y= 32.7, label = "San Diego")+
  geom_text(x=-119.7, y= 36.7, label = "Fresno")+
  geom_text(x=-124.1, y= 40.8, label = "Eureka")+
  geom_text(x=-120.0, y= 39.0, label = "Lake Tahoe")+
  geom_text(x=-118.4, y= 33.3, label = "Santa Catalina")+
  geom_text(x=-117.8, y= 33.6, label = "Balboa Island")
ggsave("calimap3.png")
```

```
## Saving 7 x 5 in image
```

```
## Warning: Using size for a discrete variable is not advised.
```

```r
cali_house_map
```

```
## Warning: Using size for a discrete variable is not advised.
```

<img src="Final_project_files/figure-html/unnamed-chunk-13-3.png" alt="This is the map of CA with median house value and their locations, and color shows the value of house, from red to blue value become less. The size of points mean the distance between ocean and house. In the map, the higher house values concentrate around California metropolitan area like San Francisco, Los Angeles, San Diego to a lesser extent Sacramento." width="672" />



```r
labels <- sprintf("Median house price: $%gk<br/> Location of houses: %s",
                  California_housing$house_value_thousand, 
                  California_housing$ocean_proximity) %>%  
                  lapply(htmltools::HTML)

pal_ocean <- colorFactor(palette = "OrRd", domain = California_housing$ocean_proximity, levels = unique(California_housing$ocean_proximity))
```



```r
leaflet(California_housing) %>% 
  addProviderTiles(providers$OpenStreetMap) %>%
  addCircleMarkers(lng = ~longitude, lat = ~latitude, clusterOptions = markerClusterOptions(),
                   popup =  labels,
                   color = ~pal_ocean(California_housing$ocean_proximity)) %>%
  addPolygons(data = Cali) %>%
  addLegend(position = "bottomright",
          pal = pal_ocean,
         values = ~California_housing$ocean_proximity,
        title = "Ocean Proximity",
       opacity = 1)
```

```{=html}
<div id="htmlwidget-aab1b2dcd053914967e2" style="width:672px;height:480px;" class="leaflet html-widget"></div>
```




```r
California_housing %>% 
  mutate(area_of_value = case_when(house_value_thousand < 200 ~ "0-200k", 
                                   house_value_thousand >= 200 & house_value_thousand < 300 ~ "200-300k",
                                   house_value_thousand >= 300 & house_value_thousand < 400 ~ "300-400k",
                                   house_value_thousand >= 400 & house_value_thousand < 500 ~ "400-500k",
                                   house_value_thousand >= 500 ~ "500k(≥)")) %>% ggplot(aes(x = fct_rev(area_of_value), fill= fct_infreq(ocean_proximity)))+
  geom_bar()+
  coord_flip()+
  labs(title = "House value by the ocean proximity",
       subtitle = "in California Sate",
       x = "House value(kUSD)", y = "Number of houses",
       fill = "Ocean pproximity")
```

<img src="Final_project_files/figure-html/unnamed-chunk-16-1.png" alt="The bar plot shows the distribution of house value by ocean proximity and the house value was been group by “0-200k”,  “200-300k”, “300-400k”, “400-500k”, and over 500k (USD). In the 0-200k level of house value the inland house has the highest percentage, then comparing to other level of house value the most of inlands house are in the 0-200k level. If comparing the houses which are smaller than 1h drive in each level, we found the percentage is close in each level." width="672" />


```r
California_housing %>% 
  mutate(alpha_loc = case_when(ocean_proximity == "ISLAND" ~ 1,
                           TRUE ~ 0.1)) %>%
  ggplot(aes(x= median_income, 
             y = house_value_thousand, 
             color = ocean_proximity))+
  geom_point(aes(alpha = alpha_loc))+
  guides(alpha = "none")+
  scale_y_log10()+
  geom_smooth(color = "darkorchid1")+
  facet_wrap(~ocean_proximity)+
  labs(title = "Relationship of median income of households and house value",
       subtitle = "By Oceab Proximity",
       x = "Median income", 
       y = " Median house value (in thoudsand USD)",
       color = "Ocean Proximity")
```

```
## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'
```

```
## Warning: Computation failed in `stat_smooth()`
## Caused by error in `smooth.construct.cr.smooth.spec()`:
## ! x has insufficient unique values to support 10 knots: reduce k.
```

<img src="Final_project_files/figure-html/unnamed-chunk-17-1.png" alt="Scatterplot shows relationship of median income of householders and house value by ocean proximity. The householders income and house value has approximately positive relationship(Except island houses, the data of island house is not enough for find relationship), but there is a difference in distribution and comparing to inland and others it start increasing faster than others." width="672" />






```r
California_housing %>% 
  mutate(alpha_loc = case_when(ocean_proximity == "ISLAND" ~ 1,
                           TRUE ~ 0.1)) %>%
  ggplot(aes(y= house_value_thousand, 
             x= housing_median_age, 
             color = ocean_proximity))+
  geom_point(aes(alpha = alpha_loc))+
  guides(alpha = "none")+
  geom_smooth(color = "darkorchid1")+
  facet_wrap(~ocean_proximity)+
  labs(title = "Relationships of House value and House age",
       subtitle = "by the ocean proximity",
       y = "Median house value (in thoudsand USD)", 
       x = "Median house age",
       color = "Ocean Proximity")
```

```
## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'
```

```
## Warning: Computation failed in `stat_smooth()`
## Caused by error in `smooth.construct.cr.smooth.spec()`:
## ! x has insufficient unique values to support 10 knots: reduce k.
```

<img src="Final_project_files/figure-html/unnamed-chunk-18-1.png" width="672" />




```r
California_housing %>% 
  ggplot(aes(x= households, y = total_rooms))+
  geom_point(alpha = 0.5)+
  geom_smooth(color = "darkorchid1")+
  facet_wrap(~ocean_proximity)
```

```
## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'
```

```
## Warning: Computation failed in `stat_smooth()`
## Caused by error in `smooth.construct.cr.smooth.spec()`:
## ! x has insufficient unique values to support 10 knots: reduce k.
```

<img src="Final_project_files/figure-html/unnamed-chunk-19-1.png" width="672" />





```r
corr=cor(na.omit(California_housing[1:9]))

cali_house_corrplot=ggcorrplot(corr,
           type="upper",
           outline.col="lightgrey",
           ggtheme = ggplot2::theme_gray,
           title= "Correlation Between\n Housing Features",
          colors = c("#6D9EC1", "white", "#E46726"),
          lab = TRUE) # add the numeric values on the plot
ggsave("cali_house_corrplot.png")
```

```
## Saving 7 x 5 in image
```

```r
cali_house_corrplot
```

<img src="Final_project_files/figure-html/unnamed-chunk-20-1.png" alt="Correlation chart with each variables shows by color and number. From 1 to 0 to -1, it’s from red to white to blue. The bigger number means has higher positive correlation, negative number means negative correlation between two variables. In the variables of house value is shows 0.69 related with householder’s income, and 0.13 related with total rooms, 0.11 related with  house age, and for other variables are close to 0." width="672" />
