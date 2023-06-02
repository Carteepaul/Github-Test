---
title: Utility Overview
description: An overview of Project Amber's downloadable utilities.
author:
topic: conceptual
date: 02/08/2023
---

# Project Amber downloadable utilities

Some Project Amber use cases require a downloadable utility to provide client-side functionality.  The articles in this section contain user guides for these utilities.

## Faithful Verifier

The Faithful Verifier (FV) is a Linux command-line utility that used to verify the fidelity ofProject Amber attestation tokens. Project Amber generates all tokens inside of TEE enclaves and registers each instance of the involved microservices with their TEE quote information in a ledger.  Every Project Amber attestation token contains references to the microservice IDs used in its generation, allowing the FV to retreive the TEE evidence and independently verify the "faithfulness" of the services used to generate the token.

[Faithful Verifier guide](utility-faithful-verifier.md)

## Policy Signing Tool

The Policy Signing Tool is an optional command-line utility that facilitates signing user-defined attestation policies.  Signed policies provide an additional layer of protection, letting the policy owner verify the integrity of the policies used for attestation.

[Policy Signing Tool user guide](utility-policy-signing.md)