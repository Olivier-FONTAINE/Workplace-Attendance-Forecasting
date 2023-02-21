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

🏭 Workplace attendance can be measured in Time and Attendance and/or access control solutions. 

Offsite attendances are also recorded in those solutions, this includes :

    🏡Teleworking (work from home)
    🚅 Business trips
    🎓Training off site
    ...

From an HR perspective, an employee is either on positive or negative time management.

➖ In negative time management, only the deviations to the planned work schedule are recorded: absence & specific attendances. 

➕ In positive time management, all time recording are captured: all absences and attendances (including clock times sometimes) for all planned working days. 

Your Time and Attendance solution can calculate daily counters of workplace attendance based on these time management mode and recordings and appropriate business rules.

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
    
    
