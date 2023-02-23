# Workplace-Attendance-Forecasting

This repo shows how you can leverage facebook prophet package to forecast workplace attendance.

📈 Forecasting workplace attendance allow businesses to right-size their investment in office space with confidence. It can also be an input for other types of Corporate forecastings i.e. number of meals to be served at the corporate restaurant.

## Repository structure

The repository is structured that way:
- 01 Data Scraping from OpenDataParis
- 02 Workplace Attendance dataset
- 03 Workplace Attendance Forecasting

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

## Original Dataset (read or go directly to final dataset below)

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
    
  
