# R_for_Data_Science
This repository contains my practice of exploring a coffee dataset, finding data-driven insights, answering questions, and creating visualizations using R.

## Introduction

I got into ‘specialty coffee’ over the pandemic and immediately took interest in the first coffee dataset I could find. ‘Specialty coffee’ is used to refer to coffee that is graded 80 points or above on a 100 point scale and requires being graded by a certified coffee taster. With this dataset, my main goal is to find out which country produces the most coffee beans and which country produces the best coffee beans. From the countries with the best coffee beans, I will analyze for any common traits or patterns and how they compare with one another.

## Packages
I will be using the following packages in R.

```{r}
install.packages("tidytuesdayR") 
library(tidytuesdayR) 
tt_available()
library(tidyverse)
library(dplyr)
library(ggplot2)
library(readr)
```

## Data Description

From Tidy Tuesday, and courtesy of Data Scientist James LeDoux, the coffee dataset I will be exploring is collected from the Coffee Quality Institute’s review pages in 2018. 

```{r}
#load the dataset 
coffee_ratings <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-07-07/coffee_ratings.csv')
```

The dataset contains 1339 rows and 43 columns. What I noticed first in this dataset was that the species of arabica beans outweighed robusta beans heavily with 1005 arabica beans and only 8 robusta beans. I would usually remove the 8 beans but will leave them for this data analysis as our interest is mainly in the countries of origin and it will not impact our findings. Therefore, after filtering out missing values in the rows of the variables of interest, the dataset I will be using will contain 1013 rows and 15 columns.

The variables of interest in the dataset are:
* Species: species of coffee bean- arabica or robusta (species)
* Country of origin: country of where the bean came from (country_of_origin)
* Altitude: the mean altitude the bean is grown (altitude_mean_meters)
* Processing method: method for processing the bean (processing_method)
* Total cupping points: the total graded score (total_cup_points)
* Aroma: grade on the aroma of coffee (aroma)
* Flavor: grade on the flavor of coffee (flavor)
* Aftertaste: grade on the aftertaste of coffee (aftertaste)
* Acidity: grade on the acidity of coffee (acidity)
* Body: grade on how the body of coffee tastes (body)
* Balance: grade on how balanced the coffee tastes (balance)
* Uniformity: grade on how uniform the coffee tastes (uniformity)
* Cleanliness: grade on how clean the coffee tastes (clean_cup)
* Sweetness: grade on the sweetness of coffee (sweetness)
* Cupper Points: grade on how the grader rates the cup of coffee (cupper_points)

  

## Results

From the dataset, I ended up removing outliers where the count of countries were below 15 and were not useful information for my goals. As a result, the top three countries of coffee beans that were graded the most are from Mexico with a count of 225, Guatemala with a count of 151, and Colombia with a count of 125, which could imply that these are the countries where specialty coffee beans are produced most or could also imply that they are a common favorite to be rated by coffee graders (see R code and figure below).

```{r}
# compute count of countries and arrange by desc
ggplot(df2, aes(x = fct_rev(fct_infreq(country_of_origin)))) + 
  geom_bar() + 
  xlab("Country of Origin") +
  coord_flip()
```
![](https://github.com/njeanette03/R_for_Data_Science/blob/main/images/count%20of%20coffees%20and%20origins.png)




However, the top three countries with the highest cupping scores appear to be Ethiopia with a score of 85.94, Kenya with a score of 84.27, and Uganda with a score of 84. The countries with the least favorable score were Taiwan, Mexico, and Honduras (see R code and table below).

```{r}
# compute number of countries and mean total cup score

df2 |> group_by(country_of_origin) |> 
  summarize(
    count = n(),
    total_cup_points = mean(total_cup_points)
  ) |> 
  filter(count > 15) |>
  arrange(desc(total_cup_points))
```

![](https://github.com/njeanette03/R_for_Data_Science/blob/main/images/highest%20cup%20score%20by%20country.png)



Analyzing the top 3 countries with the highest score, it seems that the most common processing method for Ethiopia is Natural / Dry, while the most common processing method for Kenya and Uganda are Washed / Wet (see table below).

![](https://github.com/njeanette03/R_for_Data_Science/blob/main/images/highest%20rated%20countries%20and%20processing%20method.png)




Comparing Ethiopia, Kenya, and Uganda to one another, Ethiopia grows their beans at the highest average altitude between the three. It is believed that coffee beans grown at a higher altitude will result in better taste, however Kenya has a lower altitude than Uganda but a higher cup score overall (see R code and table below).

```{r}
#compute top 3 countries mean, altitude, and grades

df2 |> 
  select(country_of_origin, altitude_mean_meters, aroma, flavor, aftertaste, acidity, body, balance, uniformity, clean_cup, sweetness, cupper_points) |>
  filter(country_of_origin == "Ethiopia" | country_of_origin == "Uganda"| country_of_origin == "Kenya") |>
  group_by(country_of_origin) |>
  summarize(
    altitude = mean(altitude_mean_meters),
    aroma = mean(aroma),
    flavor = mean(flavor),
    aftertaste = mean(aftertaste),
    acidity = mean(acidity),
    body = mean(body),
    balance = mean(balance),
    uniformity = mean(uniformity),
    clean_cup = mean(clean_cup),
    sweetness = mean(sweetness),
    cupper_points = mean(cupper_points) ) 
```
![](https://github.com/njeanette03/R_for_Data_Science/blob/main/images/top%20countries%20and%20average%20grade%20and%20altitude.png)



Other grade points that stand out are Ethiopia's strengths in aroma, flavor, aftertaste, acidity, and balance that may have contributed to it being the country with the highest total cupping score. With Uganda, the grades that stand out are a perfect score of uniformity, cleanliness, and sweetness.


## Conclusion

The countries that produce the most rated specialty coffee beans are Mexico, Guatemala, and Colombia. Countries that produce the most beans do not equate to the best rating. The best rated countries are Ethiopia, Uganda, and Kenya. The next time you are unsure of which coffee beans to buy, according to this dataset, the safest bet would be to try going for coffee beans which are produced from Ethiopia. However, if you are looking for coffee beans that embody sweetness, uniformity, and cleanliness, the safest bet would be to try going for coffee beans from Uganda.
