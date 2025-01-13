# API changelog

## New values for `employmentForm` (2025-01-13)
With the latest update to our API, we have incorporated changes to ensure compliance with new legal requirements for 2025. This update involves additions to `employmentForm` property values that our users should be aware of to maintain compatibility and compliance. New values added to `employmentForm` property
- For 'Permanent employment and hired out' use **fastAnsattUtleid** as the value.
- For 'Temporary employment and hired out' use **midlertidigAnsattUtleid** as the value.
- For 'Temporary employment as on-call coverstaff' use **midlertidigAnsattSomTilkallingsvikar** as the value.


## New filter for Employee API v2 (2025-01-07)
We are excited to announce a new feature in our API, designed to enhance the way you retrieve employee data. This update introduces a filter allowing you to fetch only those employees whose information has changed after a specified date. 
This enhancement aims to streamline your data queries, minimize response payload, and increase efficiency in data management processes.
Please check your API client to ensure it is functioning correctly after this change.

### New Parameter: `fromLastChangedDate`
- **Description:** Specifies the date after which the employee records have been modified.
- **Parameter Type:** Date
- **Format:** `YYYY-MM-DD` (e.g., `2025-01-01`)
- **Usage Example:** To retrieve employee records updated after December 25, 2024, include the following in your API request: `GET /employees?fromLastChangedDate=2024-12-25`

### Benefits
- **Efficiency:** Reduce the volume of data transferred by focusing only on recent updates.
- **Performance:** Decrease query execution times and response size, enhancing application performance.
- **Relevance:** Ensure the data you work with is current to your operational needs.

### Getting Started
To start using this new feature, simply integrate the `fromLastChangedDate` parameter into your existing API queries as needed.

### Feedback & Suppport
Your feedback is invaluable to us. Please share your thoughts and suggestions on this new feature to help us continue improving our API offerings.
If you encounter any issues, please do not hesitate to contact our support team.


## Bugfix for Employee API (v1 & v2) (2024-11-28)
We recently identified an error in our system where the shipping type values "nis" and "nos" were incorrectly described in our api documentation.
We have corrected the issue. The correct mappings are:
  - nis: Norwegian International Ship Register
  - nor: Norwegian Ship Register

Please check your API client to ensure it is functioning correctly after this change.
If you encounter any issues, please do not hesitate to contact our support team.

We apologize for any inconvenience this may have caused.


## Announcement about sunsetting Employee API v1 (2024-10-22)
We are announcing the **sunset date for Employee API v1, effective January  31st, 2025**.
We would like that all clients that are still using Employee API v1 migrate to v2 of the API. 

It has been nearly 8 months since the release of Employee API v2. New version introduces a number of new properties and provides access to more critical information, enhancing the overall functionality and user experience.
In Employee API v2 we have focused on expanding the data available to our users. New properties have been added across various endpoints, allowing for more comprehensive data retrieval and manipulation.

**There are breaking changes** when moving from Employee API v1 to Employee API v2. These changes are necessary to improve the API's performance and capabilities, changes require updates to your existing code. We recommend reviewing the detailed [release notes](https://github.com/payroll-no/employee-public-api-docs/blob/main/changelog.md#release-of-employee-api-v2-2024-02-26) carefully to understand the changes and how they might impact your application.

### **Summary of introduced changes in Employee API v2**
  - Breaking changes:
    - Update to Employee API Url
    - Introduction of Content-Type header `application/vnd.no.payroll.employee.V2.0+json` for better version control
    - Pagination - with v2, pagination is enforced by default for all tenants.
    - Schema updates: new properties supported to match data possible to manage via Employee Management UI
  - New query parameters
  - New endpoints to retrieve and work with Tax Units, Creditors, Unions, Pensions, Cars. In Employee and Position objects, identifier representation is changed to GUID so can be queried using the new endpoints

### Documentation
Updated API documentation for the new version: [Swagger documentation](https://api.employeecore.hrm.visma.net/v2/)

[Employee API v2 Release notes](https://github.com/payroll-no/employee-public-api-docs/blob/main/changelog.md#release-of-employee-api-v2-2024-02-26)

### FAQ section addressing common concerns and questions
  - Are there breaking changes when moving to v2?
    - Yes, there are breaking changes as documented in release notes. (mandatory header, schema updates, pagination by default)
  - How can I test new versions?
    - Update the client, url, make sure that pagination is supported, add the new header. Verify in the test company that the new client functions as expected. 
  - Can I rollback after making a change?
    - You can rollback safely while v1 is available (up until January 31st, 2025)
  - Are there any changes to PUT/POST routine/workflow?
    - There are no changes to workflow between the versions.
  - Where can I find out information on the different versions?
    - The change log is the best place to find information on the different versions and the changes they contain. We will highlight breaking changes in the change log clearly so you are aware of versions that may require you to make a change on your end.
  - Will Employee API v1 lose any functionality prior to January 31st, 2025
    - No, all existing functionality of API v1 will remain available till January 31st, 2025
  - What will happen to API v1 after January  31st, 2025
    - After the sunset date, API will become unavailable.

## Release of Employee API v2 (2024-02-26)
We are excited to announce the release of API Version 2. This new version introduces a number of new properties and provides access to more critical information, enhancing the overall functionality and user experience.

Employee API v2 now comes with a new Content-Type header, that will make future API updates safer and reduce possible issues while upgrading. Allowing clients to update to newer versions at client’s own pace.

In this release, we have focused on expanding the data available to our users. New properties have been added across various endpoints, allowing for more comprehensive data retrieval and manipulation. 

Please note that this version also includes breaking changes. These changes are necessary to improve the API's performance and capabilities, but they may require updates to your existing code. We recommend reviewing the detailed release notes carefully to understand the changes and how they might impact your application.

We believe that these updates will significantly improve your interaction with our API, and we are committed to continuously enhancing its capabilities based on your feedback. Thank you for your continued support and usage of our API.

**Summary**
  - Update to Employee API Url
  - Breaking changes: introduction of Content-Type to identify client supported API version
  - New query parameters
  - Schema updates: new properties supported to match data possible to manage via Employee Management UI
  - Pagination - with version 2, pagination is enforced by default for all tenants. 
  - New endpoints to retrieve and work with Tax Units, Creditors, Unions, Pensions, Cars. In Employee and Position objects, identifier representation is changed to GUID so can be queried using the new endpoints 

**Server Updates**
New default URI to be used with version 2: **https://api.employeecore.hrm.visma.net/v2/**

**API Endpoint Changes**
  - The content type for several endpoints has been updated to 'application/vnd.no.payroll.employee.V2.0+json'. This is a breaking change. Clients should be updated to parse this new content type. The new content type is used in the following endpoints:
    - Employee
      - GET /employees
      - POST /employees/withPosition
      - GET /employees/{employeeId}
      - PUT /employees/{employeeId}
      - GET /employees/jobs
      - GET /employees/jobs/{id}
    - Position
      - GET /employees/{employeeId}/positions
      - POST /employees/{employeeId}/positions
      - GET /employees/{employeeId}/positions/{positionId}
      - PUT /employees/{employeeId}/positions/{positionId}
      - GET /employees/{employeeId}/positions/jobs
      - GET /employees/{employeeId}/positions/jobs/{id}
    - Additional query parameters have been added to the GET /employees endpoint. These include:
      - page-size: parameter indicates the number of results that the client would like to see in the response.
      - onlyActive: Set this to True to return only Employees with active positions. For example, GET /employees?onlyActive=True.
      - manager: This is the EmployeeID assigned as a Manager. Only Employees who have a user created via Employee Management UI can be assigned as Manager. For example, GET /employees?manager=1234-5678-9101-1121.

**Schema Updates**
  - Employee `number` has been made optional. If Employee `number` is not provided on Employee creation, Employee number will be automatically assigned taking the next available number in the company. 
  - `askForTaxInformation` property will be set to to true by default on new employee creation, even if not specified in the POST operation. Property is available under `taxInformation` in `employee`.
  - New properties have been added to the `employee` schema:
    - `manager`: Used to set a Manager for an employee. Property can be used as a query parameter to filter employees for this Manager. 
    - `notes`: This field can be used to add any additional information about the employee.
    - `additionalAccounts`: This field can be used to add any additional accounts associated with the employee.
    - `creditorClaims`: This field can be used to add any creditor claims associated with the employee. 
    - `taxAndContributionRules`: This field can be used to add any tax and contribution rules associated with the employee.
    - `taxInformation`: Added fields to read and manage full inforamation in Tax & Claims Tab in Employee Management UI
    - `payslipInEnglish`: This field can be used to specify if the payslip should be in English.
    - `mainContact` property has been added to the 'relative' schema. This can be used to specify the main contact among the employee's relatives.
    - `setOfAccount`- values for the purpose of dividing employee costs on different accounts, depending on what value the employee have
  - The `typeOfPerson` and `manager` properties have been added to the 'position' schema. These can be used to specify the type of person in the position and the manager of the position respectively.

**Pagination**
Pagination is enforced by default for all tenants. 50 objects per page would be returned by default. Page size can be adjusted using page-size parameters. 

**New Endpoints to get Organization Settings**
New endpoints to retrieve and work with Tax Units, Work Time Agreements, Creditors, Unions, Pensions, Cars and Tags. In Employee and Position objects, identifier representation is changed to GUID so can be queried using the new endpoints 
  - CompanySettings (https://api.employeecore.hrm.visma.net/v2/#/CompanySettings)
  - TaxUnit (https://api.employeecore.hrm.visma.net/v2/#/TaxUnit)
  - Cars (https://api.employeecore.hrm.visma.net/v2/#/Cars)
  - WorkTimeAgreement (https://api.employeecore.hrm.visma.net/v2/#/WorkTimeAgreement)
  - Pension (https://api.employeecore.hrm.visma.net/v2/#/Pension)
  - Creditor (https://api.employeecore.hrm.visma.net/v2/#/Creditor). Get Creditor details used Employee creditorClaims schema
  - Union (https://api.employeecore.hrm.visma.net/v2/#/Union)
  - Tags (https://api.employeecore.hrm.visma.net/v2/#/Tags)

 **Introducing the Sunset Header in Employee API v2**
 
*What is the Sunset Header?*
The Sunset header is a standardized HTTP header that informs clients about the deprecation timeline of an API endpoint. With the Sunset header, clients can stay informed about upcoming changes and plan their migration strategies accordingly.

*What Should Clients Do?*
While the Sunset header is now available in Employee API v2, clients are encouraged to review their integration and ensure they're prepared to handle Sunset header changes when they occur.

## 24.2.6.0 (2024-02-22). 
With this update we are preparing for more streamlined management of certain references used in Employee and Position objects. 
In Employee and Position objects, identifier representation for Tax Units, Work Time Agreements, Creditors, Unions, Pensions, Cars, Unions is changed to GUID (type is still string).
Previously used values are backwards compatible, so you can continue using values that have been used before. 
New GUID represented values can be looked up in extended API published on https://api.employeecore.hrm.visma.net/

## 23.6.27.0 (2023-07-06). API Specification version update to v1.1.3
In this release, we are excited to introduce a new endpoint to the Employee API, which allows users to access pension-related information in read-only mode. This endpoint provides valuable functionality for retrieving pension data, ensuring clients have access to up-to-date pension details.

- **Pensions**: Gets all pensions defined in a company
    - Endpoint URL: `/pensions`
    - HTTP Method: `GET`
    - Returns all pensions defined in a company.

- **Individual Pension**: Gets individual pension defined in a company
    - Endpoint URL: `/pensions/{id}`
    - HTTP Method: `GET`
    - Returns details for individual pension defined in a company.

### Upgrade Guide
To start utilising the new functionality, please update your API integration to use the latest URL (**https://api.employeecore.hrm.visma.net/**) and incorporate the new endpoints as outlined above. 

## 23.6.16.0 (2023-06-20). API Specification version update to v1.1.3
We are excited to announce the release of the latest updates to Employee API. This update brings several enhancements and introduces a new URL with new endpoints, designed to provide you with critical information to efficiently work with entities managed through our API. Here are the key features and improvements:

### New URL and Endpoints
We have introduced a new URL that serves as the entry point for accessing the enhanced functionality: **https://api.employeecore.hrm.visma.net/**. This new URL provides access to a set of endpoints to better support work with Employees and Positions

- **Tax Units**: Gets all tax units defined in a company
    - Endpoint URL: `/taxUnits`
    - HTTP Method: `GET`
    - Returns all tax units defined in a company.

- **Tax Unit**: Get tax unit defined in company by tax unit identifier.
    - Endpoint URL: `/taxUnits/{id}`
    - HTTP Method: `GET`
    - Returns tax unit details in a company.

- **Work Time Agreements**: Gets all work time agreements available for company.
    - Endpoint URL: `/workTimeAgreements`
    - HTTP Method: `GET`
    - Returns all work time agreements available for company.

- **Work Time Agreements**: Get tax unit defined in company by work time agreement identifier.
    - Endpoint URL: `/workTimeAgreements/{id}`
    - HTTP Method: `GET`
    - Returns work time agreement details in a company.

- **Tags**: Get all tags defined for a company.
    - Endpoint URL: `/tags`
    - HTTP Method: `GET`
    - Returns all tags defined in a  company.

- **Tags**: Get tag defined in company by tag identifier.
    - Endpoint URL: `/tags/{id}`
    - HTTP Method: `GET`
    - Returns tag details defined in a company.

### Upgrade Guide
To start utilising the new functionality, please update your API integration to use the latest URL (**https://api.employeecore.hrm.visma.net/**) and incorporate the new endpoints as outlined above. 

If you have any questions or need assistance during the transition, please reach out to our support team. We are here to help you make the most of the new features and ensure a smooth transition.

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