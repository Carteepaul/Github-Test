---
title: Attestation policies
description: An overview of attestation policies in Project Amber.
author:
topic: conceptual
date: 04/20/2023
---

# Policies

Attestation policies are used to process attestation quotes and determine if Project Amber will issue an attestation token. Policies use the [Open Policy Agent (OPA)](https://www.openpolicyagent.org/docs/latest/policy-language/#what-is-rego) Rego standard for rule definitions. Separate policy definitions address the evidence evaluation and the content of any resulting attestation token. Multiple policies can be evaluated in a single attestation request. The policies used in a given attestation can be defined by their associations with the API Key used in the request and can also be specified in the API call request body to request attestation.  

Policy definitions can evaluate claim elements contained in  the evidence provided as part of an attestation request. Different security features will provide different claims and have correspondingly different policy options. Future feature support will enable policy definitions for claims elements related to those new features.

## Default policies

Project Amber includes default appraisal and token policies used when no other policies are specified, either by association with the API key or in the appraisal request. The default appraisal policy evaluates the TEE quote evidence and ensures that the enclave is genuine, but does not evaluate enclave-specific elements like the `MRENCLAVE` value of an Intel® SGX enclave. The token will contain these elements, and they can be evaluated by a relying party. This means that, if a relying party needs to verify the identity and/or authenticity of an enclave, the default policy is sufficient.

### Sample attestation token using th default policy and Intel® SGX

```json
{
  "amber_trust_score": 10,
  "amber_report_data": "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  "amber_tee_held_data": "AQ...SxlA==",
  "amber_sgx_mrenclave": "00000000000000000000000000000000000000000000000000000000000000000000",
  "amber_tee_is_debuggable": false,
  "amber_sgx_mrsigner": "00000000000000000000000000000000000000000000000000000000000000000000",
  "amber_sgx_isvprodid": 0,
  "amber_sgx_isvsvn": 0,
  "amber_unmatched_policy_ids": [
    "00000000-00000000-00000000-00000000",
    "00000000-00000000-00000000-00000000",
    "00000000-00000000-00000000-00000000",
    "00000000-00000000-00000000-00000000"
  ],
  "amber-faithful-service-ids": [
    "00000000-00000000-00000000-00000000",
    "00000000-00000000-00000000-00000000"
  ],
  "amber_tcb_status": "OK",
  "amber_evidence_type": "SGX",
  "amber_signed_nonce": true,
  "amber_custom_policy": {},
  "ver": "1.0",
  "exp": 1666287698,
  "jti": "00000000-00000000-00000000-00000000",
  "iat": 1666287368,
  "iss": "AS Attestation Token Issuer"
}
```

For more information about the elements of an attestation token, see the [Attestation](concept-attestation-overview.md) article.

## Claim elements

The tables below detail the allowed claim elements that can be included in a policy. Different attestation solutions include different claim elements.

### Intel® SGX claims

The table below shows the acceptable claim elements that can be used for Intel® SGX Appraisal or token customization policies.

| Claim                      | Datatype | Description                                                                                                                                                                                                                                                                                   |
| -------------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| amber\_sgx\_mrsigner       | string   | MR signer — Hash of the enclave signing key.                                                                                                                                                                                                                                                  |
| amber\_sgx\_mrenclave      | string   | MR enclave — Hash of the enclave measurement.                                                                                                                                                                                                                                                     |
| amber\_sgx\_isvsvn         | int      | ISV SVN — Security Version of the enclave.                                                                                                                                                                                                                                                    |
| amber\_sgx\_isvprodid      | int      | ISV Product ID — Enclave Product ID.<br><br>The ISV should configure a unique `ISVProdID` for each product that may want to share sealed data between enclaves signed with a specific `MRSIGNER`. The ISV may want to supply different data to identical enclaves signed for different products. |
| amber\_tee\_is\_debuggable | bool     | Flag denoting if the TEE is debuggable (Recommended to always be "false")      

**IMPORTANT** Data provisioned to a debug enclave is not secret. A debug enclave’s memory is not protected by the hardware, and it can be inspected and modified using the Intel® SGX debugging instructions. The enclave attributes, which include the debug flag, are contained in the report and quote that provide the enclave credentials. To protect all the secrets provisioned to production enclaves, local and remote entities must check the enclave attributes and exchange special debug secrets during the development process but refrain from provisioning any secret to a debug enclave. 

### Intel® TDX claim elements

| Claim                    | Datatype | Description                                                                                                                                                                                                                |
| ------------------------ | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| amber\_tdx\_seamsvn      | int      | Seam SVN — SEAM Security Version Number — Version number that indicates when security-relevant updates have occurred. A new revision can be released without incrementing the SVN if no security-related changes are made. |
| amber\_tdx\_rtmr0        | string   | RTMR 0 — Array of NUM\_RTMRS<br><br>(TDX1: 4) runtime extendable measurement registers                                                                                                                                     |
| amber\_tdx\_rtmr1        | string   | RTMR 1 — Array of NUM\_RTMRS<br><br>(TDX1: 4) runtime extendable measurement registers                                                                                                                                     |
| amber\_tdx\_rtmr2        | string   | RTMR 2 — Array of NUM\_RTMRS<br><br>(TDX1: 4) runtime extendable measurement registers                                                                                                                                     |
| amber\_tdx\_rtmr3        | string   | RTMR 3 — Array of NUM\_RTMRS<br><br>(TDX1: 4) runtime extendable measurement registers                                                                                                                                     |
| amber\_tdx\_mrtd         | string   | MRTD — Measurement of the initial contents of the TD                                                                                                                                                                       |
| amber\_tdx\_mrsignerseam | string   | MR Signer Seam — Measurement of SEAM module signer. (Not populated for Intel SEAM modules)<br><br>This field is optional and only populated on TDX TCBINFOs.                                                               |
| amber\_tdx\_mrseam       | string   | MR Seam — Measurement of the SEAM module. Secure Arbitration Mode (SEAM) is an extension to Virtual Machines Extension (VMX) architecture to define a new VMX root mode called SEAM root.                                 |

## Appraisal policy

An appraisal policy consists of OPA-compliant rules based on claims that will be evaluated for attestation (in this example, an Intel® SGX quote). Multiple appraisal policies can be used in a single attestation, based on association [API keys](howto-manage-api-keys.md) and by optional inclusion of specific policy IDs in an [appraisal request](~/restapi/restapi-attestation.md). When an attestation meets the requirements of a policy, the resulting token includes that policy ID in the `matched_policies` array. If the evidence doesn't match the requirements of a policy, that policy is included in the `unmatched_policies` array in the token.

This sample appraisal policy requires the attestation evidence to include specific `mrsigner` and `mrenclave` values.  

```json
{
"policy_name": "<Policy name>",
"policy_type": "Appraisal policy",
"service_offer_id": "<service offer ID>",
"service_offer_name": "SGX Attestation",
"policy": "default matches_sgx_policy = false \n\n\n matches_sgx_policy = true {   \n input.amber_sgx_mrenclave == \"<mrenclave value>\" \n input.amber_sgx_mrsigner == \"<mrsigner value>\" \n input.amber_tee_is_debuggable == false } " 
}
```

Intel® SGX appraisal policy example:

```json
{
  input.amber_sgx_mrenclave == "1234e819861adef61232a4865efea9337b91ed301233491b17f123d9e8212311"
  input.amber_sgx_mrsigner == "123719e77d123a1470f612362a4d774303c899db69123f9c70ee123c08123123"
  input.amber_sgx_is_debuggable == false 
}
```

## Token customization policy

A token customization policy enables users to alter the content included in attestation tokens. This policy is not part of the evaluation for whether to issue an attestation token; those rules are defined in the appraisal policy.  

```json
{    
   "policy_name": "<Policy name>",
   "policy_type": "Token customization policy",
   "service_offer_name": "SGX Attestation",
   "service_offer_id": "<Service offer ID>",
   "policy": "get_token_fields[token_fields] { \ntoken_fields := {   \n \"my_claim_isvsvn\": input.amber_sgx_isvsvn, \n \"my_claim_mrenclave\": input.amber_sgx_mrenclave, \n \"my_claim_mrsigner\": input.amber_sgx_mrsigner, \n \"my_claim_isTeeDebuggable\": input.amber_tee_is_debuggable \n} } "  
}
```

Token customization policy example:

```JSON
{
  token_fields := 
    {
       "my_claim_isvsvn": input.amber_sgx_isvsvn, 
       "my_claim_mrenclave": input.amber_sgx_mrenclave, 
       "my_claim_mrsigner": input.amber_sgx_mrsigner, 
       "my_claim_isTeeDebuggable": input.amber_tee_is_debuggable
     }
}
```

## Using policies with associated API keys

Appraisal and token customization policies can be associated with one or more [API keys](howto-manage-api-keys.md). This allows a Project Amber user to define the attestation behavior for their applications. Policies can be managed on the API key object as needed, and no changes are required on the application itself.

## Attesting using specified policies

Appraisal and token customization policies can also be specified during an attestation request as part of the [Attestation API](~/restapi/restapi-attestation.md).  

### Sample /appraisal/v1/appraise request with specified policies

```json
{
    "quote": "AwACA...Qo=",
    "nonce": {
        "val": "N0...PQ==",
        "iat": "12...RD",
        "signature": "vW...Am"
    },
    "policy_ids": ["11111111-1111-1111-1111-111111111111", "22222222-2222-2222-2222-222222222222"],
    "user_data": "AQ...lA=="
}'
```

This request will evaluate the quote evidence using the two policies specified, _as well as_ any policies associated with the API key used to authenticate the request.