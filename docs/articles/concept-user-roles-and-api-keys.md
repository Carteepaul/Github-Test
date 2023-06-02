---
title: User roles and API keys
description: Project Amber CLI policy management commands.
author: Paul Cartee
topic-type: conceptual
date: 04/22/2023
---

# User roles and API keys

User roles and API keys are provisioned through Project Amber’s user interface (UI). The user role determines which API keys you can access. This article explains the relationship between user roles and API keys.

## User roles

Project Amber has two types of user roles, users and tenant admins. Your subscription determines the number of each user type.

### Users

Your service plan determines the number of users allowed in your instance of Project Amber. A basic plan allows five users, while a premium plan allows unlimited users.  Users can modify all Project Amber resources except other users and admin API keys. The keys can be integrated into the application workflow when an attestation is requested.

### Tenant admins

Tenant admins interact with Project through the Web UI, the CLI, and REST APIs. Your service plan determines the number of tenant admins. A basic plan allows one tenant admin, while a premium plan allows up to five tenant admins. Tenant admins can manage all Project Amber resources, including other users and admin API keys. Tenant admins use the admin API keys to interact with Project Amber using the CLI and REST APIs.


The table below lists the resources each user role can manage through the Web UI.

|Resource             |User|Tenant Admin|
|---------------------|:--:|:----------:|
|Attestation API Keys |  X |     X      |
|Policies             |  X |     X      |
|Tags                 |  X |     X      |
|Users                |    |     X      |
|Admin API keys       |    |     X      |
|Reports              |  X |     X      |
|Web UI               |  X |     X      |


## API keys

Two types of API keys are provided to clients to manage resources: attestation API keys and admin API keys. User roles determine which API keys can be accessed through the User Interface (UI). Users can only access attestation API keys, while tenant admins can access both attestation and admin API keys.

### Tenant admin API keys

Each tenant is issued two admin API keys accessible through the UI. Tenant admin API keys are administrative keys. They are used with CLI utilities or REST APIs to access all the same functions managed in the Web UI, such as adding, editing, and deleting policies, tags, and other users. They can also be used to unsubscribe and delete accounts and all corresponding API keys.

> [!WARNING]
> A Project Amber admin API key is the only authorization needed to call any API or CLI method. It is critical to safeguard the admin API keys to avoid potentially compromising the Project Amber-based confidential computing system.

### Attestation API keys

Both tenant admins and users have access to attestation API keys through the UI. The number of attestation API keys an instance of Project Amber can have is determined by the subscription type. A basic subscription is given one API key while a premium subscription is give multiple API keys. Attestation API keys are used for all attestation-related functions such as, the Microsoft Azure Attestation adaptor and Faithful Verification. This key cannot be used for other tasks, such as managing policies, tags, or other users.

The table below lists the resources each API can manage.

|Resource                         |Admin API|Attestation API|
|---------------------------------|:-------:|:-------------:|
|Attestation                      |         |       X       |
|Nonce                            |         |       X       |
|Policies                         |    X    |               |
|Tags                             |    X    |               |
|Users                            |    X    |               |
|Admin API keys                   |    X    |               |
|Reports - Faithful Verifications |         |       X       |