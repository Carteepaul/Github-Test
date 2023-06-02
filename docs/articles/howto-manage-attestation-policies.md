---
title: Attestation Policy Management
description: An overview of attestation policies. 
author:
topic: conceptual
date: 12/05/2022
---

# Policy management

This article contains sample workflows for managing attestation policies. For additional detail on how policies are used, see the [Policies](concept-policies.md) article.

Policies can be managed using the Project Amber web interface, with the [client CLI](cli-policy-commands.md), or by [REST API](~/restapi/restapi-policy-management.md).

**Mandatory fields:**   
- API Key Name
- Product Type

**Optional fields:**   
- Scope
- Set of Policies & Tags

## Policy Types

There are two types of policies, Appraisal and Token customization. Appraisal policies evaluate the evidence provided and determines whether Project Amber issues an attestation toke. A Token customization policy allows customized claims that are part of the attestation token. 

Each policy type can be signed or unsigned. A signed policy is encrypted and more secure. The relaying party can be assured that the policy is unchanged.

## View policies

1. Sign in to the Project Amber portal.
1. Select **Manage policies**.
1. View the policies in the Policy Library pane.

    ![View Policies](media/howto-manage-attestation-policies/policy-detail.png)

## Search by policy name

1. Sign in to the Project Amber portal.
1. Select **Manage policies**.
1. In the **Search by policy name:** box, enter the policy name. The search is incremental. It begins after the first letter is entered. 
    The search results are displayed in the Policy Library pane.

## Filter by attestation type

1. Sign in to the Project Amber portal.
1. Select **Manage policies**.
1. Select the attestation type from the **Filter by attestation type:** dropdown list.
    ![View Policies](media/howto-manage-attestation-policies/filter-by-service.png)
    The search results are displayed in the Policy Library pane.

## Add a policy 
1. Sign in to the Project Amber portal.  
1. Select **Manage policies**.  
    ![Add a policy](media/howto-manage-attestation-policies/manage-policy.png) 
1. Select **ADD A POLICY**.
1. Enter a **Policy name**.
    ![View Policies](media/howto-manage-attestation-policies/add-policy.png)
1. Select an type of attestation type you want from the **Choose an attestation type** dropdown list.
    ![Select attestation](media/howto-manage-attestation-policies/attestation-type.png)
1. Select a policy type for the selected attestation.
    ![Select policy type](media/howto-manage-attestation-policies/policy-type.png)
1. Select **BROWSE**.
    ![Upload the policy](media/howto-manage-attestation-policies/upload-policy.png)
1. Go to the policy.
1. Select the policy and then select **Open**.
1. Select **SAVE**.
    The Manage Policies page displays with the new policy displayed in the Policy Library pane. 

## Edit a policy  

1. Sign in to the Project Amber portal.
1. Select **Manage policies**. 
1. Select the ![edit ](media/common-graphics/edit-icon.png) icon for the policy you want to edit.  
    ![Edit policy](media/howto-manage-attestation-policies/edit-popup.png)
1. Select **EDIT POLICY**.  
![Alt text](media/howto-manage-attestation-policies/edit-policy.png)  
1. You can edit the following:  
    - Policy name  
        1. In the Policy name box, enter a new name for the policy.  
    - Assign a new policy file  
        1. Select **BROWSE**.  
        1. Go to the policy file.  
        1. Select the policy file and then select **Open**.  
1. After your changes are made, select **SAVE**.   

## Delete a policy
1. Sign in to the Project Amber portal.
1. Select **Manage policies**.  
    ![Upload the policy](media/howto-manage-attestation-policies/policy-detail.png)
1. Select the ![delete](media/common-graphics/delete-icon.png) icon for the policy to be deleted.  
    ![Upload the policy](media/howto-manage-attestation-policies/delete-policy.png)
1. Select **DELETE POLICY**. 
    The policy is deleted.

## Sign a policy   

For instructions on signing a policy, see the [Policy Signing Tool](utility-policy-signing.md) article.
