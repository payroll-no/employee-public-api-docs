# Visma.net Employee API

This repository is the for documentation and support of the [Visma.net Employee API](https://developer.visma.com/api/visma.net-employee-api/). The API allows you to create integrations between Visma.net Payroll and external systems.

## Resources

- [Visma for Developers](https://developer.visma.com/api/visma.net-employee-api/) (General information and sign up for access)
- [Open API documentation](https://docs.employeeapi.employeecore.hrm.visma.net)
- [Changelog](changelog.md)
- [Frequently asked questions](FAQ.md)


## Webhooks

Webhooks for Employee API is delivered through [Payroll Webhooks](https://oauth.developers.visma.com/service-registry/webhooks/payroll-no-webhooks). This webhook provides seamless integration with service to deliver real-time updates whenever an employee is created, updated, or deleted.

#### Subscribing to Employee API Webhook:

Subscribe to webhooks on Visma Developer Portal, [Payroll Webhooks](https://oauth.developers.visma.com/service-registry/webhooks/payroll-no-webhooks)
To manage subscriptions to **Payroll Webhooks** programmatically, integrate with [Webhooks Subscriber API](https://oauth.developers.visma.com/service-registry/apistore/webhooks-subscriber)

**Event Name**: EMPLOYEE_CHANGED
This Webhook fires when an employee is created/updated/deleted
**Target**: Tenant
**Mime type**: application/json

#### Payload Example
```json
{
    "tenantId": "1000000-0000-0000-0000-000000000", //Unique identifier for the tenant receiving the webhook.
    "employees": [ //An array containing employee objects with
        {
            "id": "1000000-0000-0000-0000-100000000", //The unique identifier for each employee.
            "isDeleted": false,   //A boolean indicating whether the employee has been deleted.
            "actionDate": "2025-02-26T08:19:59.1847109Z" //The timestamp of the action performed.
        }
    ]
}
```

#### Payload schema
```json
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "type": "object",
    "properties": {
        "tenantId": {
            "type": "string",
            "format": "uuid"
        },
        "employees": {
            "type": "array",
            "items": {
                "type": "object",
                "properties": {
                    "id": {
                        "type": "string",
                        "format": "uuid"
                    },
                    "isDeleted": {
                        "type": "boolean"
                    },
                    "actionDate": {
                        "type": "string",
                        "format": "date-time"
                    }
                },
                "required": [
                    "id",
                    "actionDate"
                ]
            }
        }
    },
    "required": [
        "tenantId",
        "employees"
    ]
}
```

## Feedback & Suppport

Please create a [feature request](https://github.com/visma-net/employee-public-api-docs/issues/new?assignees=&labels=&template=feature_request.md&title=) or [bug report](https://github.com/visma-net/employee-public-api-docs/issues/new?assignees=&labels=&template=bug_report.md&title=) for us to follow up on your feedback.
If you encounter any issues, please do not hesitate to contact our support team.