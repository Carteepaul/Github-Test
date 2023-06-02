---
title: Project Amber API template example
description: Sample layout for API docs manually in markdown.  Do not publish this file in TOC.
author: Tim Knoll
topic-type: NA
date: 03/16/2023
---

# API Title

API description

## API Resource (ex api-clients)

### API Method 1 (ex GET <API URL>)

Method description

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

| Field                  | Value                                                                                                                                                                                                                                                                                                                                   |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-sample-call-endpoint | "https://amber.com:443/management/v1/services/c477f41c-9b38-49ef-b272-810eecd6256d/api-clients"                                                                                                                                                                                                                                         |
| x-sample-call-output   | "[\n {\n \"id\": \"8a41171b-9723-4cf8-afb6-1402fa137913\",\n \"service_id\": \"eb7c16c2-a985-4e99-83c9-9fe3a730252f\",\n \"product_id\": \"412bb147-5634-4613-96ad-3a134810bb68\",\n \"product_name\": \"Basic\",\n \"status\": \"Active\",\n \"name\": \"SGX Basic\",\n \"created_at\": \"2022-08-26T10:12:19.162087+05:30\"\n }\n]\n" |