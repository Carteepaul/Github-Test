---
title: Glossary
description: Contains definitions for terms used in the documentation.
author: Some Guy
topic: conceptual
date: 05/18/2023
uid: concept.glossary
---

# Project Amber Glossary

The following terms are used in the Project Amber documentation. Terminology related to the attestation process (attester, endorser, verifier, etc) is derived from [IETF RFC 9334](https://datatracker.ietf.org/doc/rfc9334/), Remote ATestation procedureS (RATS). Terms specific to Project Amber are prefixed with "Project Amber," for example, "Project Amber admin API key." Other terms are industry standard. Terms marked with an asterisk ( * ) may be names and/or brands that are claimed as the property of entities other than Intel.

#### Admin API key
A Project Amber _admin API key_ (also called a _management API key_) is required to manage Project Amber products, service offers, users, policies, and tags. An admin API key allows the bearer to manage a tenant's entire Project Amber subscription. 

> [!WARNING]
> Project Amber admin API keys are extremely sensitive values that must be safeguarded against unauthorized use. 

#### Attestation API key 
A Project Amber _attestation API key_ is required for all attestation-related functions, such as requesting a nonce, quote, or token; working with the Microsoft Azure Attestation (MAA) adapter, and using the Faithful Verification tool. An attestation API key can't be used to manage users, policies, tags, etc. 

#### Attestation appraisal policy
An _attestation appraisal policy_ is used by the relying party to evaluate claims in the attestation token. 

#### Attestation token
A Project Amber _attestation token_ is a JSON Web Token (JWT) that is generated as a result of verification. An attestation token is sometimes called an "attestation report," though that term is not used in this documentation. The JWT contains both incoming claims (evidence) and outgoing claims and is signed with a certificate that can be traced to Intel. Relying parties use attestation tokens to decide what, if any, confidential services or data to release to the attester.   

#### Attester
The _attester_ is an entity that provides evidence (a _quote_) to a verifier as to its integrity.

#### Attestation
_Attestation_ is the process by which cryptographically verifiable claims can be made by a trusted authority based on a set of evidence. For example, an Intel® Software Guard Extensions (Intel® SGX) enclave can generate a quote that can be sent to an attestation authority (such as Project Amber) to validate the integrity and identity of the quoted enclave.

#### Challenger
A _challenger_ is an entity that requests integrity metrics and evaluates the level of trust in the attester. When an attester is challenged, it may respond with either a quote or an attestation token, depending on the needs of the relying party.

#### Claim
Quotes and attestation tokens are composed of elements called _claims_. A claim usually assigns some value to a named element, often a cryptographic token that represents the signature of a component of the TCB. _Incoming claims_ are part of the quote that Intel SGX DCAP collects for the attester. _Outgoing claims_ are elements added to the attestation token by Project Amber. For more information, see [Attestation tokens](concept-attestation-overview.md#attestation-tokens).
  
#### Intel SGX DCAP
_Intel® Software Guard Extensions Data Center Attestation Primitives (Intel® SGX DCAP)_ is a software infrastructure that helps collect TEE evidence for a quote, which is sent to a verifier (such as Project Amber) for attestation.

#### ECDSA
_Elliptic Curve Digital Signature Algorithm (ECDSA)_ is a cryptographic algorithm. Project Amber uses ECDSA as an integral part of the attestation process. For more information, see [Intel Software Guard Extensions ECDSA - Attestation for Data Center Orientation Guide](https://download.01.org/intel-sgx/dcap-1.0.1/docs/Intel_SGX_DCAP_ECDSA_Orientation.pdf).

#### Endorser
An _endorser_ is an entity that endorses or substantiates evidence provided by the attester or verifier. For example, when the attester provides the key that identifies the Intel® SGX-enabled processor, that key can be verified, or endorsed, by comparing it to the original value provided by the Intel® SGX Provisioning Certification Service.   

#### Entity
An _entity_ is any component or actor with a role in the attestation process. Entities are usually logical, e.g. a service, complex logic, or human component, as opposed to a device such as a disk controller or router card.
  
#### Faithful Verification
The [Project Amber Faithful Verifier (FV) tool](concept-faithful-verifier.md) is a Linux command-line utility that is used to verify the fidelity of a Project Amber token. The Faithful Verifier tool can be downloaded from the Project Amber web interface downloads page.

#### GSC
_Gramine Shielded Containers (GSC)_* provide the infrastructure to deploy Docker* containers protected by Intel SGX enclaves using the Gramine Library OS*. For more information, see [Project Amber Gramine integration](concept-gramine-integration.md).

#### HSM
A _Hardware Security Module (HSM)_ is a highly secure, tamper-resistant, hardware-based component for storing and managing digital secrets. Project Amber uses an HSM to store reference values used during verification.

#### ISV SVN
The Intel SGX enclave author assigns a _Security Version Number (SVN)_ to each version of an enclave. The SVN reflects the level of the security property of the enclave, and should monotonically increase with improvements of the security property. After an enclave is successfully initialized, the CPU records the SVN, which can be used during attestation. 

#### ISV Product ID
The enclave author assigns a _product ID_ to each enclave. The product ID allows the enclave author to segment enclaves with the same enclave author identity. After an enclave is successfully initialized, the product ID is recorded by the CPU to be used during attestation.

#### KMS
A Key Management System (KMS) is a secure system for storing and managing keys and other digital secrets. Examples include SSH key management tools, an HMS, Azure Key Vault*, and AWS Key Management Service*.   

#### Nonce
A _nonce_ is an optional value that helps mitigate replay attacks and ensures the "freshness" of an attestation quote or token. A Project Amber nonce remains valid for 30 seconds.

#### OPA
Project Amber uses _Open Policy Agent (OPA)*_ as its policy evaluation engine. For more information about OPA, see the [OPA documentation](https://www.openpolicyagent.org/docs/latest/).

#### PCS
_Intel® Software Guard Extensions Provisioning Certification Service (Intel® SGX Provisioning Certification Service)_ is a  provisioning certificate caching service implemented in nodejs for reference. It retrieves PCK certificates and other collaterals on demand using the internet at runtime, and then caches them in local database.

#### Policy
A _policy_ is a collection of rules that are used to evaluate claims and define the requirements to issue an attestation token. An API key may have one or more policies associated with it.

#### Product
A Project Amber _product_ is a grouping of APIs used to define the services enabled for tenants.

#### Quote
A TEE _quote_ is evidence of trustworthiness of the enclave and is used during remote attestation. Different TEEs implement quoting mechanisms differently, but the quote represents the cryptographically verifiable evidence assessed by a remote attestation service. A quote is made up of a collection of claims.

#### Reference value
A _reference value_ is a value that claims can be compared against. Reference values are often cryptographic measurements of entities and devices. Reference values are stored in a secure repository such as an HSM or TPM.   

#### Relying party
The _relying party_ is any consumer of an attestation token, or to put it another way, the relying party is any entity that must trust the attester.  The relying party is so called because it relies on claims made in the attestation token created by the verifier to establish trust in the attester. Relying parties must have their own verifier or verification logic to parse and appraise a Project Amber attestation token.  .

#### Service Offer
A Project Amber _service offer_ is a collection of one or more products bundled together.

#### Intel SGX
_Intel® Software Guard Extensions (Intel® SGX)_ helps protect data in use via application isolation technology. By protecting selected code and data from modification, developers can partition their application into hardened enclaves or trusted execution modules to help increase application security. Intel SGX enclaves can be attested to prove their integrity and isolation from other system processes.

#### Tag
A Project Amber _tag_ is an optional key-value pair that can be associated with an API key to help collect reporting and metrics information.

#### TEE
A _Trusted Execution Environment (TEE)_ is a secured environment that protects the integrity and confidentiality of code and data during execution by isolating itself from the rest of the compute elements. Intel SGX is an example of a TEE. For more information about TEEs, see the [TEE Overview](concept-tees-overview.md).

#### Trust
_Trust_ is the assured reliance on the attributes and behaviors of a given entity.

#### TPM
A _Trusted Platform Module (TPM)_ is a microcontroller that provides secure, hardware-based cryptographic generation and storage functions. A TPM is often a separate chip, but it can be integrated as an independent device in a processor.  For more information, see the [TPM Library Specification](https://trustedcomputinggroup.org/resource/tpm-library-specification/).  

#### Verifier
A _verifier_ is an entity that evaluates evidence from an attester. 

<br><br>
---

**\*** Other names and brands may be claimed as the property of others.