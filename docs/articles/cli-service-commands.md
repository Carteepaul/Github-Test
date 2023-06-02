---
title: Project Amber CLI Service Management
description: Project Amber CLI service management commands.
author:
topic-type: conceptual
date: 12/05/2022
---

# Service management  

The commands listed below are used to manage your services. 

> [!NOTE]

> You need your API key to perform these commands. Have your API key available before attempting these commands. Follow the [retrieve an API key](cli-examples.md#retrieve-an-attestation-api-key) instructions.  

## List service offers  

These commands list your service offer.

```bash
tenantctl list serviceOffer -a < api key >  
```

### Sample call

```bash
tenantctl list serviceOffer -a 4rririr887604or09rfi484e7ordj879 
```

### Sample response

```bash
[
  {
    "id": "17d27d6d-8897-46ad-b8db-a178435ccc73",
    "name": "SGX Attestation"
  },
  {
    "id": "17d27d6d-8897-46ad-b8db-a178435ccc73",
    "name": "TDX Attestation"
  }
]
```

## List products  

These commands list the products in your Project Amber environment.

```bash
tenantctl list product -a < api key > -r < service offer id >
```  

### Sample call 

```bash
tenantctl list product -a 4rririr887604or09rfi484e7ordj879 -r ea6ad8d3-fd3f-4ccb-82c7-ac021899a199
```

### Sample Response  

```bash
[
  {
    "id": "17d27d6d-8897-46ad-b8db-a178435ccc73",
    "service_offer_id": "2ed1896a-dc08-4bdd-bb86-f9e8204dadbd",
    "name": "Basic",
    "policy": {
      "limit": 40,
      "quota": 1000,
      "limit_renewal_period": 60,
      "quota_renewal_period": 2592000
    }
  },
  {
    "id": "17d27d6d-8897-46ad-b8db-a178435ccc73",
    "service_offer_id": "2ed1896a-dc08-4bdd-bb86-f9e8204dadbd",
    "name": "Premium",
    "policy": {
      "limit": 40,
      "quota": 2500000,
      "limit_renewal_period": 60,
      "quota_renewal_period": 2592000
    }
  }
]
```

## Create service   

These commands enable you to create a new service in your environment.

```bash
tenantctl create service -a < api key > -r < service offer id > -n < service name >
```

### Sample call

```bash
tenantctl create service -a 4rririr887604or09rfi484e7ordj879 -r ea6ad8d3-fd3f-4ccb-82c7-ac021899a199 -n "SGX Attestation User1"
```

### Sample response  

```bash
{
  "id": "17d27d6d-8897-46ad-b8db-a178435ccc73",
  "tenant_id": "20e70b5f-3bbe-42ac-9020-5c7f114bad3b",
  "service_offer_id": "2ed1896a-dc08-4bdd-bb86-f9e8204dadbd",
  "description": "SGX Attestation User1",
  "created_at": "1010-01-10101:11:200"
}
```

## List services

These commands list the services in your environment. 

```bash
tenantctl list service -a < api key >
```

### Sample call  

```bash
tenantctl list service -a 4rririr887604or09rfi484e7ordj879  
```

### Sample response  

```bash
[
  {
    "id": "17d27d6d-8897-46ad-b8db-a178435ccc73",
    "tenant_id": "20e70b5f-3bbe-42ac-9020-5c7f114bad3b",
    "service_offer_id": "2ed1896a-dc08-4bdd-bb86-f9e8204dadbd",
    "description": "SGX Attestation User1",
    "created_at": "2022-01-10101:11:200"
  },
  {
    "id": "17d27d6d-8897-46ad-b8db-a178435ccc73",
    "tenant_id": "20e70b5f-3bbe-42ac-9020-5c7f114bad3b",
    "service_offer_id": "2ed1896a-dc08-4bdd-bb86-f9e8204dadbd",
    "description": "TDX Attestation",
    "created_at": "2022-10-10101:11:200"
  }
]
```

## Delete service

This command deletes the Project Amber service.  

```bash
tenantctl delete service -a < api key > -s < service id >  
```

### Sample call

```bash
tenantctl delete service -a 4rririr887604or09rfi484e7ordj879 -s 225cc19c-911e-4ddd-89c6-d63fedef4ab8 
```

### Sample response  

```bash
Deleted service with Id: 225cc19c-911e-4ddd-89c6-d63fedef4ab8 
```

## Update service

This commands updates a Project Amber service. You need the ID of the service being updated and your API key. To obtain your API key, follow the [retrieve API key](cli-examples.md#retrieve-an-attestation-api-key) instructions. 

```bash
tenantctl update service -a < api key > -s < service id > -n < service name >
```

### Sample call  

```bash
tenantctl update service -a 7f478e75e50a4c25a89dbc93fc6df781 -s 0d643b2c-ac26-446a-b875-b84b2e9a3584 -n "SGX Attestation User1 Updated"
```

### Sample response  

```bash
{
  "id": "17d27d6d-8897-46ad-b8db-a178435ccc73",
  "tenant_id": "20e70b5f-3bbe-42ac-9020-5c7f114bad3b",
  "service_offer_id": "2ed1896a-dc08-4bdd-bb86-f9e8204dadbd",
  "description": "TDX Attestation",
  "created_at": "2022-10-10101:11:200"
}
```