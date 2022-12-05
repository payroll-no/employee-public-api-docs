# API changelog
## 22.12.1.0 (2022-12-05). API Specification version update to v1.1.0
- Possibility to use hourly or yearly salary has been added to Visma.net Employee Management and Visma.net Employee API. 
  With this change API Specification version is increased from **1.0.8** to **1.1.0**
  New properties are introduced for Postion, `salaryInformation` object
    - `contractSalaryType` - contract salary type indicating how salary will be calculated - hourly, monthly or yearly. 
    - `hourlySalary` - value for hourly salary
    - `yearlySalary` - value for yearly salary
    - `monthlySalary` - value for yearly salary (field from previous versions)
  Set `contractSalaryType` requires a value to be set for hourly/monthly/yearly salary property. In case `"contractSalaryType":"Hourly"` is used, `hourlySalary` value should be provided and so on. 
  
- **NB!** **Your API client needs to be updated!** Payroll administrators can submit changes either through client applications or via Visma.net Employee Management UI. In a case when Employee position data (specifically Contract Salary Type) is updated via Visma.net Employee Management UI and API client **has not been updated to v1.1.0 or later** to take advantage of new fields, following scenarios will be enforced:
  - For `Position` updates, if `contractSalaryType` is not provided AND Contract Salary Type via UI has been set to "monthly" (default option), salary change will be executed as always
  - For `Position` updates, if `contractSalaryType` is not provided AND Contract Salary Type via UI has been set to "hourly" or "yearly", salary update will be ignored (with a warning in the response). Other changes provided in the call payload will be validated and committed as always.
  - For Employee creation with position, if `contractSalaryType` is not provided then `contractSalaryType` will be set to `monthly` by default and `monthlySalary` will be used as provided in payload.
  - Job for position updates will contain a warning message that salary change has been omitted from the change. Changes to other position properties would be validated and committed. Validation result message will be provided in successful job result details with warning severity: "message: contractSalaryType has not been provided. Salary update was not performed.".
  
  
  Clients implemented **before v1.1.0** (v1.0.8 and lower) that do not specify `contractSalaryType` are impacted by two corner cases:
    - If the timeline contains any entry with a value of `Hourly` or `Yearly`, adding a timeline entry will be ignored with a successful job (with a warning in the response).
    - If the timeline contains any entry with a value of `Hourly` or `Yearly`, editing that timeline entry will fail.
  
  Clients implemented **before v1.1.0** (v1.0.8 and lower) that return `contractSalaryType` as is are impacted similarly:
    - If the timeline contains any entry with a value of `Hourly` or `Yearly`, adding a timeline entry will fail. 
    - If the timeline contains any entry with a value of `Hourly` or `Yearly`, editing that timeline entry will fail.
    - `hourlySalary` and `yearlySalary` must also be returned as is.

  Sample of response containing new contract salary type and rate properties. 

    GET /employees/{employeeId}/positions/{positionId}
    Part of response:
     ```json
          ...
          "salaryInformation": [
              {
                  "salaryType": "Period",
                  "contractSalaryType": "monthly", // new property introduced with this release
                  "monthlySalary": "10000", 
                  "hourlySalary": "61.54", // new property introduced with this release
                  "yearlySalary": "120000" // new property introduced with this release
              }
          ],
          ...
      ```

  Sample of changing contract salary type to hourly. Only rate property being used, needs to have a value set. In case `"contractSalaryType":"Hourly"` is used, `hourlySalary` value should be provided. 

    PUT /employees/{employeeId}/positions/{positionId}
    Part of payload:
     ```json
          ...
          "salaryInformation": [
              { // current salary details
                  "salaryType": "Period",
                  "contractSalaryType": "monthly", // current contract salary type set on position
                  "monthlySalary": "10000", // current salary amount set on position
                  "hourlySalary": "61.54", // current calculated salary amount on position. Property will be ignored as contractSalaryType is set to monthly
                  "yearlySalary": "120000", // current calculated salary amount on position. Property will be ignored as contractSalaryType is set to monthly
                  "activeEnd": "2022-10-20" // set last day when this salary would be in effect
              },
              { // future salary details
                  "salaryType": "Period",
                  "contractSalaryType": "Hourly", // new contract salary type
                  "hourlySalary": "65", // new hourly salary rate. other rates can be skipped as would be ignored
                  "activeStart": "2022-10-20" // set when new contract salary  type and rate should come in effect
              }
          ],
          ...
      ```

## 22.08.31  (2022-08-18)
- Added support to embed positoin resource for a single employee. Feature allows getting position data with a single GET operation on employee. 

  Sample of getting position as embedded resource for an employee. 
    ```
    GET /employees/{employeeId}?embed=(positions)
    Part of response:
    ...
      "_embedded": {
        "positions": [
            {
                "id": "1000000-0000-0000-0000-000000000",
                "tenantId": "1000000-0000-0000-0000-000000000",
                "activeStart": "2022-01-01",
                "employeeId": "{employeeId}",
                "typeOfPosition": "Ordinary",
                ...
            }
        ]
    }
    ...
    ```

## 22.7.50.0  (2022-07-13)
- Possibility to manage more information about employees have been added:
  - Maritime Workers - Maritime Employment, Type of Position has been added. Properties on Position entity
  - Border Commuter - properties on Employee entity for managing extra tax information
  - Custom salary rates - set of properaties to manage additional salary rates on Position entity

## 22.04.28  (2022-04-21)
- API Specification version update to 1.0.4. Now when using Cursor to paginate results, query parameters have to be present when requesting next page. Change has been made to Cursor to prepare for release of service performance improvements.

## 22.1.17.0 (2022-01-26)
- Employee API now supports managing Accounting Dimensions for employee positions. Functionality typically is used to assign a Cost Unit, Project or other type of accoutning dimension to a position. 
Accountin dimensions has a hierarchical structure:
    - Dimension Definition - definition of the dimensions that acts as a parent and can contain multiple dimension values.
    - Dimension Values - value of the dimension with it's defined parameters. 

  Sample of assigned dimension definitiaion and value. 
    ```
    GET /employees/{employeeId}/positions/{positionId}
    ...
      "accountingDimensions": [
        {
          "dimension": "1",
          "dimensionValues": [
            {
              "value": "2",
              "activeStart": "2022-01-25"
            }
          ]
        }
      ],
    ...
    ```
Accounting dimension objects are subject to time lines. Read more about how to work with time lines [here](https://github.com/visma-net/employee-public-api-docs/blob/main/FAQ.md#what-are-ojects-that-support-timelines).

## 21.10.26.0 (2021-10-20)
- Support for partial responses via filtering for `employee` entity has been added. Depending on your use case and payload size, you can significantly reduce network bandwidth need by supporting filtering of returned entity fields. 

  With this version we support filtering by `employee` entity's `id` and `lastChange` in format `/employees?fields=(id,lastChange)`. Support for filtering by other fields will be added with future versions. 
    ```
    GET /employees?fields=(id,lastChange)
        {
          "data": [
            {
              "id": "1000000-0000-0000-0000-000000001",
              "lastChange": "2021-10-18T12:00:35.66Z"
            }
          ]
        }
    ```

## 21.10.20.0 (2021-10-19)
- Now you can manage more information with Employee API: redundancy, prepaid sickness and prepaid other sickness fields have now been added to Position endpoint. 
    ```
    GET /employees/{employeeId}/positions
        [
          {
            "id": "1000000-0000-0000-0000-000000000",
            ...
            "absence": {
              "prepaidSickness": [
                {
                  "value": true,
                  "activeStart": "2021-10-01",
                  "activeEnd": "2021-10-10"
                }
              ],
              "prepaidOtherAbsence": [
                {
                  "value": true,
                  "activeStart": "2021-10-21",
                  "activeEnd": "2021-10-29"
                }
              ],
              "redundancy": [
                {
                  "value": "15",
                  "activeStart": "2021-10-15",
                  "activeEnd": "2021-10-19"
                }
              ]
            },
            ...
          }
        ]
    ```


## 21.10.2.0 (2021-10-06)
- Last change time stamp has been added to the employee response. Information allows to quickly detect when was the last change made to an employee.
    ```
    GET /employees
        {
        "data": [
            {
              "id": "1000000-0000-0000-0000-000000000",
              ...,
              "lastChange": "2021-10-06T09:08:24.870Z",
              ...
            }
          ]
        }
    ```

## 21.08.28 (2021-08-28)
- Version 1.0.1 is here and with it, API now supports optional sub-resource embedding and partial responses via filtering. 
  - ### Support for optional embedding of sub-resources. 
    Embedding related resources (also know as Resource expansion) is a great way to reduce the number of requests. Embedding a sub-resource can possibly look like this where an employee resource has its position items as sub-resource (`/employees?embed=(positions)`):
    ```
    GET /employees?embed=(positions)
        {
        "data": [
            {
              "id": "1000000-0000-0000-0000-000000000",
              "number": "99-1",
              ...,
              "_embedded": {
                "positions": [
                  {
                    "id": "2000000-0000-0000-0000-000000000",
                    "activeStart": "2020-02-02",
                    "typeOfPosition": "Ordinary",
                    ...
                  }
                ]
              }
            }
          ]
        }
    ```
  - ### Support for Partial responses via filtering for embedded Position entity. 
    Depending on your use case and payload size, you can significantly reduce network bandwidth need by supporting filtering of returned entity fields.
  
    With this versions we support filtering of embedded `positions` entity's `id`, `activeStart` and `activeEnd` fields. Support for filtering by other fields will be added with future versions. 

    ```
    GET /employees?embed=(positions)&fields=(positions(id,activeStart,activeEnd))

        {
        "data": [
            {
              "id": "1000000-0000-0000-0000-000000000",
              "number": "99-1",
              ...,
              "_embedded": {
                "positions": [
                  {
                    "id": "1yy0000-0000-0000-0000-000000000",
                    "activeStart": "2020-02-02",
                    "activeEnd": "2021-08-29"
                  },
                  {
                    "id": "1xx0000-0000-0000-0000-000000000",
                    "activeStart": "2021-08-30"
                  }
                ]
              }
            }
          ]
        }
    ```
  

## 21.7.0.0 (2021-07-01)
- API version 1.0.0 has been released! 
  - /v1 is the API version used in endpoint URIs.
  - "Location" response header value now, by default, points to /v1 enpoints.
  - /v0 is still compatible and requests will be redirected to /v1 endpoints. We recommend to move to /v1 enpoints as soon as possible. 

## 21.6.47.0 (2021-06-30)
- In order to align with Visma.net Payroll API we are updating the job execution status for successfully executed jobs.
  - Currently: Successful job value in response for "status": "Done"
  - Starting with June 30th: Successful job value in response will be changed to  "status": "Succeeded"
- If-Match header now supports both quoted and unquoted values allowing to pass the value as is from the ETag value. Applies to PUT and DELETE operations. 
- Description updates for API methods

## 21.5.72.0 (2021-05-25)
- Support for Draft Employee Creation: Now you can create Employees even when missing data for mandatory fields. 
  - POST /employees/withPosition - *IsDraft* flag is meant for saving an employee with missing data for mandatory fields. 
  - GET /employees - parameter *includeDrafts*. Use to include Draft Employees in request results.
- Description updates for API methods 

Refer to [OpenAPI Specification](https://employeeapi.employeecore.hrm.visma.net/swag/index.html) for more details. 