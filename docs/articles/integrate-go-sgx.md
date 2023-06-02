---
title: Project Amber SGX Go Client
description: Provides an overview of the Project Amber G-SGX adapter.
author: grminch
topic: integration
date: 02/06/2023
---

# Project Amber Go-SGX adapter

This library uses Intel® SGX DCAP (Data Center Attestation Primitives) for quote generation. For more information and DCAP installation instructions, see [Intel® SGX driver in the kernel](concept-intel-sgx.md#intel-sgx-driver-in-the-kernel). You can download DCAP and the source code from Github at [SGX Data Center Attestation Primitives](https://github.com/intel/SGXDataCenterAttestationPrimitives).

> [!NOTE]
> The Project Amber Go-SGX adapter requires Go version 1.17 or newer.

## Installation

Install the latest version of the library with the following commands:

```sh
go get github.com/intel/amber-client/go-sgx
```

## NewAdapter

The Go-SGX adapter exposes one method, **NewAdapter**. The following code snipped creates a new Go SGX adapter, then use the adapter to collect quote from Intel® SGX enabled platform.

```go
import "github.com/intel/amber-client/go-sgx"

adapter, err := sgx.NewAdapter(enclaveId, enclaveHeldData, unsafe.Pointer(C.enclave_create_report))

evidence, err := adapter.CollectEvidence(nonce)
if err != nil {
    return err
}
```

For a longer example, see the [code sample](integrate-go-client.md#code-example) in the Go Client topic.

