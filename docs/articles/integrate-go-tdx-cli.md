---
title: Intel TDX CLI
description: Provides an overview of Project Amber TDX CLI.
author: grminch
topic: integration
date: 04/25/2023
uid: integrate.tdx.cli
---

# Project Amber CLI for Intel® TDX

Project Amber includes a [Go language][golang] module for Intel® Trust Domain Extensions (Intel® TDX) that exposes an Intel TDX CLI for Linux systems.

## Prerequisites
 
- [Intel® Software Guard Extensions Data Center Attestation Primitives (Intel® SGX DCAP)][sgx-dcap-installation]
- Ubuntu 18.04 ("Bionic Beaver"), 20.04 ("Focal Fossa"), or 22.04 ("Jammy Jellyfish"). Installation packages vary depending on which version you're using. 
- Red Hat Enterprise Linux (RHEL) 8.0 or newer.
- [Go 1.17 or newer][go-download]
- _libtdx-attest-dev_. This is the Project Amber Intel TDX CLI package.

For detailed installation and build instructions, see the [Intel TDX CLI Readme][tdxcli_repo_readme] in the GitHub [**intel/amber-client**][amber_client_repo] repo. 

## CLI commands

The following CLI commands are exposed by **`amber-cli`**. These commands can be used from the command line or in custom applications. The [code sample](#code-sample) at the bottom of this article shows how to call the TDX CLI from a Go module.

**In this section**  
[token](#token): Used for fetching an attestation token from Project Amber.  
[quote](#quote): Used for fetching a quote from the TD.  
[decrypt](#decrypt): Used for decrypting an encrypted blob, using the private key passed as input.  
[create-key-pair](#create-key-pair): Used for creating an RSA3k key pair.

### token 

The `token` command is used for fetching an attestation token from Project Amber. `AMBER_URL` and `AMBER_API_KEY` must be set in the environment before calling this command. The token command also accepts optional policy IDs as input for verification.

`amber-cli token --policy-ids "8500e95a-58da-4b8a-a585-274730576d31,3910b351-5865-4fb2-a731-17c6793cd4ca,c3fc94cc-ed57-42a6-a433-1c648dbeabda"`

### quote 

The `quote` command is used for fetching the quote from TD. The `quote` command takes `nonce` and `user-data` as input which are hashed during TD report creation. Both the `nonce` and `user-data` are optional and must be provided as base64 encoded strings.

`amber-cli quote --nonce "dGVzdHVzZXJkYXRh" --user-data "dGVzdHVzZXJkYXRh"`

### decrypt 

The `decrypt` command is used for decrypting the encrypted blob using the private key passed as input. The `decrypt` command takes private key location and encrypted blob as input and writes the decrypted data either to stdout or to output file if passed as argument. The encrypted blob must be passed as base64 encoded string.

`amber-cli decrypt --key-path private-key.pem --in "dGVzdHVzZXJkYXRh" --out data.dec`

### create-key-pair

The `create-key-pair` command is used for creating RSA3k key pair. The private part of the key is written to an output file passed as an argument, and the public part is written to a `public-key.pem` file in current directory.

`amber-cli create-key-pair --private-key-path private-key.pem`

## Code sample

The following code sample shows how to request an attestation token from an Intel TDX trust domain.

```go
/*
 *   Copyright (c) 2023 Intel Corporation
 *   All rights reserved.
 *   SPDX-License-Identifier: BSD-3-Clause
 */
package main

import (
	"crypto/tls"
	"flag"
	"fmt"
	"os"
	"os/exec"

	"github.com/intel/amber/v1/client"
	"github.com/pkg/errors"
)

func main() {
	var policyId string
	cfg := client.Config{
		TlsCfg: &tls.Config{
			InsecureSkipVerify: true,
		},
	}

	flag.StringVar(&cfg.Url, "url", "", "URL of Amber SaaS")
	flag.StringVar(&cfg.ApiKey, "key", "", "Api Key for Amber")
	flag.StringVar(&policyId, "pid", "", "Policy id for Amber verification")
	flag.Parse()

	client, err := client.New(&cfg)
	if err != nil {
		panic(err)
	}

	// optional: either caller can provide existing public key or create new using amber-cli create-key-pair command
	pubPath := "pub.pem"
	_, err = createRSAKeypair(pubPath)
	if err != nil {
		panic(err)
	}

	// amber-cli looks for Amber API URL and Amber API Key in environment
	os.Setenv("AMBER_URL", cfg.Url)
	os.Setenv("AMBER_API_KEY", cfg.ApiKey)

	// pubPath: public key to be used as reportdata for quote generation
	out, err := exec.Command("amber-cli", "token", "--policy-ids", policyId, "-f", pubPath).Output()
	if err != nil {
		panic(err)
	}
	token := out[:]

	fmt.Printf("TOKEN: %s\n", string(token))

	// it is best practice to always clear secrets from environment after use
	os.Unsetenv("AMBER_URL")
	os.Unsetenv("AMBER_API_KEY")

	parsedToken, err := client.VerifyToken(string(token))
	if err != nil {
		panic(err)
	}

	fmt.Printf("CLAIMS: %+v\n", parsedToken.Claims)
}

func createRSAKeypair(pubPath string) ([]byte, error) {

	out, err := exec.Command("amber-cli", "create-key-pair", "-f", pubPath).Output()
	if err != nil {
		return nil, errors.Wrap(err, "Failed to execute amber-cli create-key-pair command")
	}
	return out[:], nil
}
```

<!-- External URLs -->
[sgx-dcap-installation]: https://www.intel.com/content/www/us/en/developer/articles/guide/intel-software-guard-extensions-data-center-attestation-primitives-quick-install-guide.html
[amber-tdx-cli]: https://github.com/intel/amber-client/tree/main/amber-cli-tdx
[go-download]: https://go.dev/dl/
[golang]: https://go.dev/
[amber_client_repo]: https://github.com/intel/amber-client
[tdxcli_repo_readme]: https://github.com/intel/amber-client/tree/main/amber-cli-tdx#intel-project-amber-go-tdx-cli

