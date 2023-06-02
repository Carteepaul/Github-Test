---
title: Project Amber MAA Adapter Service
description: Describes the MAA adapter service functionality and how to use it.
author: grminch
date: 02/20/2023
service: maa-adapter
---

# Project Amber MAA adapter service

## Overview

The Project Amber MAA (Microsoft Azure Attestation) adapter service allows you to seamlessly switch to using Project Amber attestation instead of Azure attestation. The MAA adapter service offers the same set of APIs as Azure attestation to fully accommodate Azure attestation requests.

The Project Amber MAA adaptor replicates the same API endpoints, parameters, request and response schema, and behaves just like the Azure Attestation service. The MAA client/workload submits the exact same attestation request to the Project Amber service instead of Azure Attestation. Then the MAA workload or relying party retrieves and uses the Project Amber signing certificates to verify the token instead of Azure Attestation certificates. The rest of the MAA client/workload logic remains unchanged. Existing Azure Attestation workloads can be migrated with only minor changes, as described below in [MAA workload migration](#maa-workload-migration).

The following table is a brief comparison of Project Amber vs. Azure Attestation.

| | Project Amber | Azure Attestation |
|:---|:---|:---|
| URL instance  | Unified instance URL  | Regional shared provider <br> Tenant custom provider |
| API endpoints | Unified API endpoint for all TEE attestation types. | Separate endpoints for each attestation type. |
| Request/response and token schemas | Project Amber-specific  | Azure Attestation-specific |
| Policies | Tenant can define multiple policies. Uses OPA policy engine and Rego policy language. | One policy per attestation type; uses Azure policy grammar. |
| Verifier logic | Quote verification + policies + nonce & user_data verification. | Quote verification + policy + runtime data verification. |
| Authentication | API key | Azure identity |
| Client SDK | C | C#, Java, Python, and others.|

## MAA adapter workflow

1. The Azure client sends the evidence along with the Project Amber API key as a query parameter in the URL.
1. An inter-service JWT based on the API key is generated. The inter-service JWT contains information such as Project Amber tenant ID, subscription ID, etc.
1. The MAA adapter service transforms the Azure Attestation request to a Project Amber appraisal request.
1. The intermediate steps are the typical Project Amber attestation flow for quote and policies verification.
1. The MAA adapter service converts the Project Amber token to an Azure Attestation token and response.
1. The MAA adapter service sends the response back to the Azure client.

## MAA workload migration

This section describes how to migrate an existing Azure Attestation workload to use Project Amber attestation. 

### Policy migration

The Azure attestation policy must be manually migrated to Project Amber. This section describes how to map and migrate the Azure policy to the equivalent Project Amber policies.

#### Incoming claims

| MAA incoming claim | Project Amber incoming claim |
| :--- | :--- |
| `x-ms-tee-is-debuggable` | `amber_tee_is_debuggable` |
| `x-ms-sgx-product-id` | `amber_sgx_isvprodid` |
| `x-ms-sgx-mrsigner` | `amber_sgx_mrsigner` |
| `x-ms-sgx-mrenclave` | `amber_sgx_mrenclave` |
| `x-ms-sgx-svn` | `amber_sgx_isvsvn` |

#### Policy mapping

| MAA policy rule | Project Amber policy type |
| :--- | :--- |
| version | N/A |
| authorizationrules | Appraisal policy |
| issuancerules | Token customization policy |

#### Sample policy mapping

The following policy is an Azure Attestation policy. 

```
version= 1.0;
authorizationrules{
[ type=="x-ms-tee-is-debuggable", value==false ] && [ type=="x-ms-sgx-svn", value>=1 ] => permit();
};
issuancerules{
c:[type=="x-ms-tee-is-debuggable"] => issue(type="is-debuggable", value=c.value);
c:[type=="x-ms-sgx-mrsigner"] => issue(type="sgx-mrsigner", value=c.value);
c:[type=="x-ms-sgx-mrenclave"] => issue(type="sgx-mrenclave", value=c.value);
c:[type=="x-ms-sgx-product-id"] => issue(type="product-id", value=c.value);
c:[type=="x-ms-sgx-svn"] => issue(type="svn", value=c.value);
};
```
The Azure policy can be mapped to the following Project Amber attestation policies.

**Project Amber appraisal policy**

```
default matches_sgx_policy = false
matches_sgx_policy = true {
    input.amber_tee_is_debuggable == false
    input.amber_sgx_isvsvn >= 1
}
```

**Project Amber token customization policy**

```
get_token_fields[token_fields] {
    token_fields := {
        "svn": input.amber_sgx_isvsvn,
        "sgx-mrenclave": input.amber_sgx_mrenclave,
        "sgx-mrsigner": input.amber_sgx_mrsigner,
        "product-id": input.amber_sgx_isvprodid,
        "is-debuggable": input.amber_tee_is_debuggable,
    }
}

```

### Workload modification

The Project Amber MAA adapter is designed to be very easy to use, with few or no code changes. In many cases, all that is needed is a configuration change. The attestation URI will change, and the token validation option may change.

#### Attestation URI

Project Amber replicates the Azure Attestation APIs under the base URI `{{amberBaseURI}}/maa`. The Azure workload must be changed to use the Project Amber attestation URI, for example, `amber.com/maa`.

#### Attestation token validation

If you are using the Azure Attestation `AttestationClient` class to help you verify that the attestation token is issued, you will need to change the `issuer` to Project Amber, for example, `https://amber.com`.

##### Token issuer verification
```
string expectedIssuer = "https://amber.com";

// Send to service for attestation
var options = new AttestationClientOptions(tokenOptions: new AttestationTokenValidationOptions
    {
    ExpectedIssuer = expectedIssuer,
    ValidateIssuer = true,
    }
)
```



