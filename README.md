# Predicting Airbnb Listing Price
<div align="center">
  
![image](https://user-images.githubusercontent.com/90875339/192396927-6b705b6f-220e-41b9-943f-040d166c4686.png)
</div>

## Project Overview
This project aims to understand determinants of Airbnb listing price, using the listing data with 236,823 unique listings in 31 cities and 19 states in the United States between 2022-06-08 to 2022-09-16.

## Project Motivation and Research Question
The sharing economy has boomed in the past decade aimd the concern on sustainability and embrace of sharing among younger generation. One of the unicorns in the sharing economy is Airbnb -- the largest accommodation/lodge sharing platform in the world. Airbnb was founded in 2008, and grew phenomenally. To date, it is operated in over 220 countries and regions and in over 100,000 cities with 5.6 million actie listings and 150 million users. More importantly, it contributes to the prosperity of local economy, for example, alone in 2019, the host-guest community created over $117 billion economic impact across 30 citites([Meyer, 2022](https://www.thezebra.com/resources/home/airbnb-statistics/)).

It is not surprising that there is burgeoning research on Airbnb and price is always the centre of any inquiries, because it is so critical to the platform, hosts and guests. Price is the major determinant of the lisiting's attractiveness, and guests' accommodation decisions, such as whether to choose Airbnb over other competitive accommodation options (e.g., traditional hotels, other platforms). This ultimately affects hosts' income and their future engagement in Airbnb, the health of the host-guest sharing community, as well as the platform's revenue.

**To investigate determinants of price, this project collects 236,823 unique listing information in 31 cities and 19 states in the United States between 2022-06-08 to 2022-09-16 from [Kaggle](http://insideairbnb.com/get-the-data/).It focuses on how enviromental-related factors (e.g., regional difference), host-realted factors (e.g., host tenure) and the accomodation-related factors (e.g., lodge capacity) affect price.**

## Project Method

### Libraries
- ```urllib```,```gzip``` and ```shutil``` for collecting and proccessing datafiles
- ```pandas``` for wrangling dataframes
- ```numpy``` for dealing with arrays and matrices
- ```sklearn``` and ```statmodels``` for modeling
- ```matplotlib``` and ```seaborn```for virsualizing

### Data Collection and Wrangling
Data were collected from [Kaggle](http://insideairbnb.com/get-the-data/) based urls. Only US data were collected in this project. Data dictionary and other supplementary information can also be found on the page.

The critical steps in data wrangling are:
1. Drop duplicated listings (N = 4577)
2. Deal with missing values: 1) impute missing values with the mean for quantitative rating-related variables and host-related variables because the variance of these variables are small yet the missing proportion is large (15-20% are missing); 2)impute missing values with the mode for the categorical host-related variable (i.e., ```response time```), because the majority of this variable fall into one category (77%) yet the missing proportion is large (15-20% are missing); 3) drop the missing values for other variables where the missing proportion is small (less than 2%) and the loss of data is negligible
3. Create ```log price``` as the alternative outcome variable and drop the top 1% of the outliers of price (N = 2392) to address the severe outlier and skewness issues.
4. Create ```host tenure``` as a host-related predictor in the unit of years, months and days.
5. Recode ```property type``` to reduce the the number of categories by grouping rare types into "Other" category.

### Data Analysis
1. Model-free analysis: bivariate correlations for quantitative variables and omnibus ANOVA test for cateogrical variable to see if some predictors should be dropped in advance
2. Search for the best model and parameters: used cross-validation-based method to search to compare the linear regression model with/without regularization and determine the parameters.
![CV_reg](https://user-images.githubusercontent.com/90875339/192400800-10ee7a9d-5542-43a5-a39a-3bfb496449b2.png)
3. OLS model without regularization was selected and used for predicting both price and log transformed price because regularization does not improve the model fit. Instead, more features can be added in the follow-up project to improve the model fit.

## Results & Key take-aways
The OLS model, which regressed 68 features on log transformed price, yield a moderate model fit (R-squared = .61). The most strongest predictors include capacity--the number of guests can be accommodated and bedrooms, room type, city, review ratings. For full results, please see this spreadsheet.

### Take-away 1
There is substaintial difference in listing price across cities. Seaside cities enjoy a significant price premium, compared to urban cities, presumably due to the view premium. This finding is consistent with the [conclusion reached using Valencian regional data](https://www.mdpi.com/2071-1050/10/12/4596).
![city_dif](https://user-images.githubusercontent.com/90875339/192404403-92559518-824d-462a-bf72-fd46c351e5c0.png)

### Take-away 2
Capacity of the accommodation measured in the maximum number of guests can be accommodated, the number of beds and the number of bedrooms, has a strong positive influence on listing price.
![capacity_dif](https://user-images.githubusercontent.com/90875339/192404350-2b88e72d-7efb-4c9f-b08a-4aa81d48419c.png)


### Take-away 3
Property type and room type are critical categorical predictors, and the difference between propery types or/and room types could be partly attributed to capacity difference (e.g., villa is the most expensive type of property, and the average accommodates of it is 7 (vs. 4 across all types)) and view difference (e.g., entire home/apt is the mostcommon room type (87%) in seaside cities with ocean view)
![room_property_dif](https://user-images.githubusercontent.com/90875339/192404296-ea064b01-3d19-4683-b2ab-a3e355280a73.png)

### Take-away 4
Most accommodation has a rating over 4 out of 5 in each of the seven rating dimensions (i.e., overall, accuracy (of the description), checkin, cleanliness, communication,location, value). Although ratings do affect listing price but its sign and magnititude vary by dimension (see the tabel below: DV = log price).

<div align="center">
  
|Dimension|overall|accuracy|checkin|cleanliness|communication|location|value|
|---------|-------|--------|-------|-----------|-------------|--------|-----|
|Std. Beta|.034|.0057|-.047|.087|-.0029|.115|-.113|

</div>

![rating_dif](https://user-images.githubusercontent.com/90875339/192404327-9504697d-139b-4965-99bf-80d756263b8f.png)

### Take-away 5
Host tenure and their badges (i.e., super host or not, identify verified or not) have weak positive influence on lisiting price.
![host_dif](https://user-images.githubusercontent.com/90875339/192404369-9b17196a-8088-468c-8da4-b72b23f16dd0.png)

### Final words
These findings may also generate to other accommodation sharing platforms (e.g., Vrbo), and to other countries.
