# Workplace Attendance Forecasting with Facebook Prophet

![Workplace-Attendance-Forecasting with Facebook Prophet](04-Images/WP%20attendance%20steampunk.jpg?raw=true "Workplace-Attendance-Forecasting with Facebook Prophet")

This repo shows how you can leverage facebook prophet package to forecast workplace attendance.

📈 Forecasting workplace attendance allow businesses to right-size their investment in office space with confidence. It can also be an input for other types of Corporate forecastings i.e. number of meals to be served at the corporate restaurant.

## Repository structure

The repository has the following structure:
- 01 - Data Scraping from OpenDataParis
- 02 - Workplace Attendance dataset
- 03 - Workplace Attendance Forecasting

## Workplace attendance

Worplace attendance is the presence of your employees at their designated worksite during the required/planned days. 

🏭 Workplace attendance can be measured in your Time & Attendance and/or access control solutions. 

Offsite attendances are also recorded in those solutions :

    🏡Teleworking (work from home)
    🚅 Business trips
    🎓Training off site
    ...

From an HR perspective, an employee is either on positive or negative time management.

➖ In negative time management, only the deviations to the planned work schedule are recorded: absence & specific attendances. 

➕ In positive time management, all time recording are captured: all absences and attendances (including clock times sometimes) for all planned working days. 

Your Time & Attendance solution can calculate daily counters of workplace attendance based on these time management mode and recordings and appropriate business rules.

➖ TIME MANAGEMENT:

                   WorkPlace Attendance = Planned Work Schedule

                                         ➖ Absences 

                                         ➖ Offsite Attendances 

➕ TIME MANAGEMENT:

                   WorkPlace Attendance = WorkPlace Attendances

📃 Planned Work Schedule excludes non working days such as:

    Weekend days (or days not the work cycle for shift teams).
    Public holidays.
    Rest day for partial work schedules (i.e. the weekly day off for employees with a 80% activity rate).
    
  ## Facebook Prophet
  
  There are a lot of different possibilities to address a forecasting problem:

    Classical statistical approaches (SARIMA, Exponential Smoothing, Prophet, etc..)
    Machine Learning approaches (Linear Regression, Boosting Algorithms) 
    Deep Learning approaches (RNN, LSTM, CNN, …)

In this article, we use Facebook Prophet, a forecasting package in both R and Python that was developed by Meta's data science research team (formerly Facebook).

🎯 The goal of the package is to give business users a powerful and easy-to-use tool to help forecast business results. Prophet automatically captures trends, weekday/weekend movements, and seasonal patterns. 

➡️ It also comes with built-in features such as the one that allows you to define specific holiday/event days affecting time series data. This is very useful for HR time management forecasting.

# DATASET

## Original Dataset (read or go directly to dataset below)

📌 Due to the confidentiality of HR data, it is already quite difficult to find HR datasets. It is even more complicated with HR time-series-based dataset...

             ➡️ So, I decided to create my own! 

🕵🏻 I imagined that traffic data (road/transport) could be a good approximation to workplace attendance data. 

💡 After a few web searchs, I found the "permanent road counts" dataset provided by OpenData PARIS under ODbL License. 

🇫🇷 On the Parisian network, traffic measurement is mainly carried out through electromagnetic loops implanted in the road. These data provide an overview of the traffic flow 🚕 over 3,000 road sections every hour every day. The dataset is only available for the last 13 rolling months. Historical data can be retrieved here but can not be easily scrapped...

From this dataset, I selected a subset:

    One road section in the center of Paris "4 Septembre/Richelieu", good place for work offices (and for Japanese food!). 
    One hour in the day: 09:00 AM, good time to measure road traffic related to work.

You can have a look at my workplace attendance dataset creation with Python on my GitHub here. 

    It includes web scrapping, management of missing values & unexpected outliers. 
    It also includes additional the applications of linear coefficients for weekend days, fridays & vacation periods. 
    It also introduces a monthly headcount calculation based on random distributions of newcomers and leavers per month. The corresponfing monthly ratios are then applied linearly to the workplace attendance figures. 
    
## Dataset

The final dataset contains 3 columns:

    Date (from 1 Dec 2022 to 24 Jan 2023)
    Work Place attendance (per day)
    Headcount (on that day)
    
# EXPLORATORY DATA ANALYSIS (EDA)
 
## Overview
 
 The following chart shows:

    the daily workplace attendance (in blue)
    the monthly headcount (in orange)
  
  ![Workplace attendance & headcount (Dec 21 - Jan 23)](04-Images/1-Original%20time%20series.png?raw=true "Workplace attendance & headcount (Dec 21 - Jan 23)")

💡 At first sight, we observe the following:

    a weekly seasonality.
    the impact of at least 3 holiday periods Christmas 2021, Summer 2022 and Christmas 2022.
    a yearly seasonality On December/Janaury, the only months that appear twice.
    
## Week analysis

Let's visualize the mean of workplace attendance per week day:

![Workplace attendance mean for each week day (Monday to Sunday)](04-Images/3-Week%20analysis.png?raw=true "Workplace attendance mean for each week day (Monday to Sunday)")

💡 There are limited workplace attendance during week-end days.

💡 There are slight differences between the usual working days:

    Mondays and Fridays are the days with less people in the office
    Tuesdays, Wednesdays and Thursdays with more people in the office

No big surprises here!

This dataset does no contain information on the reasons behind the absence at the work place. In your company/organization, you can get this information from your Time & Attendance solution.

You can have less people in the office on Fridays because your staff:

    🏡 work from home
    🌴 are on leaves
    🤒 are absent (sick...)
    ...
    
## Working days time series

We want to forecast the workplace attendance on working days only, so we get rid of saturdays and sundays.

Let's visualize the new time series:

![Workplace attendance & headcount (Dec 21 - Jan 23))](04-Images/2-Time%20series%20wo%20WE%20days.png?raw=true "Workplace attendance & headcount (Dec 21 - Jan 23)")

💡 Now, without weekend days, 

    it is more difficult to observe the weekly frequency
    but now we are able to observe sudden decreases during the year that are not linked to weekends. They are probably linked to public holidays on working days. 
    
# FORECASTING MODELING

Now, it is time to build several forecasting models with different parameters and find the best one according to a given metric.

In order to evaluate those models,

    we will train them on a whole year period from 1 Dec 2021 to 30 Nov 22.
    we will evaluate their workplace attendance predictions against real Workplace attendance from 1 Dec 2022 until 24 Jan 23 (end of dataset).

## 1. Baseline model with weekly & yearly seasonality

As observed during the Exploratory Data Analysis, the time series has:

    weekly seasonality
    yearly seasonality

Let's build this baseline model with these 2 seasonalities. After model training, forecasting on the last period, let's plot the forecast.

![1. Baseline model with weekly & yearly seasonality](04-Images/4-1st%20model.png?raw=true "1. Baseline model with weekly & yearly seasonality")
      
How to read the results:

    The black dots are the real values
    The blue line is the prediction
    The blue shades are the uncertainty interval

💡 The overall prediction looks quite good but we do have:

    quit a lot of real points far from the prediction (not necessarly that bad because we don't want overfitting)
    a few outliers (very far from the prediction)

In addition to the forecast plot, Prophet also provides the components plot.

![1. Baseline model with weekly & yearly seasonality - COMPONENTS](04-Images/5-1st%20model-comp.png?raw=true "1. Baseline model with weekly & yearly seasonality - COMPONENTS")

💡 From this component plot chart, we can see that:

    the workplace attendance has an overall upward trend.
    the weekly seasonality shows that the workplace attendance tends to be lower at the beginning and end of the working.
    the yearly seasonality shows different movements and we can observe more easily the possible impacts of vacations periods.

➡️ The mean absolute percent error (MAPE) for this model is 20%, meaning that on average, the forecast is off by 20% of the real workplace attendance.

## 2. Multivariate model

From an HR perspective, the headcount has an impact on the workplace attendance (predictor). So, we can add this parameter to the previous model.

➡️ It becomes a multivariate model.

![2. Multivariate model (with headcount predictor)](04-Images/6-2nd%20model%20multivariate.png?raw=true "2. Multivariate model (with headcount predictor)")
  
![22. Multivariate model (with headcount regressor) COMPONENTS)](04-Images/7-2nd%20model%20multivariate-comp.png?raw=true "2. Multivariate model (with headcount regressor) COMPONENTS")

➡️ The mean absolute percent error (MAPE) for this model is 15% which is better than the previous model (20%).

## 3. Model with public holidays & vacation periods

Besides seasonalities and additional predictors, we will now include public holidays and vacation periods to the previous model and see how it impacts the model predictions.

We add:

    FR (French) public holiday calendar
    French public vacation periods, actually France is divided into 3 areas for vacation, we use "Zone C" vacation periods for Paris

Let's plot the forecast:

![3. Model with public holidays & vacation periods)](04-Images/8-3rd%20model%20with%20public%20holidays.png?raw=true "3. Model with public holidays & vacation periods")

💡 It's getting better:

    the vacaction periods are better fitted
    the public holidays as well, the blue prediction line is closer to the black point now

Let's plot the components now:

![3. Model with public holidays & vacation periods COMPONENTS)](04-Images/9-3rd%20model%20with%20public%20holidays-comp.png?raw=true "3. Model with public holidays & vacation periods COMPONENTS")

➡️ The mean absolute percent error (MAPE) for this model is 12% which is better than previous models (respectively 15% and 20%).

# SUMMARY

 💡 Forecasting workplace attendance was made easy by using Prophet. It was indeed quite easy and fast to implement the baseline model + extra parameters such as:

    Current headcount as an additional predictor
    Public holidays & vaction periods

👉 By the way, in production, forecasting the headcount will help forecasting the workplace attendance. You might get some help from your Workforce Planning solution if you have any.

➡️ Results were interpretable, thank to the components plot

➡️ Results were quite good.

Now, the dataset was not large enough to predict accurately the yearly seasonality and Prophet is known to work best with several seasons of historical data (3 years would be best).

🛠 It is also possible to dive into additional model fine tuning.

🚀As always, the challenge will be to put the model in production and follow how it behave. Such models would have been very impacted by COVID or post COVID times.

🕑 The other usual challenge is the data quality but usually Time & Attendance data have a high quality because their recording have legal and payroll impacts and they are closely monitored by HR departments. I can tell by experience as I have been supporting companies/organizations for 8 years as @SAP HCM consultant, expert on Time management for clients such as Nestlé, BNP Paribas Personal Finance or Volvo Finland Ab.

📊 Finally, this Prophet models should also be compared with others such as SARIMA, tree based models or neural network models. 
