---
title: Project Amber UI Workflows
description: An overview of the workflows in Project Amber.
author:
topic: conceptual
date: 12/05/2022
---

# Project Amber UI Workflows

Articles in this section provide guidance on common workflows within Project Amber focusing on the web user interface.

[Service management](howto-manage-service-offers.md)

[User management](howto-manage-users.md)

[Policy management](howto-manage-attestation-policies.md)

[API key management](howto-manage-api-keys.md)

[Tag management](howto-manage-tags.md)

## Getting Started with Project Amber

The first task after receiving an invitation to Project Amber is to subscribe to one or more service offers, which are attestation services for specific TEEs like Intel® SGX or Intel® TDX.  

[Subscribing to a Service Offer](howto-manage-service-offers.md)

Once a service offer is active, you'll need to create at least one attestation API key.

[API Key Management](howto-manage-api-keys.md)

The API key will be used by your workloads and/or a relying party to request attestation tokens from the Project Amber attestation services.

Once you have an API key, all that's required is an application needing attestation.  Project Amber provides easy [client integration models](concept-integrations-overview.md) to help developers use attestation with minimal code changes to existing applications.