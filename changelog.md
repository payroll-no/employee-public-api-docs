# API changelog
## 22.1.14.0 (2022-01-25)
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