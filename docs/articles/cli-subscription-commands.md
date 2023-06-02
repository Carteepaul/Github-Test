---
title: Project Amber CLI Subscription Management
description: Project Amber CLI subscription management commands.
author: Place Holder
topic-type: conceptual
date: 01/18/2023
---

# apiClient management  

These commands are used to manage API clients. 

>[!NOTE] 
> You need your API key to perform these commands. See the [retrieve an API key](cli-examples.md#retrieve-an-attestation-api-key) instructions for more information. 

## Create apiClient

This command creates a new Project Amber API client.  

```bash
tenantctl create apiClient -a < api key > -r < service id > -p < product id > -d < subscription name > -i "comma separated policy Ids" -v "tag-id1:tag-name1,tag-id2:tag-name2" -e "date in format yyyy-mm-dd"
```  

### Sample Call

```bash
tenantctl create apiClient -a 4rririr887604or09rfi484e7ordj879 -r jk83er00-lk34hdu73-gt3f-49ike33-039s -p i993kkdk-d8e3-uy77-eudm- 9ek33kepodig -d TenantClikeyuser345 -i "87408d94-384r-23er-0983-2789dkdme832" -v "88di3k9i-dkdj-3de3-8732-847304478303:kdmclejsn,8g32d234-a3dk-9554-2048-f0s4958420i5:WorkloadVal" -e "2022-11-21"
```

### Sample response  

```bash
{
  "id": "948rujr4-e3rt-0oke-ok8u-fr45tk9mf2q1",
  "service_id": "09kjmn3w-9i8u-043kd-p0j3-3erf098kjmnf",
  "service_offer_name": "",
  "product_id": "pldo9ide3e-09id-0odc-p0od-podo85mn324d",
  "product_name": "",
  "status": "Active",
  "description": "TenantCliKeyUser",
  "expired_at": "2022-08-14T11:08:40.804604528Z",
  "keys": [
    "kkr8r9r8484jfmf874jf84j48fj48ffd",
    "4iujde30i2dkmnvz12sakmfviujd34ln"
  ],
  "policy_ids": [
    "94kldhj0-741l-838g-8361-734ckdes7340"
  ],
  "tags": [
    {
      "id": "9i8du4d8-ebda-8403-3249-gdlodee0dkn3",
      "name": "Workload",
      "value": "WorkloadVal",
      "predefined": true
    },
    {
      "id": "944r90rk-dk24-dkne-dkeq-3k3mf099ek32",
      "name": "Voltage",
      "value": "tagvalabc",
      "predefined": false
    }
  ],
  "created_at": "0001-01-01T00:00:003"
}
```

## Update apiClient  

 This command updates your current apiClient.

```bash
tenantctl update apiClient -a < api key > -r < service id > -p < product id > -u < subscription id > -d < subscription name > -i "comma separated policy Ids" -v "tag-id1:tag-name1,tag-id2:tag-name2" -s "<Active/Inactive/Cancelled>" -e "date in format yyyy-mm-dd"
```

### Sample call

```bash
tenantctl update apiClient -a 4rririr887604or09rfi484e7ordj879 -r jk83er00-lk34hdu73-gt3f-49ike33-039s -p i993kkdk-d8e3-uy77-eudm-9ek33kepodig -d TenantClikeyuser3452 -v "88di3k9i-dkdj-3de3-8732-847304478303:kdmclejsn" -s Inactive -u asdfsdfa-3dk9-554a-sldk-fjlskdfjf0s4 -e 2022-11-21
```

### Sample response  

```bash
{
  "id": "948rujr4-e3rt-0oke-ok8u-fr45tk9mf2q1",
  "service_id": "9kjmn3w-9i8u-043kd-p0j3-3erf098kjmnv",
  "product_id": "pldo9ide3e-09id-0odc-p0od-podo85mn324d",
  "product_name": "",
  "status": "Inactive",
  "description": "TenantCliKeyUser2",
  "expired_at": "2022-11-14T11:08:40.804604Z",
  "created_at": "2022-10-14T10:15:24.376065Z"
}
```

## List apiClient   

This command lists your apiClient.   

```bash
tenantctl list apiClient -a < api key > -r < service id >
```

### Sample call

```bash
tenantctl list apiClient -a 4rririr887604or09rfi484e7ordj879 -r jk83er00-lk34hdu73-gt3f-49ike33-039s  
```

### Sample response

```bash
[
  {
    "id": "948rujr4-e3rt-0oke-ok8u-fr45tk9mf2q1",
    "service_id": "09kjmn3w-9i8u-043kd-p0j3-3erf098kjmnf",
    "product_id": "pldo9ide3e-09id-0odc-p0od-podo85mn324d",
    "product_name": "Premium",
    "status": "Inactive",
    "description": "TenantCliKeyUser2",
    "expired_at": "2022-11-14T11:08:40.804604Z",
    "created_at": "2022-10-14T10:15:24.376065Z"
  }
]
```

## List apiClient by ID

This command lists a specific Project Amber apiClients.  

```bash
tenantctl list apiClient -a < api key > -r < service id > -d < subscription id >
```

### Sample call

```bash
tenantctl list apiClient -a 4rririr887604or09rfi484e7ordj879 -r jk83er00-lk34hdu73-gt3f-49ike33-039s -d 36ffa32c-e1df-46a2-9d57-f2be51c07d9f
```

### Sample response  

```bash
{
  "id": "948rujr4-e3rt-0oke-ok8u-fr45tk9mf2q1",
  "service_id": "09kjmn3w-9i8u-043kd-p0j3-3erf098kjmnf",
  "service_offer_name": "SGX Attestation",
  "product_id": "pldo9ide3e-09id-0odc-p0od-podo85mn324d",
  "product_name": "Premium",
  "status": "Inactive",
  "description": "TenantCliKeyUser2",
  "expired_at": "2022-11-14T11:08:40.804604Z",
  "keys": [
    "kkr8r9r8484jfmf874jf84j48fj48ffd",
    "4iujde30i2dkmnvz12sakmfviujd34ln"
  ],
  "policy_ids": [],
  "tags": [
    {
      "id": "948rujr4-e3rt-0oke-ok8u-fr45tk9mf2q1",
      "name": "pname",
      "value": "TagValue",
      "predefined": false
    }
  ],
  "created_at": "2022-10-14T10:15:24.376065Z"
}
```

## Delete an apiClient  

This command deletes a Project Amber apiClient. 

```bash
tenantctl delete apiClient -a < api key > -r < service id > -d < subscription id >
```

**Sample Call**   

```bash
tenantctl delete apiClient -a 4rririr887604or09rfi484e7ordj879 -r jk83er00-lk34hdu73-gt3f-49ike33-039s -d 36ffa32c-e1df-46a2-9d57-f2be51c07d9f
```

### Sample response   

```bash
Deleted api client with Id: 36ffa32c-e1df-46a2-9d57-f2be51c07d9f
```

## Create tag   

This command creates a tag for Project Amber.  

```bash
tenantctl create tag -a < api key > -n < tag name > -t < tenant Id >  
```

### Sample call

```bash
tenantctl create tag -a 4rririr887604or09rfi484e7ordj879 -n TestTagUsername
```

### Sample response   

```bash
{
  "id": "60f29395-2ffc-4e65-8e60-5abbd2e97d1c",
  "name": "TestTagUsername",
  "predefined": false
}
```

## List apiClient tags   

This command lists all your Project Amber tags.  

```bash
tenantctl list apiClient tag -a < api key >
```

### Sample call  

```bash
tenantctl list apiClient tag -a 4rririr887604or09rfi484e7ordj879  
```

### Sample response  

```bash
{
  "tags": [
    {
      "id": "hyu7jii1-hyy2-2478-7yu8-dw3efj79ojy7",
      "name": "Workload",
      "predefined": true
    },
    {
      "id": "3edfew32-7yhj-724h-14hy-75e34fds67jk",
      "name": "create_tag_test_tenant_key",
      "predefined": false
    },
    {
      "id": "de4u7jkku-3f5g-2qzx4de3-4h7ukoiddsw4",
      "name": "create_tag_sanity_tenant_key",
      "predefined": false
    },
    {
      "id": "dr3erfewe2-fd32-w2qr-hyr4-4rdhu78jdssd2",
      "name": "TC935-tag",
      "predefined": false
    },
    {
      "id": "etr4eswr-g5u7-6hu8-gr54-78we32fjsa17",
      "name": "create-tag1",
      "predefined": false
    },
    {
      "id": "7u8ikldg-s382-7hud-vb38-78jhsf3r5tf0",
      "name": "UsernameTag",
      "predefined": false
    },
    {
      "id": "47yukjsv-29op-38sq-v3g8-xcqd456hju7i",
      "name": "Username-Tag",
      "predefined": false
    },
    {
      "id": "6tgk7s84-mf61-5781-7562-fe4rthu78vdx",
      "name": "Username_Tag",
      "predefined": false
    },
    {
      "id": "4e4jsc86-n290-29hj-158l-a4f7mh379147",
      "name": "TagName",
      "predefined": false
    },
    {
      "id": "23r56t21-2w3e-1s46-7w31-2sfjku6n38s5v",
      "name": "TestTagUsername",
      "predefined": false
    }
  ]
}
```

## List subscription policies  

This command lists your subscriptions and policies. 

```bash
tenantctl list subscription policy -a < api key > -r < service id > -s < subscription id >
```

### Sample call

```bash
tenantctl list subscription policy -a 4rririr887604or09rfi484e7ordj879 -r jk83er00-lk34hdu73-gt3f-49ike33-039s -s 43df35dh-7f48-82fk-qn70-w2juiolikik4 
```

### Sample response  

```bash
{
  "policy_ids": [
    "c3c77a2d-f71c-4e1c-96b5-c6fc29fecee2"
  ]
}
```

## List subscription tags  

This command lists the tags associated to a specific subscription.  

```bash
tenantctl list subscription tag -a < api key > -r < service id > -s < subscription id >
```

### Sample call  

```bash
tenantctl list subscription tag -a 4rririr887604or09rfi484e7ordj879 -r jk83er00-lk34hdu73-gt3f-49ike33-039s -s 43df35dh-7f48-82fk-qn70-w2juiolikik4
```

### Sample response

```bash
{
  "tags_values": [
    {
      "id": "7b96c518-b8ba-4257-8430-d3f1474803f9",
      "name": "Workload",
      "value": "",
      "predefined": true
    }
  ]
}
```