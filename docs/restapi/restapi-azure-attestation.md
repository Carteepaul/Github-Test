---
title: Project Amber Azure Attestation Swagger Doc
description: Project Amber Azure Attestation REST API external documentation
author: grminch
topic-type: NA
date: 03/16/2023
---

# Project Amber Azure Attestation REST API

The Project Amber Adapter Service for Microsoft Azure Attestation (MAA) lets existing MAA attestation clients switch to Project Amber attestation with minimal changes. Project Amber MAA Adapter interfaces use fully MAA-compatible request and response formats.

## azure-attestation

### POST /azure-attestation/attest/OpenEnclave

Verify SgxEnclave evidence and issue an MAA-format attestation token for a successful attestation request. In case of attestation failure, an error message is generated.

#### Parameters

| Parameters                     |                                       |
|--------------------------------|---------------------------------------|
| Content-Type  (required)       | Content-Type header                   |
|                                | Available values :   application/json |
|                                | string (header)                       |
| Accept  (required)             | Accept header                         |
|                                | Available values :   application/json |
|                                | string (header)                       |

#### Request Body
```json
{
  "draftPolicyForAttestation": "string",
  "initTimeData": {
    "data": "string",
    "dataType": "string"
  },
  "nonce": "string",
  "report": "AQAAAAIAAAD4EQAAAAAAAAMAAgAAAAAABwAMAJOacjP3nEyplAoNs5V_BgeF68hrWDb5I54zc3kuLeAmAAAAABMTAgf_g ... LUVORCBDRVJUSUZJQ0FURS0tLS0tCgA",
  "runtimeData": {
    "data": "string",
    "dataType": "string"
  }
}
```

#### Responses

Response content type: application/json

| Code | Description                                                          |
|------|----------------------------------------------------------------------|
| 200  |         Successfully created generated attestation token.   |
| 400  |         Invalid request body.                                |
| 403  |         RBAC access denied.                                          |
| 404  |         Resource not found.                                          |
| 500  |         Internal server error.                                       |

Example Response Value:

```json
[
  {
  "token": "eyJhbGciOiJQUzM4NCIsImtpZCI6IjBjZWFiOThlMWEyZjZhMDI4NThmZTczNWFkMzFhYWQ4Y2Q0M2Q4YWUiLCJ0eXAiOiJKV1QifQ.eyJtcmVuY2xh..."
  }
]
```

#### Extensions

| Field | Value |
|---    |---    |
| x-sample-call-endpoint | "https://amber.com/azure-attestation/attest/OpenEnclave" |


### POST /azure-attestation/attest/SgxEnclave

Verify SgxEnclave evidence and issue an MAA-format attestation token for a successful attestation request. In case of attestation failure, an error message is generated.

#### Parameters

| Parameters                     |                                       |
|--------------------------------|---------------------------------------|
| Content-Type  (required)       | Content-Type header                   |
|                                | Available values :   application/json |
|                                | string (header)                       |
| Accept  (required)             | Accept header                         |
|                                | Available values :   application/json |
|                                | string (header)                       |

#### Request Body
```json
{
  "quote": "AwACAAAAAAAHAAwAk5pyM_ecTKmUCg2zlX8GB2dLbI9PK1yLCrYtYE00KUAAAAAABQgDDP__AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAhQAAAAAAAADnA ... ",
  "RuntimeData": {
    "Data": "AQIDBAUG",
    "DataType": "Binary"
  },
  "InittimeData": {
    "Data": "eyJwbGF0Zm9ybSI6ImljZWxha2UifQ",
    "DataType": "JSON"
  },
  "DraftPolicyForAttestation": null
}
```

#### Responses

Response content type: application/json

| Code | Description                                                          |
|------|----------------------------------------------------------------------|
| 200  |         Successfully created an attestation token.   |
| 400  |         Invalid request body.                                |
| 403  |         RBAC access denied.                                          |
| 500  |         Internal server error.                                       |

Example Response Value:

```json
[
  {
  "token": "eyJhbGciOiJQUzM4NCIsImtpZCI6IjBjZWFiOThlMWEyZjZhMDI4NThmZTczNWFkMzFhYWQ4Y2Q0M2Q4YWUiLCJ0eXAiOiJKV1QifQ.eyJtcmVuY2xh..."
  }
]
```

#### Extensions

| Field | Value |
|---    |---    |
| x-sample-call-endpoint | "https://amber.com/azure-attestation/attest/SgxEnclave" |