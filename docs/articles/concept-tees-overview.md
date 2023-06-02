---
title: TEEs Overview
description: A brief introduction to Trusted Execution Environments (TEEs).
author: grminch
topic: conceptual
date: 04/12/2023
---

# Trusted Execution Environments (TEE)

A trusted execution environment (TEE) helps to protect user-executed code and data from modification by untrusted software, hardware, and system components that are outside the boundaries of the TEE. TEEs can provide a high level of protection against most software-based attacks and many hardware-based attacks, and give assurance that the software and hardware in the TEE have not been tampered with. 

Project Amber currently supports Intel® Software Guard Extensions (Intel® SGX) and Intel® Trust Domain Extension (Intel® TDX) TEEs. Support for more TEEs is planned for future releases of Project Amber. To understand how to use Project Amber services, you need a working knowledge of the underlying TEE technology. To learn more about Intel SGX and Intel TDX, see [Next Steps](#next-steps), below.

A TEE running directly on an Intel SGX-enabled platform is called an _enclave_. An Intel TDX virtual machine (VM) TEE running on an Intel SGX-enabled platform is called a _trust domain_.

Every TEE has a trusted computing base (TCB) that includes all the software, firmware, and hardware resources within the boundaries of the TEE. When a new enclave or trust domain is instantiated, the TEE's TCB must be verified (_attested_) before it can be trusted with sensitive workloads and data. For more information, see [Project Amber Attestation](concept-attestation-overview.md).

The attesting TEE collects cryptographic keys and platform collaterals, called a quote, as evidence to support its claim of authenticity. Evidence is collected by low-level attestation primitives (hardware drivers, essentially) for the TEE platform. For more information, see []

Project Amber provides several ways to interact with a TEE to facilitate migration of existing applications to Project Amber and simplify new application development. 

* Your TEE workload handles the low-level code to create a quote, and then calls the Project Amber REST API or TDX CLI (for Intel TDX trust domains only) for attestation services. This is most useful for migrating existing applications to Project Amber attestation.
* You can use the Project Amber Go client libraries to integrate Intel SGX or Intel TDX attestation into your application. The Project Amber client library handles the low-level calls to platform-specific attestation primitives for the TEE. The client library masks some of the complexity of working with TEEs, making it the preferred option for new development. Currently, only Go language is supported for integration. Support for more languages is planned for future releases of Project Amber.
* You can migrate existing Microsoft Azure Attestation (MAA) applications to use Project Amber attestation. In this case, your existing Microsoft Azure TEE application code is responsible for creating the quote in a format that is compatible with MAA. The Project Amber MAA Adaptor accepts quotes in MAA format for remote attestation by Project Amber.
* You can encapsulate existing application code in a Gramine container running in an Intel SGX enclave. This allows you to use existing applications with few modifications to the code. To facilitate attestation with Project Amber, a branch of the Gramine project includes a Project Amber client. 

## Next steps

Intel SGX and Intel TDX primary resources:

- [Intel SGX main page][sgx-overview]
- [Intel TDX main page][tdx-overview]

Working with Project Amber and TEEs, overview:
 - [Intel SGX Overview](concept-intel-sgx.md)
 - [Intel TDX Overview](concept-intel-tdx.md)
 
Project Amber TEE integrations:
- [Project Amber Go Client](integrate-go-client.md)
- [Go-SGX adapter](integrate-go-sgx.md)
- [Go-TDX adapter](integrate-go-tdx.md)
- [TDX CLI](integrate-go-tdx-cli.md)
- [MAA Adaptor](integrate-maa.md)
- [Gramine client](integrate-gramine.md)
 


<!-- External URLs -->
[sgx-overview]: https://www.intel.com/content/www/us/en/developer/tools/software-guard-extensions/overview.html
[tdx-overview]: https://www.intel.com/content/www/us/en/developer/articles/technical/intel-trust-domain-extensions.html

