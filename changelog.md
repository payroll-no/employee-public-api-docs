# API changelog
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

Refer to [OpenAPI Specification](http://docs.employeeapi.employeecore.hrm.visma.net/) for more details. 