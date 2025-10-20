# flight-price-prediction
The project aims to identify underlying patterns and interactions among key variables influencing airfare, providing more robust insights than traditional analyses and supporting strategic decision-making in the airline industry.

**The following mainly introduce what we have done in this project. Please see our results and discussion in the report attached.**

## Data description
The data for our research is sourced from the  EaseMyTrip Flight Fare Details 2020 on Kaggle. The dataset contains flight fare information collected through web scraping from easemytrip.in for the period January 1, 2020, to February 29, 2020, with a total of 1,794,624 records. It includes fields such as flight operator, flight number, departure and arrival time, layovers, number of stops, and fare prices.

## Data Processing
Based on the original variables, we created some new features to help our analysis:
- Total Minutes: Total duration of the flight in minutes.
- Distance: Distance between the departure and arrival locations in kilometres.
- IsWeekend: A binary feature indicating whether the departure date falls on a weekend (Friday, Saturday, or Sunday).
- If Holiday: A binary feature indicating whether the arrival date is a holiday 1.
- Is Low Cost: A binary variable identifying whether the airline is classified as a low-cost carrier 2.
- Departure Off Peak: A binary feature indicating whether the departure doesn’t occur during peak hours (8am - 9pm).
- Arrival Off Peak: A binary feature indicating whether the arrival doesn’t occur during peak hours.

## Data Cleaning
Our dataset exhibited severe right-skewness in airfare distribution due to extremely high fares, violating assumptions of normality required by certain modeling methods. To mitigate potential bias, we employed the Interquartile Range (IQR) method to remove outliers and non-value data points, achieving a cleaner and more symmetric distribution for analysis.

## Exploratory Data Analysis 
### Univariate analysis 
- Total_Minutes: Right-skewed; 68.86% flights last 300–1,500 minutes, with 6.7% less than 3,000.
- Distance: Right-skewed; 70.94% flights are under 3,000 km, 4.66% exceed 10,000 km.
- Low_Cost_Count: 80% have no low-cost carriers; few have 1-3.
- Number-of-Stops: Most flights have 1 stop; fewer have 2; very few are nonstop or have 3 stops.
- If_Weekend: 33.9% depart on weekends; 66.1% on weekdays.
- If_Holiday: 5.03% depart on holidays.
- If_Low_Cost: 24.80% are low-cost; 75.20% are not.
- Departure_Off_Peak: 30.50% depart off-peak.
- Arrival_Off_Peak: 30.72% arrive off-peak.

### Bivariate analysis
(a) Correlation between flight fares and predictors. This also supports our interaction terms: 
- Categorical variables V.S. fare
Nonstop flights show lower median fares but more high-priced outliers. Fares increase with stops; two-stop flights are the costliest. As low-cost carrier availability increases, median fares decrease, indicating price reductions due to low-cost segments.
- Numerical variables V.S. fare
Both duration and distance show a positive relationship with fare separately, where longer flights tend or longer distances generally command higher ticket prices. 
- Binary variables V.S. fare
Weekend flights and off-peak departures show minimal impact on fares. Flights with low-cost carriers notably reduce median fares compared to traditional airlines. Arrivals during off-peak hours have a wider fare distribution and higher median, suggesting greater variability.

(b) Correlations among predictors:
To ensure predictor independence, we analyzed inter-predictor correlations. Based on our constructed hit map 4, we found that IsWeekend and ifHoliday,Is-Low-Cost and distance, Total-Minutes and Number.Of.Stops, Departure.Off.Peak and Arrival.Off.Peak, Total-Minutes and ifHoliday, Total-Minutes and IsWeekend, Departure.Off.Peak and Number.Of.Stops have more significant relations. 

## Methodology
Our research contributed by explicitly integrating interaction terms, addressing Chen et al. (2015)'s call for exploring complex routes and Liu et al. (2017)'s focus on context-aware modeling. Additionally, inspired by Boruah et al. (2019), we advance Bayesian methods by employing a hierarchical Bayesian framework to better capture airfare volatility.

Our main research question is the influence of diverse variables on airline ticket pricing, specifically, how do interaction terms among factors such as dates, airline type and more impact pricing? Our project starts with Linear Regression to establish a comprehensive baseline of factors influencing airfare. Subsequently, we adopt the Hamiltonian Monte Carlo (HMC) for the Bayesian Model with all important factors. This aims to enhance our estimation accuracy.

The exploration of interaction terms through Linear Regression:
- IsWeekend:ifHoliday
     - Hypothesis: Weekend moderates holiday airfare effects. Following Koo & Mantin (2010), holiday weekends may intensify price dispersion due to higher leisure travel and varied pricing strategies.
- Is_Low_Cost:distance 
     - Hypothesis: Airline type moderates the relationship between travel distance and airfare. Building on Wehner et al. (2018), differences in pricing strategies between low-cost and traditional carriers likely affect how these airlines price flights across varying distances.
- Total_Minutes:Number.Of.Stops 
     - Hypothesis: Number of stops confounds total flight time and airfare relationships. Martínez-Val et al. (2012) demonstrate that stops significantly influence costs, implying an interdependent impact with flight duration.
- Departure.Off.Peak:Arrival.Off.Peak 
     - Hypothesis: Off-peak arrival time moderates the relationship between off-peak departure time and airfare. Building on Smith and Johnson (2021), who highlight how departure and arrival times significantly influence airline pricing strategies, we posit that their combined effect uniquely impacts airfare.
- Total_Minutes:ifHoliday 
     - Hypothesis: Holidays mediate the relationship between total flight time and airfare by influencing flight selections, indirectly affecting airfare. Wen and Yeh (2017) emphasize that understanding traveller preferences during holidays helps airlines optimize pricing and scheduling.
- Total_Minutes: IsWeekend 
     - Hypothesis: The weekend indicator moderates the relationship between total flight time and airfare. Puller (2012) noted larger weekend discounts on business-heavy routes compared to leisure routes, suggesting weekend booking impacts airfare differently based on flight duration.
- Departure.Off.Peak:Number.Of.Stops 
     - Hypothesis: Off-peak departure time acts as a collider between the number of stops and airfare. Escobari (2017) found systematically higher fares during peak times, indicating that departure timing significantly influences airfare decisions related to route complexity.

We utilized ANOVA to confirm the significance of these interactions, subsequently incorporating significant terms into our Linear and Bayesian models. Model validation employed R-squared and k-fold cross-validation.

Given the relatively high RMSE observed, we further explored four advanced models: 
- Non-linear model - Capture the complex and often non-linear relationships inherent in the airfare pricing data
- Random forest - Avoid overfitting and handle complex datasets with numerous variables effectively, making it ideal for the multifaceted nature of airfare data.
- Feature engineering - Refine and create new predictors from existing data, potentially unveiling hidden patterns that more straightforward models might miss.
- Hierarchical Bayesian model - Inspired by the successful application of Bayesian methods with Kalman filter (Boruah et al., 2019), we propose a hierarchical Bayesian model to delve deeper into the data hierarchy.
