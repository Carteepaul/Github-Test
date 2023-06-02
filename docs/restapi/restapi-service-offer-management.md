---
title: Project Amber Service Offer Management REST API
description: External swagger-like documentation for the Service Offer API.
author: grminch
topic-type: NA
date: 03/16/2023
---

# Project Amber Service Offer Management

This set of APIs lets you find all service offers supported by your Project Amber deployment, get details for a given service offer, and get the attestation and policy types and descriptions for a service offer. 

## /management/v1/service-offers

### GET /management/v1/service-offers

Returns all the service offers supported in the system.

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
| 200  |         Successfully retrieved the service offers. |
| 401  |         Unauthorized.                                                |
| 403  |         RBAC access denied.                                           |
| 404  |         Resource not found.                                          |
| 415  |         Invalid Accept header.                             |
| 500  |         Internal server error.                                       |

Example Response Value:

```json
[
    {
      "id": "d9949ae4-26f4-4b5f-896b-c554afa619d7",
      "name": "TEE Attestation"
    }
]
```

#### Extensions

| Field   | Value |
|---      |---    |
|x-sample-call-endpoint | https://amber.com:443/management/v1/service-offers |


### GET /management/v1/service-offers/{serviceOfferId}

Gets the service offer details for the specified serviceOfferId.

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
| 200  |         Successfully retrieved the service offer details. |
| 401  |         Unauthorized.                                                |
| 403  |         RBAC access denied.                                           |
| 404  |         Resource not found.                                          |
| 415  |         Invalid Accept header.                             |
| 500  |         Internal server error.                                       |

Example Response Value:

```json
[
    {
      "id": "d9949ae4-26f4-4b5f-896b-c554afa619d7",
      "name": "TEE Attestation"
    }
]
```

#### Extensions

| Field   | Value |
|---      |---    |
|x-sample-call-endpoint | "https://amber.com:443/management/v1/service-offers/d9949ae4-26f4-4b5f-896b-c554afa619d7" |

### GET /management/v1/service-offers/{serviceOfferId}/attestation-policy-type

Retrieves attestation and policy type and descriptions for a given serviceOfferId.

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
| 200  |         Successfully retrieved the service offer's attestation types. |
| 401  |         Unauthorized.                                                |
| 403  |         RBAC access denied.                                           |
| 404  |         Resource not found.                                          |
| 415  |         Invalid Accept header.                             |
| 500  |         Internal server error.                                       |

Example Response Value:

```json
{
    "id": "30c6dc6c-c514-424c-abf6-7a8dfe76cd7d",
    "name": "TEE Attestation",
    "attestation_policy_type": [
        {
            "attestation_type": "SGX",
            "attestation_type_description": "SGX Attestation type Desc",
            "policy_type": "Appraisal",
            "policy_type_description": "Policy type desc for Appraisal"
        },
        {
            "attestation_type": "SGX",
            "attestation_type_description": "SGX Attestation type Desc",
            "policy_type": "Token",
            "policy_type_description": "Policy type desc for Token"
        },
        {
            "attestation_type": "TDX",
            "attestation_type_description": "TDX Attestation type Desc",
            "policy_type": "Appraisal",
            "policy_type_description": "Policy type desc for Appraisal"
        },
        {
            "attestation_type": "TDX",
            "attestation_type_description": "TDX Attestation type Desc",
            "policy_type": "Token",
            "policy_type_description": "Policy type desc for Token"
        }
    ]
}
```

#### Extensions

| Field   | Value |
|---      |---    |
|x-sample-call-endpoint | "https://amber.com:443/management/v1/service-offers/30c6dc6c-c514-424c-abf6-7a8dfe76cd7d/attestation-policy-type" |