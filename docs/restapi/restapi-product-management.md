---
title: Project Amber Management API
description: External documentation for the Project Amber Management REST API.
author: grminch
topic-type: NA
date: 03/16/2023
---

# Management

This set of management APIs supports CRUD operations on tenants, tenant users, and API client keys. Tenant users can subscribe to different service offers like SGX/TDX Attestation under a Basic or Premium plan. Once subscribed to a service offer and plan, users can generate an API key to use attestation services. Tenant users are by default enrolled in the Premium plan and have the flexibility to switch between plans.

## management/v1

### GET /management/v1/service-offers/{serviceOfferId}/products

Returns a list of products associated with the serviceOfferId.

#### Parameters

| Parameters                     |                                       |
|--------------------------------|---------------------------------------|
| serviceOfferId  (required)     | UUID assigned to the service offer.        |
|                                | Available values :    |
|                                | string ($uuid)                       |
| Accept  (required)             | Accept header                         |
|                                | Available values :   application/json |
|                                | string (header)                       |

#### Responses

Response content type: application/json

| Code | Description                                                          |
|------|----------------------------------------------------------------------|
| 200  |         Successfully retrieved products that match serviceOfferId. |
| 401  |         Unauthorized.                                                |
| 403  |         RBAC access denied.                                           |
| 404  |         Resource not found.                                         |
| 415  |         Invalid Accept header.                             |
| 500  |         Internal server error.                                       |

Example Response Value:

```json
[
  {
    "id": "a680e5d0-23e5-4b3c-9174-fb99ffa96e16",
    "service_offer_id": "d9949ae4-26f4-4b5f-896b-c554afa619d7",
    "name": "Basic",
    "policy": {
        "limit": 60,
        "quota": 100,
        "limit_renewal_period": 60,
        "quota_renewal_period": 604801
      },
    "plan_id" : "259a289e-48bd-4caa-8470-a2e1839675c6",
    "product_type": "attestation"
  }
]
```

#### Extensions

| Field                  | Value          |
|------------------------|-------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/service-offers/d9949ae4-26f4-4b5f-896b-c554afa619d7/products"    |


### GET /management/v1/service-offers/{serviceOfferId}/products/{productId}

Gets details of the a product specified by productId, for the service offer specified by serviceOfferId.

#### Parameters

| Parameters                     |                                       |
|--------------------------------|---------------------------------------|
| serviceOfferId  (required)     | UUID assigned to the service offer.        |
|                                | Available values :    |
|                                | string ($uuid)                       |
| Accept  (required)             | Accept header                         |
|                                | Available values :   application/json |
|                                | string (header)                       |

#### Responses

Response content type: application/json

| Code | Description                                                          |
|------|----------------------------------------------------------------------|
| 200  |         Successfully retrieved the product details. |
| 401  |         Unauthorized.                                                |
| 403  |         RBAC access denied.                                           |
| 404  |         Resource not found.                                         |
| 415  |         Invalid Accept header.                             |
| 500  |         Internal server error.                                       |

Example Response Value:

```json
[
  {
      "id": "a680e5d0-23e5-4b3c-9174-fb99ffa96e16",
      "service_offer_id": "d9949ae4-26f4-4b5f-896b-c554afa619d7",
      "name": "Basic",
      "policy": {
          "limit": 60,
          "quota": 100,
          "limit_renewal_period": 60,
          "quota_renewal_period": 604801
        },
      "plan_id" : "259a289e-48bd-4caa-8470-a2e1839675c6",
        "product_type": "attestation"
  }
]
```

#### Extensions

| Field                  | Value          |
|------------------------|-------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/service-offers/d9949ae4-26f4-4b5f-896b-c554afa619d7/products"    |
