---
title: Project Amber Service Management Swagger doc
description: Servcie Management external API definition
author: Paul Cartee
topic-type: NA
date: 03/16/2023
---

# Service Management

These set of management APIs supports CRUD operations on tenants, tenant users, and API client keys.
Tenant users can subscribe to different service offers like SGX/TDX Attestation under a Basic or Premium plan.
Once subscribed to a service offer, users can generate the corresponding APIClient key to use attestation services.
Tenant users are by default enrolled to Premium plan and have flexibility to switch between plans.

## Services

### GET /management/v1/services

Returns all the subscribed services for the logged in Tenant.

#### Parameters

| Parameters                     |                                                        |
|--------------------------------|--------------------------------------------------------|
| Accept  (required)             | Accept header                                          |
|                                | Available values :   application/json                  |
|                                | string (header)                                        |

#### Responses

| Code | Description                                              |
|------|----------------------------------------------------------|
| 200  | Successfully retrieved the services that match criteria. |
| 401  | Unauthorized.                                            |
| 403  | RBAC access denied.                                      |
| 404  | Resource Not Found.                                      |
| 415  | Invalid Content-Type/Accept Header in Request.           |
| 500  | Internal server error.                                   |

Example Response Value:

```json
  {
    "active": true,
    "created_at": "2022-10-28T13:02:14.682366+05:30",
    "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "name": "Service Offer Name + Tenant (TDX Attestation GE Health)",
    "plan_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "plan_name": "Premium",
    "service_offer_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "tenant_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8"
  }
]
```

#### Extensions

| Field                  | Value                                                                                                                                                                                                                                                                                                                                                                                                  |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/services"                                                                                                                                                                                                                                                                                                                                                         |
| x-sample-call-output   | "[\n {\n \"id\": \"5d389ba0-f683-4d42-ad52-c76a94ccabc7\",\n \"tenant_id\": \"c10d9d20-19b6-4a28-a01d-2a806e8ca36d\",\n \"service_offer_id\": \"c477f41c-9b38-49ef-b272-810eecd6256d\",\n \"name\": \"name of the service\",\n \"plan_id\": \"eeed665a-af5e-4966-8556-244272e4bc31\",\n \"plan_name\": \"Basic\",\n \"created_at\": \"2022-08-25T08:59:42.440576+05:30\",\n \"active\": true\n }\n]\n" |

### POST /management/v1/services

Creates a Service by subscribing to the Service Offer (SGX/TDX Attestation) for the logged in Tenant.

#### Parameters

| Name                    | Description                         |
|-------------------------|-------------------------------------|
| Content-Type (required) | Content-Type header                 |
|                         | Available values : application/json |
|                         | string (header)                     |
| Accept (required)       | Accept header                       |
|                         | Available values : application/json |
|                         | string (header)                     |

##### Request Body

Parameter content type: application/json

```json
{
  "name": "Service Offer Name + Tenant (TDX Attestation GE Health)",
  "plan_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "service_offer_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "source": "Azure OR Amber"
}
```

#### Responses

Response content type: application/json

| Code | Description                                    |
|------|------------------------------------------------|
| 201  | Successfully created the service.              |
| 400  | Invalid request body provided.                 |
| 401  | Unauthorized.                                  |
| 403  | RBAC access denied.                            |
| 404  | Resource Not Found.                            |
| 415  | Invalid Content-Type/Accept Header in Request. |
| 500  | Internal server error.                         |

Example response value:

```json
{
  "active": true,
  "created_at": "2022-10-28T13:02:14.682366+05:30",
  "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "name": "Service Offer Name + Tenant (TDX Attestation GE Health)",
  "plan_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "plan_name": "Premium",
  "service_offer_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "tenant_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8"
}
```

#### Extensions

| Field                  | Value                                                                                                                                                                                                                                                                                                                                                                          |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/services"                                                                                                                                                                                                                                                                                                                                 |
| x-sample-call-input    | "{\n \"service_offer_id\": \"c477f41c-9b38-49ef-b272-810eecd6256d\",\n \"name\" : \"name of service\",\n \"plan_id\" : \"259a289e-48bd-4caa-8470-a2e1839675c6\",\n \"source\" : \"Amber\"\n}\n"                                                                                                                                                                                |
| x-sample-call-output   | "{\n \"id\": \"5d389ba0-f683-4d42-ad52-c76a94ccabc7\",\n \"tenant_id\": \"c10d9d20-19b6-4a28-a01d-2a806e8ca36d\",\n \"service_offer_id\": \"c477f41c-9b38-49ef-b272-810eecd6256d\",\n \"name\": \"name of service\",\n \"plan_id\": \"eeed665a-af5e-4966-8556-244272e4bc31\",\n \"plan_name\": \"Basic\",\n \"created_at\": \"0001-01-01T00:00:00Z\",\n \"active\": true\n}\n" |

### DELETE /management/v1/services/{serviceId}

Deletes subscribed service with the specified serviceId for the logged in tenant.

| Name                 | Description                                 |
|----------------------|---------------------------------------------|
| serviceID (required) | Universal Unique IDentifier of the service. |
|                      | string($uuid) (path)                        |

#### Responses

Response content type: application/json

| Code | Description                       |
|------|-----------------------------------|
| 204  | Successfully deleted the service. |
| 401  | Unauthorized.                     |
| 403  | RBAC access denied.               |
| 404  | Resource Not Found.               |
| 500  | Internal server error.            |

#### Extensions

| Field                  | Value                                                                               |
|------------------------|-------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/services/b02b67cc-02a9-4fc0-a558-889a5f00cd47" |

### GET /management/v1/services/{serviceId}

Retrieves subscribed service details for the specified serviceId for logged in Tenant.

#### Parameters

| Name | Description                         |
|------|-------------------------------------|
| 200  | Successfully retrieved the service. |
| 401  | Unauthorized.                       |
| 403  | RBAC access denied.                 |
| 404  | Resource Not Found.                 |
| 415  | Invalid Accept Header in Request.   |
| 500  | Internal server error.              |

Example response value:

```json
{
  "active": true,
  "created_at": "2022-10-28T13:02:14.682366+05:30",
  "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "name": "Service Offer Name + Tenant (TDX Attestation GE Health)",
  "plan_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "plan_name": "Premium",
  "service_offer_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "tenant_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8"
}
```

##### Extensions

| Field                  | Value                                                                                                                                                                                                                                                                                                                                                                                        |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/services/5d389ba0-f683-4d42-ad52-c76a94ccabc7"                                                                                                                                                                                                                                                                                                          |
| x-sample-call-output   | "{\n \"id\": \"5d389ba0-f683-4d42-ad52-c76a94ccabc7\",\n \"service_offer_id\": \"c477f41c-9b38-49ef-b272-810eecd6256d\",\n \"service_offer_name\": \"TEE Attestation\",\n \"name\": \"description of my third service\",\n \"created_at\": \"2022-08-25T08:59:42.440576+05:30\"\n \"active\": true\n \"plan_id\": \"eeed665a-af5e-4966-8556-244272e4bc31\",\n \"plan_name\": \"Basic\"\n}\n" |