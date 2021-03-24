# API - Frequently asked questions and answers

On this page you will find some of the more common Q&A’s, the target demographic is developers integrating with the Employee API.

## What are ojects that support timelines? 
  Visma.net Employee API supports objects that can have multiple values in different time frames, called timelines.  
  Timeline is an option to manage historic, current and future values for various supported objects or values.</br>
  When retreiving data, without any filters, you will always see 
  <ul>
  <li>historic values - values that have been defined in the past (in case there have been changes)</li>
  <li>active (current) values in effect today</li> 
  <li>future values - values that will be applied in the future (in case have been defined)</li></br>
  </ul></br>
  activeStart and activeEnd are date items expressed as string values. 
  <ul>
  <li>activeStart - is a date value indicating when value takes effect/ becomes active</li>
  <li>activeEnd - is a date value indicating when value takes effect/ becomes active</li> 
  </ul></br>
  In order to correctly update a value with timeline support, activeEnd needs to be defined for current active value. New value has to have activeStart defined.  

Examples of timeline usage
1. Oject timeline upon new employee creation, when object timeline values are null. This means that the value is active since the begging of time and has no end date
    ```json
    "salaryInformation": [
      {
        "salaryType": "Period",
        "monthlySalary": "4000"
      }
    ]
    ```
2. Employee with current salary (valid untill April 30th, 2021) and a new salary that will become in effect starting with May 1st, 2021
    ```json
    "salaryInformation": [
      {
        "salaryType": "Period",
        "monthlySalary": "4000",
        "activeEnd": "2021-04-30"
      },
      {
        "salaryType": "Period",
        "monthlySalary": "5000",
        "activeStart": "2021-05-01"
      }
    ]
    ```
  Timelines cannot have overlapping periods and cannot have gaps. One value ends on a certain date, the new value has to start with the next day. </br>
  Pensions is an exception where there can be multiple pensions defined for an employee, where timeline can overlap. 
  
---

