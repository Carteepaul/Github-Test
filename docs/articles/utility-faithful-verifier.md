---
title: Faithful Verifier
description: An overview of Project Amber's Faithful Verifier.
author:
topic: conceptual
date: 12/06/2022
---

# Faithful Verifier

The Faithful Verifier (FV) is a Linux command-line utility that can be used to verify the fidelity of a Project Amber token.  

> [!NOTE]
> The Faithful Verifier tool can be downloaded from the Project Amber downloads page. This feature is supported only with Amber Premium subscription.

## Faithful verification

A token is "faithful" when it is generated by authentic Project Amber services in an attested trusted execution enviroment (TEE). When Project Amber microservices that are involved in token generation start, they initialize TEE environments to contain attestation signing assets and other secrets. All token generation processing occurs in these TEEs, and protected certificates and keys are released only to microservice instances that meet TEE quoting requirements. In this way, the attestation information for each of these microservices is stored in a Faithful Verification ledger and referenced by every Amber token generated.

These ledger references in an Amber token can be used to validate the specific microservice instances that produced that token and retrieve their TEE attestation information. The FV can both retrieve this attestation report, and independently validate the TEE quotes. In this way an Amber user can audit the secure processing of Amber tokens.

> [!NOTE]
> All Project Amber tokens have an expiration time, and the FV process must be completed for a given Amber token before it expires. This feature is intended for use with recently-created tokens, not historical audits.

## Downloading the Faithful Verifier

The Faithful Verifier is a downloadable executable found on the Project Amber Downloads page.

## Operating system support

The Faithful Verifier is supported on Ubuntu 20.04.

## Prerequisites

The Faithful Verifier requires Intel® SGX DCAP (Data Center Attestation Primitives) for quote verification. This means that DCAP must be installed on the system where the Faithful Verifier will run, and the system must also have access to the Intel® SGX PCCS (Provisioning Certificate Caching Service) for SGX collaterals.

### Installing Intel® SGX DCAP libraries

Add the Intel® SGX repository and install the DCAP library packages:

```bash
echo 'deb [arch=amd64] https://download.01.org/intel-sgx/sgx_repo/ubuntu focal main' > /etc/apt/sources.list.d/intel-sgx.list
wget -qO - https://download.01.org/intel-sgx/sgx_repo/ubuntu/intel-sgx-deb.key | apt-key add -
apt-get update
apt-get install -y libsgx-urts libsgx-dcap-quote-verify-dev libsgx-dcap-default-qpl
```
Edit `/etc/sgx_default_qcnl.conf` (pointing DCAP to use PCCS at https://api.trustedservices.intel.com/sgx/certification/v3/)

```json
{
  // *** ATTENTION : This file is in JSON format so the keys are case sensitive. Don't change them.
 
  //PCCS server address
  "pccs_url": "https://api.trustedservices.intel.com/sgx/certification/v3/",
 
  // To accept insecure HTTPS certificate, set this option to false
  "use_secure_cert": true,
 
...
```

To check that the system can connect to PCCS, use the following command:

```bash
curl -v -X GET "https://api.trustedservices.intel.com/sgx/certification/v3/qe/identity
```

If this command returns a 200 OK with PCS indentity content, DCAP should be able to connect.


## Verifying an attestation token

The Faithful Verifier requires a Project Amber attestation token to begin.  Save the token to a file (for example, `token.jwt`) on the system where the Faithful Verifier will run.


Run the Faithful Verifier "verify" function, providing a Project Amber API key for a tenant with a premium subscription to TEE attestation, and the attestation token:

```bash
./faithful-verifier verify --api-key $APIKEY --attestation-token token.jwt --tls-verify -o faithfulservices.json -r fvreport.json
```

The Faithful Verifier will retrieve and save the token's faithfulness report to `fvreport.json`.

The Faithful Verifier will then verify the facts asserted by the faithfulness report. It does this by independently validating the Intel® SGX quotes for each of the quoted services using the Intel® SGX DCAP libraries and the Intel® SGX PCCS server—validating the quote itself, without using any Project Amber attestation resources. In this way the "faithfulness" of the Amber token can be verified by a third party without relying only on Project Amber's assertion. The result will be saved to `faithfulservices.json`.

> [!NOTE]
> This function depends on the DCAP libraries communicating to the public Intel® PCS server to attest the Intel® SGX quotes from the faithfulness report.  If this communication is blocked (by a proxy configuration, etc) the FV may appear to "hang" until manually stopped.  

Sample output:
```bash
Verification passed, results saved at: /<path>/faithfulservices.json
```
The content of the verification output will include comparisons of each faithful service against the permitlist as well as an independent verification that the service Intel® SGX quotes are valid.  If both of these facts are true, then the service will be reported as "faithful."

```json
{
        "052335ca-7c5c-4dbc-b373-d0f7774392f6": [
                "Service record quote for appraisal-service with id: 052335ca-7c5c-4dbc-b373-d0f7774392f6 is valid",
                "appraisal-service with id: 052335ca-7c5c-4dbc-b373-d0f7774392f6 passed verification against permitlist",
                "appraisal-service with id: 052335ca-7c5c-4dbc-b373-d0f7774392f6 is faithful"
        ],
        "cb8452ac-9f7f-4212-a061-f93f3aad9281": [
                "Service record quote for quote-verification-service with id: cb8452ac-9f7f-4212-a061-f93f3aad9281 is valid",
                "quote-verification-service with id: cb8452ac-9f7f-4212-a061-f93f3aad9281 passed verification against permitlist",
                "quote-verification-service with id: cb8452ac-9f7f-4212-a061-f93f3aad9281 is faithful"
        ],
        "ceb6e9a3-b555-47e9-b92c-034fb6005429": [
                "Service record quote for tee-caching-service with id: ceb6e9a3-b555-47e9-b92c-034fb6005429 is valid",
                "tee-caching-service with id: ceb6e9a3-b555-47e9-b92c-034fb6005429 passed verification against permitlist",
                "tee-caching-service with id: ceb6e9a3-b555-47e9-b92c-034fb6005429 is faithful"
        ]
}
```

Sample content of the faithfulness report:

```json
{
	"report": {
		"service_records": {
			"052335ca-7c5c-4dbc-b373-d0f7774392f6": {
				"service_id": "052335ca-7c5c-4dbc-b373-d0f7774392f6",
				"name": "appraisal-service",
				"quote": "<SGX quote>",
				"registration_date": "2023-02-02T16:19:17.6587Z",
				"permitlist_version": "v0.3.0-66f6bcd",
				"fmspc": "00606A000000",
				"ca": "platform"
			},
			"cb8452ac-9f7f-4212-a061-f93f3aad9281": {
				"service_id": "cb8452ac-9f7f-4212-a061-f93f3aad9281",
				"name": "quote-verification-service",
				"quote": "<SGX quote>",
				"registration_date": "2023-02-02T16:18:10.743747Z",
				"permitlist_version": "v0.3.0-66f6bcd",
				"fmspc": "00606A000000",
				"ca": "platform"
			},
			"ceb6e9a3-b555-47e9-b92c-034fb6005429": {
				"service_id": "ceb6e9a3-b555-47e9-b92c-034fb6005429",
				"name": "tee-caching-service",
				"quote": "<SGX quote>",
				"registration_date": "2023-02-02T16:18:12.925546Z",
				"permitlist_version": "v0.3.0-66f6bcd",
				"fmspc": "00606A000000",
				"ca": "platform"
			}
		},
		"permit_lists": [
			{
				"version": "v0.3.0-66f6bcd",
				"measurements": {
					"appraisal-service": {
						"mrenclave": "A2Ew...",
						"mrsigner": "Y9q...",
						"semver": "v0.5.3-b3027a6-5132d8f"
					},
					"fv-controller": {
						"mrenclave": "xh68...",
						"mrsigner": "Y9qJ...",
						"semver": "v0.3.2-f5161aa-29c7eed"
					},
					"maa-adaptor-service": {
						"mrenclave": "fDT...",
						"mrsigner": "Y9qJ...",
						"semver": "v0.1.3-f5161aa-8b6276b"
					},
					"policy-provisioner": {
						"mrenclave": "PESo...",
						"mrsigner": "Y9qJ...",
						"semver": "v0.4.2-f5161aa-8b6276b"
					},
					"quote-verification-service": {
						"mrenclave": "Hd+9...",
						"mrsigner": "Y9qJ...",
						"semver": "v0.5.2-f5161aa-8b6276b"
					},
					"tee-caching-service": {
						"mrenclave": "zLI2...",
						"mrsigner": "Y9qJ...",
						"semver": "v0.5.2-f5161aa-8b6276b"
					}
				}
			}
		],
		"date": "2023-02-07T19:50:16.40252Z",
		"nonce": "<nonce value>",
		"sgx_collaterals": [
			{
				"collection_date": "2023-01-10T21:08:02.835329Z",
				"fmspc": "00606A000000",
				"ca": "platform",
				"sgx_collaterals": {
					"pck_crl_issuer_chain": "<pck cert info>",
					"root_ca_crl": "<root ca info>",
					"pck_crl": "<pck_crl>",
					"tcb_info_issuer_chain": "<TCB issuer cert info>",
					"tcb_info": "<TCB info>",
					"qe_identify_issuer_chain": "<Quoting enclave certificate info>",
					"qe_identity": "<Quoting enclave identity>"
				}
			}
		]
	},
	"user_data": "<SGX user_data for the service that issues FV reports>",
	"report_quote": "<SGX quote for the service that issues FV reports>"
}
```

## Additional CLI details

```bash
Faithful Verifier CLI is used for getting and verifying amber faithful services

Usage:
  faithful-verifier [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  help        Help about any command
  verify      Verifies the faithful verification report

Flags:
  -h, --help   help for faithful-verifier

Use "faithful-verifier [command] --help" for more information about a command.
```