---
title: Project Amber CLI User Management
description: Project Amber CLI user management commands.
author:
topic-type: conceptual
date: 12/05/2022
---

# User management

These instructions describe how to manage users with CLI commands. Users can also be managed through the [managing users](howto-manage-users.md) section of the Project Amber portal. 

> [!NOTE]
> You need Admin API keys to perform these commands. See the [Admin API key management](howto-manage-admin-api-keys.md) instructions for more information.  

## Create user  

Create a new user for your environment. 

`tenantctl create user -a < api key > -e < email Id> -r < Role (Tenant Admin/User) >`  

**Sample call**  

```bash
tenantctl create user -a 4rririr887604or09rfi484e7ordj879 -e abcdefgregularuser@gmail.com -r "User”
```

**Sample response**  

```bash
User: { "id": "598db1fc-4340-4bbc-9e19-2d596c3b7bd8", "email": abcdefgregularuser@gmail.com, "tenant_roles": [ { "tenant_id": "020f1162-25ed-441c-9d8f-69cfc7974cc1", "roles": [ { "id": "885391bf-2a37-4dc7-9444-833c5a817cdf", "name": "User" } ] } ], "active": false, "created_at": "0001-01-01T00:00:00Z" }
```

## List users   

This command lists the current users in your environment.   

`tenantctl list user -a < api key >` 

**Sample call**  

```bash
tenantctl list user -a 4rririr887604or09rfi484e7ordj879
```

**Sample response**   

```bash
Users: [ { "id": "e3968eb8-e053-4646-98dd-7b61991a66d1", "email": user1@gmail.com, "tenant_roles": [ { "tenant_id": "020f1162-25ed-441c-9d8f-69cfc7974cc1", "roles": [ { "id": "66ec2e33-8cd3-42b1-8963-c7765205446e", "name": "Tenant Admin" } ] } ], "active": true, "created_at": "2022-09-29T16:42:46.236499Z" }, { "id": "f9af3e31-9fc5-48db-afc3-c818049f6570", "email": CreateAdminUserSanityTMS1@gmail.com, "tenant_roles": [ { "tenant_id": "020f1162-25ed-441c-9d8f-69cfc7974cc1", "roles": [ { "id": "66ec2e33-8cd3-42b1-8963-c7765205446e", "name": "Tenant Admin" } ] } ], "active": false, "created_at": "2022-10-01T01:00:53.924856Z" }, { "id": "82ac5d98-c8ba-49cf-ac3e-4d4a1385be68", "email": CreateUserSanityTMS3ByTenant-updated-by-amber-admin@gmail.com, "tenant_roles": [ { "tenant_id": "020f1162-25ed-441c-9d8f-69cfc7974cc1", "roles": [ { "id": "885391bf-2a37-4dc7-9444-833c5a817cdf", "name": "User" } ] } ], "active": false, "created_at": "2022-10-01T01:27:44.145412Z" }, { "id": "42850600-7a58-43a1-970a-85bb4008cd88", "email": TC917-1@gmail.com, "tenant_roles": [ { "tenant_id": "020f1162-25ed-441c-9d8f-69cfc7974cc1", "roles": [ { "id": "66ec2e33-8cd3-42b1-8963-c7765205446e", "name": "Tenant Admin" } ] } ], "active": false, "created_at": "2022-10-11T20:30:40.348744Z" }, { "id": "1c83eeb3-e9f7-4a59-8104-7009f6f385b0", "email": TC917-2@hello.com, "tenant_roles": [ { "tenant_id": "020f1162-25ed-441c-9d8f-69cfc7974cc1", "roles": [ { "id": "66ec2e33-8cd3-42b1-8963-c7765205446e", "name": "Tenant Admin" } ] } ], "active": false, "created_at": "2022-10-11T21:16:45.358066Z" }, { "id": "598db1fc-4340-4bbc-9e19-2d596c3b7bd8", "email": abcdefgregularuser@gmail.com, "tenant_roles": [ { "tenant_id": "020f1162-25ed-441c-9d8f-69cfc7974cc1", "roles": [ { "id": "885391bf-2a37-4dc7-9444-833c5a817cdf", "name": "User" } ] } ], "active": false, "created_at": "2022-10-12T03:18:17.545383Z" }, { "id": "dd4b49c7-9207-43e4-a3c1-8c734cf828a4", "email": companytenantadmin@gmail.com, "tenant_roles": [ { "tenant_id": "020f1162-25ed-441c-9d8f-69cfc7974cc1", "roles": [ { "id": "66ec2e33-8cd3-42b1-8963-c7765205446e", "name": "Tenant Admin" } ] } ], "active": false, "created_at": "2022-10-12T03:18:19.458591Z" } ]
```

## Update user role  

Update the user role of a specific user.  

```bash
tenantctl update user role -a < api key > -u < user id > -r < role (Tenant Admin/User)
```

**Sample call**   

```bash
tenantctl update user role -a 4rririr887604or09rfi484e7ordj879 -u "4324598fs-0404-4f4r-9oii9-8f8flk893w21" -r "Tenant Admin"
```

**Sample response**  

```bash
Updated User: { "id": "598db1fc-4340-4bbc-9e19-2d596c3b7bd8", "email": abcdefgregularuser@gmail.com, "tenant_roles": [ { "tenant_id": "020f1162-25ed-441c-9d8f-69cfc7974cc1", "roles": [ { "id": "66ec2e33-8cd3-42b1-8963-c7765205446e", "name": "Tenant Admin" } ] } ], "active": false, "created_at": "2022-10-12T03:18:17.545383Z" }
```

## Delete user   

This command is used to delete a specific user.  

`tenantctl delete user -a < api key > -u < user id >`

**Sample call**  

```bash
tenantctl delete user -a 4rririr887604or09rfi484e7ordj879 -u "4324598fs-0404-4f4r-9oii9-8f8flk893w21"
```

**Sample response**    

```bash
User 4324598fs-0404-4f4r-9oii9-8f8flk893w21 deleted
```  