---
title: Project Amber Client Management Swagger doc
description: Client Management external API definition
author: Paul Cartee
topic-type: NA
date: 03/16/2023
---

# Client Management

These set of management APIs supports CRUD operations on tenants, tenant users, and API client keys.
Tenant users can subscribe to different service offers like SGX/TDX Attestation under a Basic or Premium plan.
Once subscribed to a service offer, users can generate the corresponding APIClient key to use attestation services.
Tenant users are by default enrolled to the Premium plan and have flexibility to switch between plans.

## Services for API clients

### GET /management/v1/services/{serviceId}/api-clients

Returns the list of API Clients associated with the specified serviceId for the logged in Tenant.

#### Parameters

| Parameters                     |                                                        |
|--------------------------------|--------------------------------------------------------|
| Accept  (required)             | Accept header                                          |
|                                | Available values :   application/json                  |
|                                | string (header)                                        |
| serviceId  (required)          | Universal Unique IDentifier of the service             |
|                                | string($uuid) (path)                                   |
| productType                    | type of api key to search (attestation/management)     |
|                                | Available values : attestation, management             |
|                                |string (query)                                          |


#### Responses

Response content type: application/json

| Code | Description                                                |
|------|------------------------------------------------------------|
| 200  | Successfully retrieved the ApiClients that match criteria. |
| 401  | Unauthorized                                               |
| 403  | RBAC access denied                                         |
| 404  | Resource Not Found.                                        |
| 415  | Invalid Accept Header in Request                           |
| 500  | Internal server error.                                     |

Example Response Value:

```json
[
  {
    "created_at": "2022-10-28T13:02:14.682366+05:30",
    "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "name": "TDX Attestation Premium",
    "product_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "product_name": "Premium",
    "product_type": "string",
    "service_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "status": "string"
  }
]
```

#### Extensions

| Field                                  | Value                                                                                                                                                                                                                                                                                                                        |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  x-sample-call-endpoint | "https://amber.com:443/management/v1/services/c477f41c-9b38-49ef-b272-810eecd6256d/api-clients"                                                                                                                                                                                                                                         |
| x-sample-call-output    | "[\n {\n \"id\": \"8a41171b-9723-4cf8-afb6-1402fa137913\",\n \"service_id\": \"eb7c16c2-a985-4e99-83c9-9fe3a730252f\",\n \"product_id\": \"412bb147-5634-4613-96ad-3a134810bb68\",\n \"product_name\": \"Basic\",\n \"status\": \"Active\",\n \"name\": \"SGX Basic\",\n \"created_at\": \"2022-08-26T10:12:19.162087+05:30\"\n }\n]\n" |


### POST /management/v1/services/{serviceId}/api-clients

Creates a new API Client Key. It associates list of policies and tags if provided.

#### Parameters

| Name                    | Description                                          |
|-------------------------|------------------------------------------------------|
|  serviceID (required)   | Universal Unique IDentifier assigned to the service. |
|                         | string($uuid) (path)                                 |
| Content-Type (required) | Content-Type header                                  |
|                         | Available values : application/json                  |
|                         | string (header)                                      |
| Accept                  | Accept header                                        |
|                         | Available values : application/json                  |
|                         | string (header)                                      |

##### Request Body

Parameter content type: application/json

```json
{
  "name": "TDX Attestation Basic",
  "policy_ids": [
    "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "4cdf4288-d4e7-445c-9c0c-581cffb53e00"
  ],
  "product_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "status": "string",
  "tags": [
    {
      "key": "Workload",
      "value": "AI Workload"
    },
    {
      "key": "Workload",
      "value": "EXE Workload"
    }
  ]
}
```

#### Responses

Response content type: application/json

| Code | Description                                    |
|------|------------------------------------------------|
| 201  | Successfully created the ApiClient.            |
| 400  | Invalid request body provided.                 |
| 401  | Unauthorized.                                  |
| 403  | RBAC access denied.                            |
| 404  | Resource Not Found.                            |
| 415  | Invalid Content-Type/Accept Header in Request. |
| 500  | Internal server error.                         |


Example response value:

```json
{
  "created_at": "2022-10-28T13:02:14.682366+05:30",
  "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "keys": [
    "85kyf1720ddd4bb893d3f6092e5b4c7d",
    "12kyf1720ddd4bb893d3f6092e5b4c7d"
  ],
  "name": "TDX Attestation Basic",
  "policy_ids": [
    "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "4cdf4288-d4e7-445c-9c0c-581cffb53e00"
  ],
  "product_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "product_name": "Premium",
  "product_type": "string",
  "service_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "service_offer_name": "TDX Attestation",
  "status": "string",
  "tags": [
    {
      "key": "Workload",
      "value": "AI Workload"
    },
    {
      "key": "Workload",
      "value": "EXE Workload"
    }
  ]
}
```

#### Extensions

| Field                  | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/services/a68a2570-ba57-41fd-9663-c25d3e861666/api-clients"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| x-sample-call-input    | "{\n \"product_id\": \"a68a2570-ba57-41fd-9663-c25d3e861666\",\n \"name\": \"SGX Basic\",\n \"policy_ids\":[\"1cf3db7d-81ea-4904-babf-fcb3501492db\", \"2cf3db7d-81ea-4904-babf-fcb3501492db\", \"3cf3db7d-81ea-4904-babf-fcb3501492db\", \"4cf3db7d-81ea-4904-babf-fcb3501492db\"],\n \"tags\": [\n {\n \"key\": \"Workload\",\n \"value\": \"Workload-Binary\"\n }\n ]\n}\n"                                                                                                                                                                                                                                                                                                                                                                                            |
| x-sample-call-output   | "{\n \"id\": \"8a41171b-9723-4cf8-afb6-1402fa137913\",\n \"service_id\": \"eb7c16c2-a985-4e99-83c9-9fe3a730252f\",\n \"service_offer_name\": \"\",\n \"product_id\": \"412bb147-5634-4613-96ad-3a134810bb68\",\n \"product_name\": \"\",\n \"status\": \"Active\",\n \"name\": \"SGX Basic\",\n \"keys\": [\n \"a7ed25ff371b470bb79ce81b3a087186\",\n \"ccaa4f7239de406092639f9a6b9f11ad\"\n ],\n \"policy_ids\": \": [\n \"1cf3db7d-81ea-4904-babf-fcb3501492db\",\n \"2cf3db7d-81ea-4904-babf-fcb3501492db\",\n \"3cf3db7d-81ea-4904-babf-fcb3501492db\",\n \"4cf3db7d-81ea-4904-babf-fcb3501492db\"\n ],\n \"tags\": [\n {\n \"key\": \"Workload\",\n \"value\": \"Workload-Binary\",\n \"predefined\": true\n }\n ],\n \"created_at\": \"0001-01-01T00:00:00Z\"\n}\n" |

### DELETE /management/v1/services/{serviceId}/api-clients/{apiClientId}

Deletes the API Client for the specified serviceId and apiClientId for the logged in Tenant.

#### Parameters

| Name                   | Description                                   |
|------------------------|-----------------------------------------------|
| serviceID (required)   | Universal Unique IDentifier of the service.   |
|                        | string($uuid) (path)                          |
| apiClientId (required) | Universal Unique IDentifier of the ApiClient. |
|                        | string($uuid) (path)                          |

#### Responses

Response content type: application/json

| Code | Description                         |
|------|-------------------------------------|
| 204  | Successfully deleted the ApiClient. |
| 401  | Unauthorized.                       |
| 403  | RBAC access denied.                 |
| 404  | Resource Not Found.                 |
| 500  | Internal server error.              |

#### Extensions

| Field                  | Value                                                                                                                                |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| x-smaple-call-endpoint | "https://amber.com:443/management/v1/services/b02b67cc-02a9-4fc0-a558-889a5f00cd47/api-clients/b02b67cc-02a9-4fc0-a558-889a5f00cd47" |

### GET /management/v1/services/{serviceId}/api-clients/{apiClientId}

Retrieves the ApiClient for the specified serviceId and apiClientId for the logged in Tenant.

#### Parameters

| Name                   | Description                                   |
|------------------------|-----------------------------------------------|
| Accept (required)      | Accept header                                 |
|                        | Available values : application/json           |
|                        | string (header)                               |
| serviceId (required)   | Universal Unique IDentifier of the service.   |
|                        | string($uuid) (path)                          |
| apiClientId (required) | Universal Unique IDentifier of the ApiClient. |
|                        | string($uuid) (path)                          |

#### Responses

Response content type: application/json

| Code | Description                           |
|------|---------------------------------------|
| 200  | Successfully retrieved the ApiClient. |
| 401  | Unauthorized.                         |
| 403  | RBAC access denied.                   |
| 404  | Resource Not Found.                   |
| 415  | Invalid Accept Header in Request.     |
| 500  | Internal server error.                |


Example response value:

```json
{
  "created_at": "2022-10-28T13:02:14.682366+05:30",
  "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "keys": [
    "85kyf1720ddd4bb893d3f6092e5b4c7d",
    "12kyf1720ddd4bb893d3f6092e5b4c7d"
  ],
  "name": "TDX Attestation Basic",
  "policy_ids": [
    "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "4cdf4288-d4e7-445c-9c0c-581cffb53e00"
  ],
  "product_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "product_name": "Premium",
  "product_type": "string",
  "service_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "service_offer_name": "TDX Attestation",
  "status": "string",
  "tags": [
    {
      "key": "Workload",
      "value": "AI Workload"
    },
    {
      "key": "Workload",
      "value": "EXE Workload"
    }
  ]
}
```

#### Extensions

| Field                  | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/services/c477f41c-9b38-49ef-b272-810eecd6256d/api-clients/636fe3df-d718-4f1e-8445-61a1bd72912f"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| x-sample-call-output   | "{\n \"id\": \"8a41171b-9723-4cf8-afb6-1402fa137913\",\n \"service_id\": \"eb7c16c2-a985-4e99-83c9-9fe3a730252f\",\n \"service_offer_name\": \"TEE Attestation\",\n \"product_id\": \"412bb147-5634-4613-96ad-3a134810bb68\",\n \"product_name\": \"Basic\",\n \"status\": \"Active\",\n \"name\": \"SGX Basic\",\n \"keys\": [\n \"a7ed25ff371b470bb79ce81b3a087186\",\n \"ccaa4f7239de406092639f9a6b9f11ad\"\n ],\n \"policy_ids\": [\n \"1cf3db7d-81ea-4904-babf-fcb3501492db\",\n \"2cf3db7d-81ea-4904-babf-fcb3501492db\",\n \"3cf3db7d-81ea-4904-babf-fcb3501492db\",\n \"4cf3db7d-81ea-4904-babf-fcb3501492db\"\n ],\n \"tags\": [\n {\n \"key\": \"Workload\",\n \"value\": \"AI\",\n \"predefined\": true\n }\n ],\n \"created_at\": \"2022-08-26T10:12:19.162087+05:30\"\n}\n" |

### PUT /management/v1/services/{serviceId}/api-clients/{apiClientId}

Updates the API Client details for the specified apiClientId for the logged in Tenant. Policy and tag collections will be updated in their entirety.

#### Parameters

| Name                    | Description                                   |
|-------------------------|-----------------------------------------------|
| Content-Type (required) | Content-Type header                           |
|                         | Available values : application/json           |
|                         | string (header)                               |
| Accept (required)       | Accept header                                 |
|                         | Available values : application/json           |
|                         | string (header)                               |
| serviceId (required)    | Universal Unique IDentifier of the service.   |
|                         | string($uuid) (path)                          |
| apiClientId (requried)  | Universal Unique IDentifier of the ApiClient. |
|                         | string($uuid) (path)                          |

##### Request Body

Parameter content type: application/json

```json
{
  "name": "TDX Attestation Basic",
  "policy_ids": [
    "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "4cdf4288-d4e7-445c-9c0c-581cffb53e00"
  ],
  "product_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "status": "string",
  "tags": [
    {
      "key": "Workload",
      "value": "AI Workload"
    },
    {
      "key": "Workload",
      "value": "EXE Workload"
    }
  ]
}
```

#### Responses

Response content type: application/json

| Code | Description                                    |
|------|------------------------------------------------|
| 200  | Successfully updated the ApiClient.            |
| 400  | Invalid request body provided.                 |
| 401  | Unauthorized.                                  |
| 403  | RBAC access denied.                            |
| 404  | Resource Not Found.                            |
| 415  | Invalid Content-Type/Accept Header in Request. |
| 500  | Internal server error.                         |

Example response value:

```json
{
  "created_at": "2022-10-28T13:02:14.682366+05:30",
  "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "name": "TDX Attestation Premium",
  "product_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "product_name": "Premium",
  "product_type": "string",
  "service_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "status": "string"
}
```
#### Extensions

| Field                  | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/services/b1378536-f7ba-4bef-b9a4-9e2ff2c6bbde/api-clients/3a7fd7ba-3dad-4740-8faf-99bc2b192ab5"                                                                                                                                                                                                                                                                                                                                                   |
| x-sample-call-input    | "{\n \"product_id\": \"35e0d556-bb0c-4fad-a0a1-65d37e870a94\",\n \"status\": \"Active\",\n \"name\": \"APPLE_SGX_Basic_SUB_2\",\n \"policy_ids\": [\n \"1cf3db7d-81ea-4904-babf-fcb3501492db\",\n \"2cf3db7d-81ea-4904-babf-fcb3501492db\",\n \"5cf3db7d-81ea-4904-babf-fcb3501492db\",\n \"6cf3db7d-81ea-4904-babf-fcb3501492db\"\n ],\n \"tags\": [\n {\n \"key\": \"Workload\",\n \"value\": \"EXE Update\"\n },\n {\n \"key\": \"Power\",\n \"value\": \"AI Update\"\n }\n ]\n}\n" |
| x-sample-call-output   | "{\n \"id\": \"3a7fd7ba-3dad-4740-8faf-99bc2b192ab5\",\n \"service_id\": \"2e302c60-e999-406b-8cb0-8fc18b136af2\",\n \"product_id\": \"35e0d556-bb0c-4fad-a0a1-65d37e870a94\",\n \"product_name\": \"\",\n \"status\": \"Active\",\n \"name\": \"APPLE_SGX_Basic_SUB_2\",\n \"created_at\": \"2022-10-21T10:17:12.654339+05:30\"\n}\n"                                                                                                                                                 |

### GET /management/v1/services/{serviceId}/api-clients/{apiClientId}/policies

Returns the list of policies associated with the specified serviceId and apiClientId for the logged in Tenant.

#### Parameters

| Name                 | Description                                   |
|----------------------|-----------------------------------------------|
| Accept (required)    | Accept header                                 |
|                      | Available values : application/json           |
|                      | string (header)                               |
| servcieId (required) | Universal Unique IDentifier of the service.   |
|                      | string($uuid) (path)                          |
| apiClientId          | Universal Unique IDentifier of the ApiClient. |
|                      | string($uuid) (path)                          |

#### Responses

Response content type: application/json

| Code | Description                                                   |
|------|---------------------------------------------------------------|
| 200  | Successfully retrieved the policies associated with ApiClient |
| 401  | Unauthorized.                                                 |
| 403  | RBAC access denied.                                           |
| 404  | Resource Not Found.                                           |
| 415  | Invalid Accept Header in Request.                             |
| 500  | Internal server error.                                        |

Example response value:

```json
{
  "policy_ids": [
    "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "4cdf4288-d4e7-445c-9c0c-581cffb53e00"
  ]
}
```
#### Extensions

| Field                  | Value                                                                                                                                         |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/services/c477f41c-9b38-49ef-b272-810eecd6256d/api-clients/636fe3df-d718-4f1e-8445-61a1bd72912f/policies" |
| x-sample-call-output   | "{\n policy_ids: [ \"c855d8d6-744f-48c6-a06d-a97ef1811a61\",\"5d389ba0-f683-4d42-ad52-c76a94ccabc7\" ]\n}\n"                                  |

### GET /management/v1/services/{serviceId}/api-clients/{apiClientId}/tags

Returns a list of tags associated with the specified serviceId and apiClientId for the logged in Tenant.

#### Parameters

| Name                   | Description                                                   |
|------------------------|---------------------------------------------------------------|
| Accept (required)      | Accept header                                                 |
|                        | Available values : application/json                           |
|                        | string (header)                                               |
| serviceId (required)   | Universal Unique IDentifier of the service.                   |
|                        | string($uuid) (path)                                          |
| apiClientId (required) | Successfully retrieved the policies associated with ApiClient |
|                        | string($uuid) (path)                                          |

#### Responses

Response content type: application/json

| Code | Description.                                |
|------|---------------------------------------------|
| 200  | Successfully retrieved the tags and values. |
| 401  | Unauthorized.                               |
| 403  | RBAC access denied.                         |
| 404  | Resource Not Found.                         |
| 415  | Invalid Accept Header in Request            |
| 500  | Internal server error.                      |

Example response value:

```json
{
  "tags": [
    {
      "key": "Workload",
      "value": "AI Workload"
    },
    {
      "key": "Workload",
      "value": "EXE Workload"
    }
  ]
}
```

#### Extensions

| Field                  | Value                                                                                                                                                                                             |
|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/services/b1378536-f7ba-4bef-b9a4-9e2ff2c6bbde/api-clients/3a7fd7ba-3dad-4740-8faf-99bc2b192ab5/tags"                                                         |
| x-sample-call-output   | "{\n \"tags\": [\n {\n \"name\": \"Workload\",\n \"value\": \"AI\",\n \"predefined\": true\n },\n {\n \"name\": \"Workload\",\n \"value\": \"EXE Workload\",\n \"predefined\": true\n }\n ]\n}\n" |