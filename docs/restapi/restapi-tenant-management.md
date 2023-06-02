---
title: Project Amber Tenant Management REST API
description: External documentation for the Tenant Management API.
author: grminch
topic-type: NA
date: 03/16/2023
---

# Project Amber Tenant Management

This set of APIs allows you to manage tags, tenants, and users. 

## Tags

### GET /management/v1/tags

Returns all the user defined and predefined tags for the logged in tenant.

#### Parameters

| Parameters                     |                                       |
|--------------------------------|---------------------------------------|
| Accept  (required)             | Accept header                         |
|                                | Available values :   application/json |
|                                | string (header)                       |

#### Responses

Response content type: application/json

| Code | Description                                                          |
|------|----------------------------------------------------------------------|
| 200  |         Successfully retrieved the list of tags. |
| 401  |         Unauthorized.                                                 |
| 403  |         RBAC access denied.                                          |
| 404  |         Resource not found.                                          |
| 415  |         Invalid Accept header.                             |
| 500  |         Internal server error.                                       |

Example Response Value:

```json
[
  {
    "tags": [
        {
            "id": "59a7fc44-4747-4b11-add8-da5f9a9cad60",
            "name": "Workload",
            "predefined": true
        },
        {
            "id": "1d668ea7-2e0e-4b4a-8ac7-3fbed6146133",
            "name": "Power",
            "predefined": false
        }
    ]
  }
]
```

#### Extensions

| Field    | Value    |
|----------|----------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/tags"  |




### POST /management/v1/tags

Creates a tag that allows tenants to attach additional information to identify their API keys.

#### Parameters

| Parameters                     |                                       |
|--------------------------------|---------------------------------------|
| Content-Type  (required)       | Content-Type header                   |
|                                | Available values :   application/json |
|                                | string (header)                       |
| Accept  (required)             | Accept header                         |
|                                | Available values :   application/json |
|                                | string (header)                       |
| request body (required)        | application/json |

Request body example:

```json
{
    "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "name": "Frequency"
}
```

#### Responses

Response content type: application/json

| Code | Description                                                          |
|------|----------------------------------------------------------------------|
| 200  |         Successfully retrieved the list of tags. |
| 401  |         Unauthorized.                                                 |
| 403  |         RBAC access denied.                                          |
| 404  |         Resource not found.                                          |
| 415  |         Invalid Accept header.                             |
| 500  |         Internal server error.                                       |

Example Response Value:

```json
{
    "id": "a9f765e4-0296-4147-be03-1c9deb7c050f",
    "name": "Frequency",
    "predefined": false
}
```

#### Extensions

| Field    | Value    |
|----------|----------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/tags"  |

### DELETE /management/v1/tags/{tagId}

Deletes the tag for a specified tagId.

#### Parameters

| Name                   | Description                                   |
|------------------------|-----------------------------------------------|
| tagID (required)       | Universal unique identifier assigned to the tag.|
|                        | string($uuid) (path)                          |

#### Responses

Response content type: application/json

| Code | Description                         |
|------|-------------------------------------|
| 204  | Successfully deleted the tag from the tenant.|
| 401  | Unauthorized.                     |
| 403  | RBAC access denied.                 |
| 404  | Resource Not Found.                 |
| 500  | Internal server error.              |


### GET /management/v1/tags/{tagID}

Retrieves tag details for the specified tagId.

#### Parameters

| Parameters                     |                                                        |
|--------------------------------|--------------------------------------------------------|
| Accept  (required)             | Accept header                                          |
|                                | Available values :   application/json                  |
|                                | string (header)                                        |
| tagId  (required)              | Universal Unique IDentifier assigned to the Tag.       |
|                                | string($uuid) (path)                                   |


#### Responses

Response content type: application/json

| Code | Description                                                          |
|------|----------------------------------------------------------------------|
| 200  |         Successfully retrieved the tag details. |
| 401  |         Unauthorized.                                                 |
| 403  |         RBAC access denied.                                          |
| 404  |         Resource not found.                                          |
| 415  |         Invalid Accept header.                             |
| 500  |         Internal server error.                                       |

Example Response Value:

```json

{
  "tags": [
    {
      "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
      "name": "Voltage",
      "predefined": false
    }
  ]
}

```
## Tenants

### GET /management/v1/tenants

Retrieves tenant details of the logged in tenant.

#### Parameters

| Parameters                     |                                                        |
|--------------------------------|--------------------------------------------------------|
| Accept  (required)             | Accept header                                          |
|                                | Available values :   application/json                  |
|                                | string (header)                                        |



#### Responses

Response content type: application/json

| Code | Description                                                          |
|------|----------------------------------------------------------------------|
| 200  |         Successfully retrieved the tenants. |
| 401  |         Unauthorized.                                                 |
| 403  |         RBAC access denied.                                          |
| 404  |         Resource not found.                                          |
| 415  |         Invalid Accept header in the request.                             |
| 500  |         Internal server error.                                       |

Example Response Value:

```json

{

  "address": "1st Street, NY",
  "company": "ABC Inc",
  "email": "jane.doe@domain.com",
  "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "name": "jane.doe@domain.com"

}

```

## Users

### GET /management/v1/users

Returns all the users registered under the logged in tenant.

#### Parameters

| Parameters                     |                                                        |
|--------------------------------|--------------------------------------------------------|
| Accept  (required)             | Accept header                                          |
|                                | Available values :   application/json                  |
|                                | string (header)                                        |



#### Responses

Response content type: application/json

| Code | Description                                                          |
|------|----------------------------------------------------------------------|
| 200  |         Successfully retrieved the list of users. |
| 401  |         Unauthorized.                                                 |
| 403  |         RBAC access denied.                                          |
| 404  |         Resource not found.                                          |
| 415  |         Invalid Accept header in the request.                             |
| 500  |         Internal server error.                                       |

Example Response Value:

```json

[
  {
    "active": true,
    "created_at": "2025-09-21T01:12:59.137Z",
    "email": "jane.doe@domain.com",
    "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "privacy_acknowledgement": true,
    "role": {
      "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
      "name": "Tenant Admin",
      "permissions": {
        "policy": {
          "grants": [
            "remove",
            "read",
            "modify",
            "write"
          ]
        }
      },
      "scope": "tenant reports"
    }
  }
]

```
### POST /management/v1/users

Adds a new user with the specified role to an existing tenant.

#### Parameters

| Name                    | Description                                          |
|-------------------------|------------------------------------------------------|
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
  "email": "jane.doe@domain.com",
  "role": "Tenant Admin"
}
```

#### Responses

Response content type: application/json

| Code | Description                                    |
|------|------------------------------------------------|
| 200  | Successfully created the tenant user.          |
| 400  | Invalid request body provided.                 |
| 401  | Unauthorized.                                  |
| 403  | RBAC access denied.                            |
| 404  | Resource Not Found.                            |
| 415  | Invalid Content-Type/Accept header in the request. |
| 500  | Internal server error.                         |


Example response value:

```json
{
  "active": true,
  "created_at": "2025-09-21T01:12:59.137Z",
  "email": "jane.doe@domain.com",
  "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "privacy_acknowledgement": true,
  "role": {
    "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "name": "Tenant Admin",
    "permissions": {
      "policy": {
        "grants": [
          "remove",
          "read",
          "modify",
          "write"
        ]
      }
    },
    "scope": "tenant reports"
  }
}
```
### DELETE /management/v1/users/{userId}

Deletes the user from the logged in tenant for specified userId.

#### Parameters

| Name                   | Description                                   |
|------------------------|-----------------------------------------------|
| userID (required)      | Universal Unique IDentifier of the user.      |
|                        | string($uuid) (path)                          |


#### Responses

Response content type: application/json

| Code | Description                         |
|------|-------------------------------------|
| 204  | Successfully deleted the user from the tenant. |
| 401  | Unauthorized.                       |
| 403  | RBAC access denied.                 |
| 404  | Resource not found.                 |
| 500  | Internal server error.              |

### PUT /management/v1/users/{userId}

Updates the role (user or tenant admin) of the specified userId.

#### Parameters

| Name                    | Description                                          |
|-------------------------|------------------------------------------------------|
| Content-Type (required) | Content-Type header                                  |
|                         | Available values : application/json                  |
|                         | string (header)                                      |
| Accept                  | Accept header                                        |
|                         | Available values : application/json                  |
|                         | string (header)                                      |
| userID                  | Universal Unique IDentifier assigned to the user.    |
|                         | string($uuid) (path)                                 |

##### Request Body

Parameter content type: application/json

```json
{
  "role": "Tenant Admin"
}
```

#### Responses

Response content type: application/json

| Code | Description                                    |
|------|------------------------------------------------|
| 200  | Successfully updated the tenant user role.     |
| 400  | Invalid request body provided.                 |
| 401  | Unauthorized.                                  |
| 403  | RBAC access denied.                            |
| 404  | Resource Not Found.                            |
| 415  | Invalid Content-Type/Accept header in the request. |
| 500  | Internal server error.                         |


Example response value:

```json
{
  "active": true,
  "created_at": "2025-09-21T01:12:59.137Z",
  "email": "jane.doe@domain.com",
  "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "privacy_acknowledgement": true,
  "role": {
    "id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "name": "Tenant Admin",
    "permissions": {
      "policy": {
        "grants": [
          "remove",
          "read",
          "modify",
          "write"
        ]
      }
    },
    "scope": "tenant reports"
  }
}
```

