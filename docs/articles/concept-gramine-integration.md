---
title: Gramine Integration
description: Understanding Gramine integration with Project Amber.
author:
topic: conceptual
date: 12/05/2022
---

# Gramine integration

Gramine is a library OS that provides a means to host application-facing code from the operating system kernel. Using a platform adaptation layer (PAL) in which to operate, Gramine supports unmodified applications that run inside an [Intel® SGX](concept-intel-sgx.md) enclave. This means there is minimal development needed to run the application from within a Gramine environment. Instead of having to rewrite code to run the application in a different environment, it encapsulates the code in a Gramine container in an Intel® SGX enclave. This makes it much easier for customers to adopt Intel® SGX without rewriting the core business code. 

The Gramine Library OS provides tool kits to help include customer workloads in Gramine Shielded Containers (GSC). A GSC protects a customer’s workload utilizing the secure hardware features enabled by Intel® SGX.  

To facilitate attestation of a GSC using Project Amber, a fork of the Gramine Project includes the Project Amber client. This enables applications to verify attestation evidence using Project Amber with minimal changes to the customer application.

## Gramine Project Amber architecture and pseudo-device files 

The Project Amber client is integrated as part of a forked Gramine project's library OS. 

### Gramine Project Amber client architecture

The Project Amber fork of Gramine provides a set of pseudo-device files a user application uses to trigger the attestation flow with the Project Amber service. The attestation flow is transparent to the user application. The user application reads the pseudo-device file `/dev/amber/token` to get an attestation token from Project Amber. In more detail, the following steps are performed:

1. The user application reads `dev/amber/token`.
1. The Project Amber client is triggered to collect an Intel® SGX quote and sends the quote to the Project Amber service.
1. Project Amber verifies the quote and returns the attestation token if it is successfully verified.
1. The Project Amber client provides the token to the user application using the pseudo-device file `/dev/amer/token`.
1. The user application sends the token to the relying party to request a protected resource, such as an API key. 
1. The relying party checks the validity of the token by... and returns the protect resource. 

### Gramine Project Amber client pseudo-device files 

The Project Amber client branch of Gramine exposes pseudo-device files that user applications can use to get status and configure endpoints.

**`/dev/amber/token`** — Reading this file triggers a new attestation request to Project Amber, and then reads the resulting attestation token response.  

**`/dev/amber/endpoint_apikey`** — This file contains the [API key](howto-manage-api-keys.md) used for Project Amber attestation. The user application must write the API key to this file, and the Amber client will use it for future attestation requests.

**`/dev/amber/status`** — This file contains the status of the client connection to Project Amber and the file is updated whenever a new attestation attempt is made.

**`/dev/amber/endpoint_url`** — The Gramine library OS does not innately support DNS resolution. Thus, the URL and IP address of a Project Amber endpoint must be configured for the Project Amber client to function. Write the Project Amber endpoint to this file (https://https://projectamber.intel.com/).

**`/dev/amber/endpoint_ip`** — The Gramine library OS does not innately support DNS resolution. Thus, the URL and IP address of a Project Amber endpoint must be configured for the Project Amber client ot function. Write the Project Amber endpoint to this file (https://projectamber.intel.com).

## Cloud deployment using Gramine

The Azure Confidential Computing services supports Intel SGX, i.e.,the service provides Intel SGX-enabled virtual machines (VMs). These VMs also support Gramine's Intel® SGX attestation. 

Gramine's documentation provides instructions on how to compile Gramine on the Intel® SGX-enabled Azure VMs. To build Gramine with the Project Amber client pre-installed, use the Project Amber fork instead of the main Gramine version branch, as shown in the following example. 

To build Gramine with the Project Amber client pre-installed, use the Project Amber fork instead of the main Gramine version branch, as shown in the following example.

```bash
$ git clone --depth 1  https://github.com/gramineproject/gramine.git
$ cd gramine
$ git fetch origin pull/1065/head:amber_pr_kbs
$ git checkout amber_pr_kbs
```

## Gramine Shielded Containers

Gramine maintains a separate project called Gramine Shielded Containers, which provides a tool to convert existing container images into _graminized_ images. Graminization provides the simplest possible migration of an existing container-based application to an Intel® SGX-enabled Gramine application. 

For more information, see the [Gramine Shielded Containers synopsis](https://gramine.readthedocs.io/projects/gsc/en/latest/).

To add the Project Amber client, update the `config.yaml` file to set the Project Amber client branch before building the GSC tool, as shown in the following code.

```bash
# Specify the OS distro. Currently tested distros are
# ``ubuntu:18.04``, ``ubuntu:20.04``, ``ubuntu:21.04`` and ``centos:8``.
Distro: "ubuntu:18.04"

# If the image has a specific registry, define it here.
# Empty by default; example value: "registry.access.redhat.com/ubi8".
Registry: ""

# If you're using your own fork and branch of Gramine, specify the GitHub link and the branch name
# below; for Project Amber, use the values in the sample below:
Gramine:
    Repository: "https://github.com/bigdata-memory/gramine.git"
    Branch:     "amber_pr"

# Specify the Intel SGX driver installed on your machine (more specifically, on the machine where
# the graminized Docker container will run); there are several variants of the SGX driver:
#
#   - legacy out-of-tree driver: use something like the below values, but adjust the branch name
#         Repository: "https://github.com/01org/linux-sgx-driver.git"
#         Branch:     "sgx_driver_1.9"
#
#   - DCAP out-of-tree driver: use something like the below values
#         Repository: "https://github.com/intel/SGXDataCenterAttestationPrimitives.git"
#         Branch:     "DCAP_1.11 && cp -r driver/linux/* ."
#
#   - DCAP in-kernel driver: use empty values like below
#         Repository: ""
#         Branch:     ""
#
SGXDriver:
    Repository: ""
    Branch:     ""
```

## Gramine container integration

Gramine maintains a [base Gramine Docker image](https://hub.docker.com/r/gramineproject/gramine) at DockerHub.
This Gramine image is a minimal collection of the essential Gramine binaries and tools. It still requires Intel® SGX support on the underlying virtual machine or physical server host (including DCAP drivers). The image **does not** contain the Project Amber client. This option can be used to handle the Intel® SGX enclave implementation if the [Project Amber client](concept-client-integration.md) is added to the customer application.

For more information, see the [Gramine Container integration documentation](https://gramine.readthedocs.io/en/stable/container-integration.html).

## Update the manifest workload

Change the manifest file of the target workload by configuring the following Project Amber related settings:

```bash
# dummy configuration for project Amber
# please replace the IP with real IP of project Amber endpoint
sgx.amber_ip = "127.0.0.1"
sgx.amber_url = "https://127.0.0.1:443/appraisal/v1/"
sgx.amber_apikey = ""

# dummy configuration for KBS provided by project Amber
# the public key component of a 2048-bit RSA key for secret wrapping
sgx.amber_userdata = "5wISSLU3UP0vZ8G+pgkO3BhhRAtdcY22UMomtdQabSxlA=="
sgx.kbs_ip = "127.0.0.1"
sgx.kbs_url = "https://127.0.0.1:9443/kbs/v1/keys/"
sgx.kbs_keyid = "ae6281b3-e4fe-3db5-82fd-eed40f6e4f18"
```

## Gramine Amber client code samples 

The code sample (in Go) below demonstrates using the pseudo device files exposed Project Amber's Gramine fork to interact with Project Amber.  

```golang
@@ -0,0 +1,107 @@
/*
*  Copyright (C) 2022, Intel, All Rights Reserved
*
*  SPDX-License-Identifier: Apache-2.0
*
*  Licensed under the Apache License, Version 2.0 (the "License"); you may
*  not use this file except in compliance with the License.
*  You may obtain a copy of the License at
*
*  http://www.apache.org/licenses/LICENSE-2.0
*
*  Unless required by applicable law or agreed to in writing, software
*  distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
*  WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
*  See the License for the specific language governing permissions and
*  limitations under the License.
*/

#define _GNU_SOURCE

#include <assert.h>
#include <errno.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <fcntl.h>

int read_to_buffer(const char* fn, char buf[], size_t bufsz);

#define AMBER_TOKEN_DEVFILE "/dev/amber/token"
#define AMBER_SECRET_DEVFILE "/dev/amber/secret"
#define AMBER_STATUS_DEVFILE "/dev/amber/status"
#define BUF_SZ 8096

int read_to_buffer(const char* fn, char buf[], size_t bufsz) {
    int ret;
    ssize_t cnt;
    int fd = open(fn, O_RDONLY);
    if (fd < 0) {
        fprintf(stderr, "[error] cannot open '%s'\n"
                        "Please make sure this app is running with Gramine-SGX\n", fn);
        return -1;
    }

    ssize_t bytes_read = 0;
    while (1) {
        cnt = read(fd, buf + bytes_read, bufsz - bytes_read);
        if (cnt > 0) {
            bytes_read += cnt;
        } else if (cnt == 0) {
            /* end of file */
            buf[bytes_read] = '\0';
            break;
        } else if (errno == EAGAIN || errno == EINTR) {
            continue;
        } else {
            fprintf(stderr, "[error] cannot read '%s'\n", fn);
            close(fd);
            return -1;
        }
    }

    ret = close(fd);
    if (ret < 0) {
        fprintf(stderr, "[error] cannot close '%s'\n", fn);
        return -1;
    }

    return ret;
}


int main(int argc, char** argv) {
    int ret;
    char buf[BUF_SZ] = {0};

    ret = read_to_buffer(AMBER_TOKEN_DEVFILE, buf, BUF_SZ);
    if (ret == 0) {
        printf("Read from %s: \n%s\n", AMBER_TOKEN_DEVFILE, buf);
    } else {
        printf("Failed to read from %s: %d\n", AMBER_TOKEN_DEVFILE, ret);
    }

    return ret;
}
```

