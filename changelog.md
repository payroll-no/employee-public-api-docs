# API changelog
## 21.5.72.0 (2021-05-25)
- Support for Draft Employee Creation: Now you can create Employees even when missing data for mandatory fields. 
  - POST /employees/withPosition - *IsDraft* flag is meant for saving an employee with missing data for mandatory fields. 
  - GET /employees - parameter *includeDrafts*. Use to include Draft Employees in results. Refer to isDraft property in POST /employees/withPosition endpoint for more information.
- Description updates for API methods 

Refer to [OpenAPI Specification](http://docs.employeeapi.employeecore.hrm.visma.net/) for more details. 