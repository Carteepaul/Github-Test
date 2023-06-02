---
title: Project Amber Billing Management
description: An overview of billing management in Project Amber.
author:
topic: conceptual
date: 01/06/2023
---

# Attestation API Keys  

Project Amber uses API keys to authenticate attestation requests and to control the policies used to evaluate evidence claims from an attester to generate an attestation token and control the token's contents.

A given tenant will be subscribed to one or more service offerings, such as Intel® SGX Attestation or Intel® TDX Attestation. Attestation API keys must be generated for each subscribed service offering based on the needs of the application. For example, an application designed to use SGX attestation will require an API key for an Intel® SGX Attestation service offering.  

API keys are optionally associated with one or more policies that determine attestation behavior and can optionally be associated with tags that can be used for reporting and metrics visibility.  

[Attestation](concept-attestation-overview.md)

[Policies](concept-policies.md)

## Create an attestation API key  
Attestation API keys must be created using the Project Amber web UI. There are no CLI or REST API methods to generate new attestation API keys. Attestation API keys can be created by any user and become available for all users within the tenant organization.  

1. Sign into Project Amber.  
1. Select **Manage services**.  
    ![Manage Services](media/cli/manage-services-page.png)
1. Select **ADD API KEY**. 
     The **Add API key** page displays.  
     ![Add API key](media/howto-manage-api-keys/add-api-key.png)   
1. Provide a name for the API key. Names can be any string of up to 64 alphanumeric characters. Spaces and special characters other than "_" or "-" are not supported.  
1. Optionally, assign one or more tags to the new API key. Tags are key-value pairs use to help track utilization for reports and metrics. Note that the Workload tag is pre-defined. This tag can use values like an application's name to track the attestations requested by a given application.  See [Tag management](howto-manage-tags.md) for more details.
1. Optionally, assign one or more policies to the new API key. Use the **Select an existing policy** dropdown menu to choose from existing policies. Alternatively, you can create a new policy by selecting the **Create a new policy** radio button. See [Policy management](howto-manage-attestation-policies.md) for more details.  
1. Once finished, select **SAVE & CONTINUE**.  
1. Review the information on the **Confirm API key details** page.  
1. If everything is correct, select **SUBMIT**. 
    The new API key is created, along with any new tags and/or policies.  