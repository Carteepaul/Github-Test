---
title: Project Amber CLI Installation
description: Project Amber CLI prerequisites and installation guide.
author:
topic-type: conceptual
date: 12/05/2022
---

# Project Amber Tenant CLI installation guide  

These instructions describe how to build and install the Project Amber tenant CLI. The Project Amber CLI is an open-source tool tenants use to make API calls to their instance of Project Amber. The binary for the Project Amber tenant CLI is available on [GitHub](https://github.com/intel/amber-cli).

## Build the Project Amber CLI  

Follow these instructions to build the Project Amber CLI.

### Prerequisite packages

- make

### Supported operating system

- Ubuntu LTS 20.04

> [!NOTE] 
> You must be a Tenant Admin to install the CLI, however, both the admin and users can use the CLI.

### Build the CLI

1. Create a directory on the build machine named `CLI`.

    `mkdir cli`

1. Go to the CLI directory.

    `cd cli`  

1. Clone the Project Amber CLI code to the newly created CLI directory by performing the following command.

    `git clone https://github.com/intel/amber-cli`

1. In the newly created CLI directory, create the CLI installer.

    `cd amber-cli && make installer`  

> [!NOTE]

> If behind a proxy, add the Amber FQDN to NO_PROXY the environment variable.

           
## Install the Project Amber tenant CLI  

Follow these instructions to install the tenant CLI.  

### Prerequisites

Have the following information before attempting these instructions:  

 - The URL of the API gateway  
 - Your tenant ID

### Install the tenant CLI

1. If you have not already navigated to the CLI directory in which the CLI was built, navigate to it.  

    `cd cli`  

1. Copy the binary installer to the system on which it is being deployed. 

     `tenantctl-{version}.bin` 

1. The `tac.env` file enables the CLI to contact a specific Project Amber instance so it can be used to make changes. Have the following information before creating the `tac.env` file.  

     - URL of the API Gateway  
     - Id of the the tenant to which the CLI is connecting 

1. Create the `tac.env` file and add th URL and tenant id.  

    `AMBER_BASE_URL=< URL of API Gateway`
    `TENANT_ID="< Id of the Tenant >"`  
    
1. To install the tenant CLI on your system, run following command:   

    `./tenantctl-{version}.bin`   
        
## Tenant CLI configuration    

Use the command below to configure the tenant CLI. You need to know the file path to the `tac.env` file created in the previous step.   

`tenantctl config -v < env file path >`

## Install Bash completion  

Use the following command to install bash completion for the Project Amber tenant CLI. 

`tenantctl completion`

## Retrieve the version  

To get the version number of the tenant CLI installed on your system, run the following command:   

`tenantctl version`

## Uninstall the CLI

To uninstall the tenant CLI, use the following command: 

`tenantctl uninstall` 

