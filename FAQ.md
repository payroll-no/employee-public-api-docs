# API - Frequently asked questions and answers

On this page you will find some of the more common Q&A’s, the target demographic is developers integrating with the Employee API.

## What are ojects that support timelines? 
  Visma.net Employee API supports objects that can have multiple values in different time frames, called timelines.  
  Timeline is an option to manage historic, current and future values for various supported objects or values.</br>
  When retreiving data, without any filters, you will always see 
  - historic values - values that have been defined in the past (in case there have been changes)</li>
  - active (current) values in effect today</li> 
  - future values - values that will be applied in the future (in case have been defined)</li></br>
  </br>
  activeStart and activeEnd are date items expressed as string values. 
  
  - activeStart - is a date value indicating when value takes effect/ becomes active. Null means infinity. From what date this configuration of the value is active.</li>
  - activeEnd - is a date value indicating when value losses effect or becomes inactive. Null means infinity. To what date this configuration of the value is active</li> 
  </br>
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
2. Assuming that it is March, 2021, employee with current salary (valid untill April 30th, 2021) and a new salary that will become in effect starting with May 1st, 2021
    ```json
    "salaryInformation": [
      {
        "salaryType": "Period",
         "monthlySalary": "3000", //Historic value that was active untill December 13th, 2020
         "activeEnd": "2020-12-13"
      },
      {
        "salaryType": "Period",
        "monthlySalary": "4000", //Currently active timeline value assuming that it is March, 2021
        "activeStart": "2020-12-14",  
        "activeEnd": "2021-04-30" //Current value will expire in the end of April 
      },
      {
        "salaryType": "Period",
        "monthlySalary": "5000", //Future timeline value
        "activeStart": "2021-05-01"
      }
    ]
    ```
  Some objects can have overlapping periods and/or can gaps. 
  Examples:
  1. Pensions is an exception where there can be multiple pensions defined for an employee, where timeline can overlap. 
  2. Postitions is an object that can have gaps in timeline. When position ends, employee's employment is terminated. Employee later can be rehired which creates a gap in a timeline.
  
---

