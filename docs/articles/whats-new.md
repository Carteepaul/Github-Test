---
title: What's New
description: A short description on the new features of Project Amber.
author: 
topic: conceptual
date: 01/23/2023
---

# What's New

## February 2023 

### Intel® TDX Attestation Support

  Added Intel® TDX Go client and CLI. Developers can now use Project Amber for Intel® TDX VM remote attestation using the new go-tdx client libraries or Intel® TDX CLI client.   

### Microsoft Azure Attestation Support

  Microsoft Azure Attestation (MAA) adapter service. MAA adapter allows developers to use Project Amber attestation instead of Azure attestation with few or no changes to their existing MAA code. In many cases, only attestation policies need to be rewritten.

### Integration with Azure Marketplace

  Project Amber subscriptions can now be added and managed through the Azure Marketplace.

## November 2022

### Onboarding email now contains instructions
  
  The onboarding email for new users previously only contained an account verification link.  The onboarding email now contains detailed instructions for the onboarding process for new users.

### API URI Refactoring
  
  Most REST API URIs have changed.  Please see the updated [API documentation](~/restapi/restapi-overview.md) for the new resource paths.  The actual functions for these APIs have not changed.

### Quick Start Actions
  
  A new ribbon will appear at the top of the Project Amber page for new users with a link to a new "Quick Start Actions" popup.  The ribbon can be dismissed at any time.  The link will open a popup containing brief instructions for common actions within the Project Amber UI:

  - Subscribing to a service
  - Configuring an API Key
  - Downloading the Project Amber Attestation Client
  - Reports and Metrics
  - Other Actions, including creating user-defined policies and inviting new users
  