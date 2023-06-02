---
title: Project Amber Faithful Verification Swagger doc
description: Faithful Verification external API definition
author: Tim Knoll
topic-type: NA
date: 03/16/2023
---

# Faithful Verification

The Faithful Verification provides the evidence to verify the integrity of the services involved in the process of attestation.
Evidence is created by verifying the quotes provided by services running in the TEE, during their registration process.
These measurements provided by individual services are validated against the permitlist configured.


## Token Audit Report

### POST /faithful-verification/v1/token-audit-report

Generates faithful verification token audit report. This report can be attested using the faithful verifier tool to verify
the integrity of the services involved in the attestation.

#### Parameters

| Parameters                     |                                                                          |
|--------------------------------|--------------------------------------------------------------------------|
|     policyName                 | Name of policy                                                           |
|     string($string) (query)    |                                                                          |
|                                |                                                                          |
|     attestationType            |     Type of Attestation [SGX Attestation/TDX   Attestation]              |
|     string (query)             |     Available values :   TDX Attestation, SGX Attestation                |
|                                |                                                                          |
|     policyType                 |     Type of a policy - [Appraisal \Token   customization]                |
|     string (query)             |     Available values :   Appraisal policy, Token customization policy    |
|                                |                                                                          |
|     Accept (required)          |     Accept header                                                        |
|     string (header)            |     Available values :   application/json                                |


#### Responses

Response content type: application/json

| Code | Description                                                    |
|------|----------------------------------------------------------------|
| 201  |         Policy collection matching search criteria returned    |
| 400  |         Valid contents for query parameters must be specified  |
| 403  |         RBAC access denied                                     |
| 415  |         Invalid Accept Header in Request                       |
| 500  |         Internal server error                                  |

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

| Field                  | Value                                                               |
|------------------------|---------------------------------------------------------------------|
| x-permissions          | "report:create"                                                     |
| x-sample-call-endpoint | "https://amber.com:443/faithful-verification/v1/token-audit-report" |

x-sample-call-input

```json
{
  "token": "eyJhbGciOiJQUzM4NCIsImtpZCI6ImM4Mjk4OWJhMTg0ZjQ2ZmYzNjNkMjNlZDk2MTJjMGFiMzg0OWM3MTIiLCJ0eXAiOiJKV1QifQ.eyJhbWJlci10b2tlbi1pZCI6IjliZDc1ZWNiLWU0NTItNGEyNS04NGEzLTVjMmM0NDM4Y2RjOSIsImFtYmVyLXRydXN0LXNjb3JlIjoxMCwiYW1iZXItc2d4LW1yZW5jbGF2ZSI6IjgzZjRlODE5ODYxYWRlZjZmZmIyYTQ4NjVlZmVhOTMzN2I5MWVkMzBmYTMzNDkxYjE3ZjBkNWQ5ZTgyMDQ0MTAiLCJhbWJlci1zZ3gtbXJzaWduZXIiOiI4M2Q3MTllNzdkZWFjYTE0NzBmNmJhZjYyYTRkNzc0MzAzYzg5OWRiNjkwMjBmOWM3MGVlMWRmYzA4YzdjZTllIiwiYW1iZXItc2d4LWlzdnByb2RpZCI6MCwiYW1iZXItc2d4LWlzdnN2biI6MCwiYW1iZXItc2d4LWVoZCI6IkFRQUJBREczNGdwQWlMN1Z5T01CMy9ZSmxuV2ZqNFRpMkU1NWtWOXlkYkcvcnU2bHpjS25acCtSakFqVy9TTUd0S0hhNi81M1FKTGIyQXRROWtrNVIwNENiTzZ3NTRheCsyYUlaQjMzaGVkR0xxUUViNE51TzVzWStXeHpLSTdsdkFvaElWWkt4MkFJZC9UcHg1R2ZZd3F6QmZQL3FiMkw1NXpJTDRYSkRXV2IrZTRsSlJoZlZ1c01pY0RlMWZpbHRZd3FzK2N1REw3YnhHcjRpSFh4KzdiKzhJSTJ6cFBoTVJyOXFnWWEvQlFLeGIzRzZDQ21VSHBROVVleHltTlRkVkc1MExOM1FyK1dlTVJGNGdRRWRNcWxZTSthSWZJWGYwdUFUMkFRRVd0L2loN3J0RHlLUU4yOTBnZ25hSE1Jd0JjRzRibm8vYUhEQkwxVnd1elZWT0wyc2ZORjZYZ2JhVTdwMTQweG9EeHIwdUc2ZVMrY3A0NXpXTHkzVzJUUWRwbjJocGVTT041emhtMlR4OHlaVVRjT2dWbUQwZDYrWU1hTkRFMms5cXcxeVBhTlVta2ZxWXU3bHZ4Uk5pamtYQ1JWSFhYZEgzOSt5NW11azBkblJRcWN5ZlJLZ2l6N1lDb2NkSHZpVlRJOGZ0RDdLdjh4M3BSQzltUVdhNDlWa29rcm1BPT0iLCJhbWJlci1tYXRjaGVkLXBvbGljeS1pZHMiOm51bGwsImFtYmVyLXVubWF0Y2hlZC1wb2xpY3ktaWRzIjpbIjJhZDQyZDQ0LWYyZWUtNDI4MC1iZmExLWFhNzRjNTQ4YjY5MiIsIjg0NzMxNGU3LWU0OWItNDg2Ni1hYzRiLTZiMzZlYTg1MDQ4ZiJdLCJhbWJlci1mYWl0aGZ1bC1zZXJ2aWNlLWlkcyI6WyI3OWM0YTAyNy1hOTFjLTQzODAtOGQ0Ny1iNzFiYTllZDRiN2IiLCI5NzQ5YzU2Ny1kMDU5LTQ0NDQtOGFjNy1mOGEyZDkxZDlmMjQiXSwiYW1iZXItdGNiLXN0YXR1cyI6Ik9LIiwiYW1iZXItZXZpZGVuY2UtdHlwZSI6IlREWCIsImFtYmVyLXNpZ25lZC1ub25jZSI6dHJ1ZSwidmVyIjoiMS4wIiwiZXhwIjoxNjYxNzcyNTg5LCJpYXQiOjE2NjE3NzIyNTksImlzcyI6IkFTIEF0dGVzdGF0aW9uIFRva2VuIElzc3VlciJ9.K8yloUn8zzKH3sCGj04Vs4IQeKkgT6ME6fLNVE6oePKnwwvgqu4Jc91OembZgG_iit2yJ8KTr92QPdUvrJgPtlmx7omLF9PauaqIkHeUDP6y9wgOZr1yotjqQOGrXPAz2GHhzMq-40KLMcbP0732lmn3ZH8XchQvgcczxG2rEacrVg1lu99jTV3hKN3psqZqsiKIYegJucF9ZpScvuY-TETTw2QH9ABV07SMmc4dhH4ysZ3FHT_-gio4EABgRWVEn6I2VMjvXYT0CXT68Fyjq2Cyg_KmZJMf0bWpRdlAqp3d7qR3vkUWcbyBIjqnGAmEnnQZENF7WLeDCnvSHVkLWHumF2x_SENF5hmP06S9Rs4xvZOOOot2fHhDPqCd6YNZH_7pZ0PtiiXlJAkuWp2euYmOyuGI4Fn44NuEoNH2BfK2sebd8zuoLNmxejnUF6TXhTzemxz3Qr0jmoPvV0ZzRvM9jwC56QINNcFOzQv2NM9P-LLzLjemsK4MWI1SxTo0","nonce": "test"
}
```

x-sample-call-output

```json
{
    "report": {
        "service_records": {
            "23f8ce22-0c41-4fe1-be0f-ca3b1b44bf68": {
                "service_id": "23f8ce22-0c41-4fe1-be0f-ca3b1b44bf68",
                "name": "tee-caching-service",
                "quote": "...",
                "registration_date": "2023-03-15T06:38:42.712292Z",
                "permitlist_version": "v1.0.0",
                "fmspc": "00606A000000",
                "ca": "platform"
            },
            "3466b22b-5053-4398-b9a1-6dd0d35a412f": {
                "service_id": "3466b22b-5053-4398-b9a1-6dd0d35a412f",
                "name": "quote-verification-service",
                "quote": "...",
                "registration_date": "2023-03-15T06:38:11.016599Z",
                "permitlist_version": "v1.0.0",
                "fmspc": "00606A000000",
                "ca": "platform"
            },
            "f41f574c-44d5-42f5-936d-48a8bc508520": {
                "service_id": "f41f574c-44d5-42f5-936d-48a8bc508520",
                "name": "appraisal-service",
                "quote": "...",
                "registration_date": "2023-03-15T06:38:21.31555Z",
                "permitlist_version": "v1.0.0",
                "fmspc": "00606A000000",
                "ca": "platform"
            }
        },
        "permit_lists": [
            {
                "version": "v1.0.0",
                "measurements": {
                    "appraisal-service": {
                        "mrenclave": "9L0...",
                        "mrsigner": "Y9q...",
                        "semver": "v1.0.0"
                    },
                    "fv-controller": {
                        "mrenclave": "gsI...",
                        "mrsigner": "Y9q...",
                        "semver": "v1.0.0"
                    },
                    "maa-adaptor-service": {
                        "mrenclave": "F2d...",
                        "mrsigner": "Y9q...",
                        "semver": "v1.0.0"
                    },
                    "policy-provisioner": {
                        "mrenclave": "0g5...",
                        "mrsigner": "Y9q...",
                        "semver": "v1.0.0"
                    },
                    "quote-verification-service": {
                        "mrenclave": "yUF...",
                        "mrsigner": "Y9q...",
                        "semver": "v1.0.0"
                    },
                    "tee-caching-service": {
                        "mrenclave": "tPG...",
                        "mrsigner": "Y9q...",
                        "semver": "v1.0.0"
                    }
                }
            }
        ],
        "date": "2023-03-15T20:52:50.595346Z",
        "nonce": "test",
        "sgx_collaterals": [
            {
                "collection_date": "2023-01-10T21:08:02.835329Z",
                "fmspc": "00606A000000",
                "ca": "platform",
                "sgx_collaterals": {
                    "pck_crl_issuer_chain": "...",
                    "root_ca_crl": "...",
                    "pck_crl": "...",
                    "tcb_info_issuer_chain": "...",
                    "tcb_info": "...",
                    "qe_identify_issuer_chain": "...."
                }
            }
        ]
    },
    "report_quote": "...",
    "user_data": "..."
}
```