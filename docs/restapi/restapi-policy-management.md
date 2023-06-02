---
title: Project Amber Policy Management Swagger doc
description: Policy Management external API definition
author: Tim Knoll
topic-type: NA
date: 03/16/2023
---



# Policy Management

Policy Management enables users to create and manage rules that inspect the quote generated and submitted by
the client/workload during execution. The Policy Management allows Users to Create, Update, Delete and
Search SGX and TDX policies. The policies have an attestation-specific OPA format defined in Rego language and
are stored as JSON.
There can be two types of policies - Appraisal or Token customization. The Appraisal policy includes details like
server firmware measurements, hardware capabilities, security technology information,
and additional workload-specific parameters. Token customization policy includes user defined fields to be
populated in the issued Attestation Token.
Policy(s) are applied and verified against the quote\evidence provided by client during the attestation request.
The output returned is a signed JWT token that can be parsed to verify the policies evaluated.

## Policies

### DELETE /management/v1/policies

Deletes all custom policies associated with the specified attestationType

#### Parameters

| Parameters        |                                                       |
|-------------------|-------------------------------------------------------|
| attestationType * | Type of attestation [SGX Attestation/TDX Attestation] |
| string (query)    | Available values : TDX Attestation, SGX Attestation   |

#### Responses

Response content type: application/json

| Code | Description                      |
|------|----------------------------------|
| 204  | Successfully deleted the policy. |
| 400  | Bad request.                     |
| 403  | RBAC access denied               |
| 404  | Resource not found.              |
| 500  | Internal server error            |



#### Extensions

| Field                  | Value                                                                          |
|------------------------|--------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/policies?attestationType=SGX Attestation" |


### GET /management/v1/policies

Returns all the user defined policies matching the search criteria specified by a user

#### Parameters

| Parameters                     |                                       |
|--------------------------------|---------------------------------------|
| Content-Type  (required)       | Content-Type header                   |
|                                | Available values :   application/json |
|                                | string (header)                       |
| Accept  (required)             | Accept header                         |
|                                | Available values :   application/json |
|                                | string (header)                       |

#### Responses

Response content type: application/json

| Code | Description                                                          |
|------|----------------------------------------------------------------------|
| 200  |         Successfully retrieved the   ApiClients that match criteria. |
| 401  |         Unauthorized                                                 |
| 403  |         RBAC access denied                                           |
| 404  |         Resource Not Found.                                          |
| 415  |         Invalid Accept Header in Request                             |
| 500  |         Internal server error.                                       |

Example Response Value:

```json
[
  {
    "attestation_type": "SGX Attestation",
    "created_time": "0001-01-01T00:00:00Z",
    "creator_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "deleted": false,
    "modified_time": "0001-01-01T00:00:00Z",
    "policy": "default matches_sgx_policy = false \\n\\n matches_sgx_policy = true { \\n input.amber_sgx_is_debuggable == false \\n input.amber_sgx_isvsvn == 0 \\n input.amber_sgx_isvprodid == 0 \\n input.amber_sgx_mrsigner ==  \\\"d412a4f07ef83892a5915fb2ab584be31e186e5a4f95ab5f6950fd4eb8694d7b\\\" \\n  \\n input.amber_sgx_mrenclave == \\\"bab91f200038076ac25f87de0ca67472443c2ebe17ed9ba95314e609038f51ab\\\"",
    "policy_hash": "uJzueTbG8uTrkoBcSG7Duu2izGJ2lAZveRqx0E/exGnX81/4kJdU1Wh1FDkn0K8+",
    "policy_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
    "policy_jwt": "uJzueTbG8uTrkoBcSG7Duu2izGJ2lAZveRqx0E/exGnX81/4kJdU1Wh1FDkn0K8+",
    "policy_name": "Sample_Policy_Test",
    "policy_signature": "TzHW7ZvLMPUpj3v//XtHEF2x2oT+Hl1bj/wgOc0Anayslux/bGDKD6hyFxJwdPT0HEQDRpOhYAgt0jr+OhmF1y43Qp/t5P7dRo8rX+HIOj8ThmEk2pGX3Yoyy3bm/1P/CGy2dI/KmP/J4QwrLpdz3ApUFU421aB+mLiG3vYme5VpiLJEcTGKKmFslWxYg7HZ07VScw2yPk49y7H4ZWTWgdVOhb4L4UVeuXGxtXKFrRDcdZKBg/kLU3UUuoNIxgE8xdxEh4TX/iD0bmpb6Y69KGLegxb0bPiAGWfq/vF8k3GdVFUxkDYrn+ogiJkXxulyYrbPIkapilOn9Clb7Mm1p+5X5aH3kakLjR2IwGt5LFyVZWFjBYdf39GHLS8wN/U+BFInB4WkcBED0rYe7cHv1yYgUkxrii/nAxhTNc4x7LYwvU3eidvl9AExlxUHm92vhnPMTvAQq6xg/S513UNgnzIxyO1FYbw+FA6WBDX/LJmU6+6bjJ44xaI6z29VA13N",
    "policy_type": "Appraisal policy",
    "service_offer_id": "7ba36c8a-59b5-4f0e-a595-1c2945d5a1ce",
    "updater_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8"
  }
]
```

#### Extensions

| Field                  | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/policies?attestationType=SGX Attestation"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| x-sample-call-output   | "[\n {\n \"policy_id\": \"1722b479-cbba-4f10-9ac8-c21dff5bf8db\",\n \"policy\": \"default matches_sgx_policy = false \\n\\n matches_sgx_policy = true { \\n input.amber_sgx_is_debuggable == false \\n input.amber_sgx_isvsvn == 0 \\n input.amber_sgx_isvprodid == 0 \\n input.amber_sgx_mrsigner == \\\"d412a4f07ef83892a5915fb2ab584be31e186e5a4f95ab5f6950fd4eb8694d7b\\\" \\n \\n input.amber_sgx_mrenclave == \\\"bab91f200038076ac25f87de0ca67472443c2ebe17ed9ba95314e609038f51ab\\\" \\n }\",\n \"creator_id\": \"7110194b-a703-4657-9d7f-3e02b62f2ed8\",\n \"policy_name\": \"Sample_Policy_Test1\",\n \"policy_type\": \"Appraisal policy\",\n \"policy_jwt\": \"eyJhbGciOiJub25lIn0.eyJBdHRlc3RhdGlvblBvbGljeSI6ImRlZmF1bHQgbWF0Y2hlc19zZ3hfcG9saWN5ID0gZmFsc2VcbiJ9.\",\n \"service_offer_id\": \"7ba36c8a-59b5-4f0e-a595-1c2945d5a1ce\",\n \"attestation_type\": \"SGX Attestation\",\n \"updater_id\": \"7110194b-a703-4657-9d7f-3e02b62f2ed8\",\n \"deleted\": false,\n \"created_time\": \"0001-01-01T00:00:00Z\",\n \"modified_time\": \"0001-01-01T00:00:00Z\",\n \"policy_hash\": \"uJzueTbG8uTrkoBcSG7Duu2izGJ2lAZveRqx0E/exGnX81/4kJdU1Wh1FDkn0K8+\",\n \"policy_signature\": \"TzHW7ZvLMPUpj3v//XtHEF2x2oT+Hl1bj/wgOc0Anayslux/bGDKD6hyFxJwdPT0HEQDRpOhYAgt0jr+OhmF1y43Qp/t5P7dRo8rX+HIOj8ThmEk2pGX3Yoyy3bm/1P/CGy2dI/KmP/J4QwrLpdz3ApUFU421aB+mLiG3vYme5VpiLJEcTGKKmFslWxYg7HZ07VScw2yPk49y7H4ZWTWgdVOhb4L4UVeuXGxtXKFrRDcdZKBg/kLU3UUuoNIxgE8xdxEh4TX/iD0bmpb6Y69KGLegxb0bPiAGWfq/vF8k3GdVFUxkDYrn+ogiJkXxulyYrbPIkapilOn9Clb7Mm1p+5X5aH3kakLjR2IwGt5LFyVZWFjBYdf39GHLS8wN/U+BFInB4WkcBED0rYe7cHv1yYgUkxrii/nAxhTNc4x7LYwvU3eidvl9AExlxUHm92vhnPMTvAQq6xg/S513UNgnzIxyO1FYbw+FA6WBDX/LJmU6+6bjJ44xaI6z29VA13N\"\n },\n {\n \"policy_id\": \"14bd8d98-bca6-490a-9836-32b2cb46509f\",\n \"policy\": \"default matches_sgx_policy = false\\n\",\n \"creator_id\": \"7110194b-a703-4657-9d7f-3e02b62f2ed8\",\n \"policy_name\": \"Sample_Policy_Test2\",\n \"policy_type\": \"Appraisal policy\",\n \"policy_jwt\": \"eyJhbGciOiJub25lIn0.eyJBdHRlc3RhdGlvblBvbGljeSI6ImRlZmF1bHQgbWF0Y2hlc19zZ3hfcG9saWN5ID0gZmFsc2VcbiJ9.\",\n \"service_offer_id\": \"7ba36c8a-59b5-4f0e-a595-1c2945d5a1ce\",\n \"attestation_type\": \"SGX Attestation\",\n \"updater_id\": \"7110194b-a703-4657-9d7f-3e02b62f2ed8\",\n \"deleted\": false,\n \"created_time\": \"0001-01-01T00:00:00Z\",\n \"modified_time\": \"0001-01-01T00:00:00Z\",\n \"policy_hash\": \"pUCgG9csD+mFT0+zl0/j3VQTEH9qoMrIEJalbCi69y3BtPsgD/4VG3l+2zcwZWcf\",\n \"policy_signature\": \"eTZfByb9eoPx+XySh9TTgEMWzdw65uRh+cd7L8Jy7Kaof1oIm52eOCb0AZYr6Ee4FXjKSU0TlpvrMLvJ5PtisfLzfRiY1H57PP/ZSKLFmHOVjs7gMhMrWLaI0AZ8quD02En8kTAbwj9Ad0/oGiZEcy+oVpvZRQh795DRBFIA9Woecnz+fhr/sIYf45oRCN05ptR2oA6TJRs+232ynoU3cabALzmDN+PlpdCsqgRF6MHjUwaZRIwsYrhq6L9IRusvOdxAg0vBIdbYHaAmqc0mZxx0ie7U6hrmYduuJd3frBZI6lAMpYwMsDrgk9/Ka3DIhqxAQVIJpQTXnZ0/dT2uK/urdPZ1qJud4d9aEQAJlTyRi46IUSH9dUEfGL/yLMni0jxhLwaUD0WqJ4CJHsh9WdIT7cGtpKHK5XCZMSG0OgsUTH/6Hy0GEHWAHXUM/vY8PxIhuTnU6h+tjIK/6yZSpkXlAqaMXrDm74o3QFWMsidveiKByKGGvP5eBIsSv/t6\"\n }\n]" |


### POST /management/v1/policies

Creates a new policy with a user specified input parameters

#### Parameters

| Parameters                     |                                       |
|--------------------------------|---------------------------------------|
| Content-Type  (required)       | Content-Type header                   |
|                                | Available values :   application/json |
|                                | string (header)                       |
| Accept  (required)             | Accept header                         |
|                                | Available values :   application/json |
|                                | string (header)                       |

##### Request Body

Parameter content type:
application/json

```json
{
  "attestation_type": "SGX Attestation",
  "policy": "default matches_sgx_policy = false \\n\\n matches_sgx_policy = true { \\n input.amber_sgx_is_debuggable == false \\n input.amber_sgx_isvsvn == 0 \\n input.amber_sgx_isvprodid == 0 \\n input.amber_sgx_mrsigner ==  \\\"d412a4f07ef83892a5915fb2ab584be31e186e5a4f95ab5f6950fd4eb8694d7b\\\" \\n  \\n input.amber_sgx_mrenclave == \\\"bab91f200038076ac25f87de0ca67472443c2ebe17ed9ba95314e609038f51ab\\\"",
  "policy_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "policy_name": "Sample_Policy_Test",
  "policy_type": "Appraisal policy",
  "service_offer_id": "7ba36c8a-59b5-4f0e-a595-1c2945d5a1ce"
}
```

#### Responses

Response content type: application/json

| Code | Description                                                          |
|------|----------------------------------------------------------------------|
| 201  |         Successfully created the policy.                             |
| 400  |         Invalid request body provided                                |
| 403  |         RBAC access denied                                           |
| 415  |         Invalid Content-Type/Accept Header in Request                |
| 500  |         Internal server error                                        |

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

| Field                  | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/policies"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| x-sample-call-input    | "{\n \"policy\": \"default matches_sgx_policy = false \\n\\n matches_sgx_policy = true { \\n input.amber_sgx_is_debuggable == false \\n input.amber_sgx_isvsvn == 0 \\n input.amber_sgx_isvprodid == 0 \\n input.amber_sgx_mrsigner == \\\"d412a4f07ef83892a5915fb2ab584be31e186e5a4f95ab5f6950fd4eb8694d7b\\\" \\n \\n input.amber_sgx_mrenclave == \\\"bab91f200038076ac25f87de0ca67472443c2ebe17ed9ba95314e609038f51ab\\\" \\n }\",\n \"policy_name\": \"Sample_Policy_Test1\",\n \"policy_type\": \"Appraisal policy\",\n \"service_offer_id\": \"7ba36c8a-59b5-4f0e-a595-1c2945d5a1ce\",\n \"attestation_type\": \"SGX Attestation\"\n}\n"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| x-sample-call-output   | "{\n \"policy_id\": \"1722b479-cbba-4f10-9ac8-c21dff5bf8db\",\n \"policy\": \"default matches_sgx_policy = false \\n\\n matches_sgx_policy = true { \\n input.amber_sgx_is_debuggable == false \\n input.amber_sgx_isvsvn == 0 \\n input.amber_sgx_isvprodid == 0 \\n input.amber_sgx_mrsigner == \\\"d412a4f07ef83892a5915fb2ab584be31e186e5a4f95ab5f6950fd4eb8694d7b\\\" \\n \\n input.amber_sgx_mrenclave == \\\"bab91f200038076ac25f87de0ca67472443c2ebe17ed9ba95314e609038f51ab\\\" \\n }\",\n \"creator_id\": \"7110194b-a703-4657-9d7f-3e02b62f2ed8\",\n \"policy_name\": \"Sample_Policy_Test1\",\n \"policy_type\": \"Appraisal policy\",\n \"policy_jwt\": \"\",\n \"service_offer_id\": \"7ba36c8a-59b5-4f0e-a595-1c2945d5a1ce\",\n \"attestation_type\": \"SGX Attestation\",\n \"updater_id\": \"7110194b-a703-4657-9d7f-3e02b62f2ed8\",\n \"deleted\": false,\n \"created_time\": \"0001-01-01T00:00:00Z\",\n \"modified_time\": \"0001-01-01T00:00:00Z\",\n \"policy_hash\": \"uJzueTbG8uTrkoBcSG7Duu2izGJ2lAZveRqx0E/exGnX81/4kJdU1Wh1FDkn0K8+\",\n \"policy_signature\": \"TzHW7ZvLMPUpj3v//XtHEF2x2oT+Hl1bj/wgOc0Anayslux/bGDKD6hyFxJwdPT0HEQDRpOhYAgt0jr+OhmF1y43Qp/t5P7dRo8rX+HIOj8ThmEk2pGX3Yoyy3bm/1P/CGy2dI/KmP/J4QwrLpdz3ApUFU421aB+mLiG3vYme5VpiLJEcTGKKmFslWxYg7HZ07VScw2yPk49y7H4ZWTWgdVOhb4L4UVeuXGxtXKFrRDcdZKBg/kLU3UUuoNIxgE8xdxEh4TX/iD0bmpb6Y69KGLegxb0bPiAGWfq/vF8k3GdVFUxkDYrn+ogiJkXxulyYrbPIkapilOn9Clb7Mm1p+5X5aH3kakLjR2IwGt5LFyVZWFjBYdf39GHLS8wN/U+BFInB4WkcBED0rYe7cHv1yYgUkxrii/nAxhTNc4x7LYwvU3eidvl9AExlxUHm92vhnPMTvAQq6xg/S513UNgnzIxyO1FYbw+FA6WBDX/LJmU6+6bjJ44xaI6z29VA13N\"\n}" |


### DELETE /management/v1/policies/{policyId}

Deletes the policy for the specified policyId

#### Parameters

| Name                 | Description                                         |
|----------------------|-----------------------------------------------------|
| policyId (required)  | Universal Unique IDentifier assigned to the policy. |
| string($uuid) (path) |                                                     |

#### Responses

Response content type: application/json

| Code | Description                      |
|------|----------------------------------|
| 204  | Successfully deleted the policy. |
| 400  | Bad request.                     |
| 403  | RBAC access denied               |
| 404  | Resource Not Found.              |
| 500  | Internal server error            |


#### Extensions

| Field                  | Value                                                                               |
|------------------------|-------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/policies/b02b67cc-02a9-4fc0-a558-889a5f00cd47" |


### GET /management/v1/policies/{policyId}

Returns the policy attributes in the JSON format for the specified policyId.

#### Parameters

| Parameters                     |                                                     |
|--------------------------------|-----------------------------------------------------|
| policyId (required)            | Universal Unique IDentifier assigned to the policy. |
| string($uuid) (path)           |                                                     |
| Accept  (required)             | Accept header                                       |
|                                | Available values :   application/json               |
|                                | string (header)                                     |

#### Responses

Response content type: application/json

| Code | Description                                                          |
|------|----------------------------------------------------------------------|
| 200  |         Successfully retrieved the policy.                           |
| 400  |         Bad request.                                                 |
| 403  |         RBAC access denied                                           |
| 404  |         Resource Not Found.                                          |
| 415  |         Invalid Accept Header in Request                             |
| 500  |         Internal server error.                                       |

Example Response Value:

```json
{
  "attestation_type": "SGX Attestation",
  "created_time": "0001-01-01T00:00:00Z",
  "creator_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "deleted": false,
  "modified_time": "0001-01-01T00:00:00Z",
  "policy": "default matches_sgx_policy = false \\n\\n matches_sgx_policy = true { \\n input.amber_sgx_is_debuggable == false \\n input.amber_sgx_isvsvn == 0 \\n input.amber_sgx_isvprodid == 0 \\n input.amber_sgx_mrsigner ==  \\\"d412a4f07ef83892a5915fb2ab584be31e186e5a4f95ab5f6950fd4eb8694d7b\\\" \\n  \\n input.amber_sgx_mrenclave == \\\"bab91f200038076ac25f87de0ca67472443c2ebe17ed9ba95314e609038f51ab\\\"",
  "policy_hash": "uJzueTbG8uTrkoBcSG7Duu2izGJ2lAZveRqx0E/exGnX81/4kJdU1Wh1FDkn0K8+",
  "policy_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "policy_jwt": "uJzueTbG8uTrkoBcSG7Duu2izGJ2lAZveRqx0E/exGnX81/4kJdU1Wh1FDkn0K8+",
  "policy_name": "Sample_Policy_Test",
  "policy_signature": "TzHW7ZvLMPUpj3v//XtHEF2x2oT+Hl1bj/wgOc0Anayslux/bGDKD6hyFxJwdPT0HEQDRpOhYAgt0jr+OhmF1y43Qp/t5P7dRo8rX+HIOj8ThmEk2pGX3Yoyy3bm/1P/CGy2dI/KmP/J4QwrLpdz3ApUFU421aB+mLiG3vYme5VpiLJEcTGKKmFslWxYg7HZ07VScw2yPk49y7H4ZWTWgdVOhb4L4UVeuXGxtXKFrRDcdZKBg/kLU3UUuoNIxgE8xdxEh4TX/iD0bmpb6Y69KGLegxb0bPiAGWfq/vF8k3GdVFUxkDYrn+ogiJkXxulyYrbPIkapilOn9Clb7Mm1p+5X5aH3kakLjR2IwGt5LFyVZWFjBYdf39GHLS8wN/U+BFInB4WkcBED0rYe7cHv1yYgUkxrii/nAxhTNc4x7LYwvU3eidvl9AExlxUHm92vhnPMTvAQq6xg/S513UNgnzIxyO1FYbw+FA6WBDX/LJmU6+6bjJ44xaI6z29VA13N",
  "policy_type": "Appraisal policy",
  "service_offer_id": "7ba36c8a-59b5-4f0e-a595-1c2945d5a1ce",
  "updater_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8"
}
```

#### Extensions

| Field                  | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/policies/b02b67cc-02a9-4fc0-a558-889a5f00cd47"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| x-sample-call-output   | "{\n \"policy_id\": \"b02b67cc-02a9-4fc0-a558-889a5f00cd47\",\n \"policy\": \"default matches_sgx_policy = false \\n\\n matches_sgx_policy = true { \\n input.amber_sgx_is_debuggable == false \\n input.amber_sgx_isvsvn == 0 \\n input.amber_sgx_isvprodid == 0 \\n input.amber_sgx_mrsigner == \\\"d412a4f07ef83892a5915fb2ab584be31e186e5a4f95ab5f6950fd4eb8694d7b\\\" \\n \\n input.amber_sgx_mrenclave == \\\"bab91f200038076ac25f87de0ca67472443c2ebe17ed9ba95314e609038f51ab\\\" \\n }\",\n \"creator_id\": \"7110194b-a703-4657-9d7f-3e02b62f2ed8\",\n \"policy_name\": \"Sample_Policy_Test1\",\n \"policy_type\": \"Appraisal policy\",\n \"policy_jwt\": \"eyJhbGciOiJub25lIn0.eyJBdHRlc3RhdGlvblBvbGljeSI6ImRlZmF1bHQgbWF0Y2hlc19zZ3hfcG9saWN5ID0gZmFsc2VcbiJ9.\",\n \"service_offer_id\": \"7ba36c8a-59b5-4f0e-a595-1c2945d5a1ce\",\n \"attestation_type\": \"SGX Attestation\",\n \"updater_id\": \"7110194b-a703-4657-9d7f-3e02b62f2ed8\",\n \"deleted\": false,\n \"created_time\": \"0001-01-01T00:00:00Z\",\n \"modified_time\": \"0001-01-01T00:00:00Z\",\n \"policy_hash\": \"uJzueTbG8uTrkoBcSG7Duu2izGJ2lAZveRqx0E/exGnX81/4kJdU1Wh1FDkn0K8+\",\n \"policy_signature\": \"TzHW7ZvLMPUpj3v//XtHEF2x2oT+Hl1bj/wgOc0Anayslux/bGDKD6hyFxJwdPT0HEQDRpOhYAgt0jr+OhmF1y43Qp/t5P7dRo8rX+HIOj8ThmEk2pGX3Yoyy3bm/1P/CGy2dI/KmP/J4QwrLpdz3ApUFU421aB+mLiG3vYme5VpiLJEcTGKKmFslWxYg7HZ07VScw2yPk49y7H4ZWTWgdVOhb4L4UVeuXGxtXKFrRDcdZKBg/kLU3UUuoNIxgE8xdxEh4TX/iD0bmpb6Y69KGLegxb0bPiAGWfq/vF8k3GdVFUxkDYrn+ogiJkXxulyYrbPIkapilOn9Clb7Mm1p+5X5aH3kakLjR2IwGt5LFyVZWFjBYdf39GHLS8wN/U+BFInB4WkcBED0rYe7cHv1yYgUkxrii/nAxhTNc4x7LYwvU3eidvl9AExlxUHm92vhnPMTvAQq6xg/S513UNgnzIxyO1FYbw+FA6WBDX/LJmU6+6bjJ44xaI6z29VA13N\"\n}" |


### PUT /management/v1/policies/{policyId}

Updates the allowed custom policy attributes like name, type, policy content, etc. for the specified policyId

#### Parameters

| Parameters                     |                                                     |
|--------------------------------|-----------------------------------------------------|
| policyId (required)            | Universal Unique IDentifier assigned to the policy. |
| string($uuid) (path)           |                                                     |
|                                |                                                     |
| Content-Type  (required)       | Content-Type header                                 |
|                                | Available values :   application/json               |
|                                | string (header)                                     |
|                                |                                                     |
| Accept  (required)             | Accept header                                       |
|                                | Available values :   application/json               |
|                                | string (header)                                     |

##### Request Body

Parameter content type:
application/json

```json
{
  "_": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "policy": "default matches_sgx_policy = false \\n\\n matches_sgx_policy = true { \\n input.amber_sgx_is_debuggable == false \\n input.amber_sgx_isvsvn == 0 \\n input.amber_sgx_isvprodid == 0 \\n input.amber_sgx_mrsigner ==  \\\"d412a4f07ef83892a5915fb2ab584be31e186e5a4f95ab5f6950fd4eb8694d7b\\\" \\n  \\n input.amber_sgx_mrenclave == \\\"bab91f200038076ac25f87de0ca67472443c2ebe17ed9ba95314e609038f51ab\\\"",
  "policy_id": "7110194b-a703-4657-9d7f-3e02b62f2ed8",
  "policy_name": "Sample_Policy_Test"
}
```

#### Responses

Response content type: application/json

| Code | Description                                  |
|------|----------------------------------------------|
| 200  |         Successfully updated the policy.     |
| 400 | Invalid request body provided                 |
| 403 | RBAC access denied                            |
| 404 | Resource Not Found.                           |
| 415 | Invalid Content-Type/Accept Header in Request |
| 500 | Internal server error                         |

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

| Field                  | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/policies/71d768be-e86a-40c8-b14a-00f984b67125"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| x-sample-call-input    | "{\n \"policy_id\": \"71d768be-e86a-40c8-b14a-00f984b67125\",\n \"policy\": \"default matches_sgx_policy = false \\n\\n matches_sgx_policy = true { \\n input.amber_sgx_is_debuggable == false \\n input.amber_sgx_isvsvn == 0 \\n input.amber_sgx_isvprodid == 0 \\n input.amber_sgx_mrsigner == \\\"d412a4f07ef83892a5915fb2ab584be31e186e5a4f95ab5f6950fd4eb8694d7b\\\" \\n \\n input.amber_sgx_mrenclave == \\\"bab91f200038076ac25f87de0ca67472443c2ebe17ed9ba95314e609038f51ab\\\" \\n }\",\n \"policy_name\": \"Sample_Policy_Test1\"\n}\n"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| x-sample-call-output   | "{\n \"policy_id\": \"71d768be-e86a-40c8-b14a-00f984b67125\",\n \"policy\": \"default matches_sgx_policy = false \\n\\n matches_sgx_policy = true { \\n input.amber_sgx_is_debuggable == false \\n input.amber_sgx_isvsvn == 0 \\n input.amber_sgx_isvprodid == 0 \\n input.amber_sgx_mrsigner == \\\"d412a4f07ef83892a5915fb2ab584be31e186e5a4f95ab5f6950fd4eb8694d7b\\\" \\n \\n input.amber_sgx_mrenclave == \\\"bab91f200038076ac25f87de0ca67472443c2ebe17ed9ba95314e609038f51ab\\\" \\n }\",\n \"creator_id\": \"7110194b-a703-4657-9d7f-3e02b62f2ed8\",\n \"policy_name\": \"Sample_Policy_Test1\",\n \"policy_type\": \"Appraisal policy\",\n \"policy_jwt\": \"\",\n \"service_offer_id\": \"7ba36c8a-59b5-4f0e-a595-1c2945d5a1ce\",\n \"attestation_type\": \"SGX Attestation\",\n \"updater_id\": \"7110194b-a703-4657-9d7f-3e02b62f2ed8\",\n \"deleted\": false,\n \"created_time\": \"0001-01-01T00:00:00Z\",\n \"modified_time\": \"0001-01-01T00:00:00Z\",\n \"policy_hash\": \"uJzueTbG8uTrkoBcSG7Duu2izGJ2lAZveRqx0E/exGnX81/4kJdU1Wh1FDkn0K8+\",\n \"policy_signature\": \"TzHW7ZvLMPUpj3v//XtHEF2x2oT+Hl1bj/wgOc0Anayslux/bGDKD6hyFxJwdPT0HEQDRpOhYAgt0jr+OhmF1y43Qp/t5P7dRo8rX+HIOj8ThmEk2pGX3Yoyy3bm/1P/CGy2dI/KmP/J4QwrLpdz3ApUFU421aB+mLiG3vYme5VpiLJEcTGKKmFslWxYg7HZ07VScw2yPk49y7H4ZWTWgdVOhb4L4UVeuXGxtXKFrRDcdZKBg/kLU3UUuoNIxgE8xdxEh4TX/iD0bmpb6Y69KGLegxb0bPiAGWfq/vF8k3GdVFUxkDYrn+ogiJkXxulyYrbPIkapilOn9Clb7Mm1p+5X5aH3kakLjR2IwGt5LFyVZWFjBYdf39GHLS8wN/U+BFInB4WkcBED0rYe7cHv1yYgUkxrii/nAxhTNc4x7LYwvU3eidvl9AExlxUHm92vhnPMTvAQq6xg/S513UNgnzIxyO1FYbw+FA6WBDX/LJmU6+6bjJ44xaI6z29VA13N\"\n}" |


