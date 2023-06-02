---
title: Project Amber CLI Policy Management
description: Project Amber CLI policy management commands.
author:
topic-type: conceptual
date: 04/20/2023
---

# Policy management

This section provides commands to create, get, and update a Project Amber policy.

> [!NOTE] 
> You need your API key to perform these commands. See the [retrieve API key](cli-examples.md) instructions for more information.

## Create policy  

This command creates a new Project Amber policy. You need your API key and the file path to where the policy is to be stored to perform this command.

```bash
tenantctl create policy -a < api key > -f < policy file path >  
```

### Sample policy for `create policy` command

```bash   
      {
      "policy": "default matches_sgx_policy = false \n\n matches_sgx_policy = true { \n input.amber_tee_is_debuggable == false \n input.amber_sgx_isvsvn == 0 \n input.amber_sgx_isvprodid == 0 \n input.amber_sgx_mrsigner ==  \"d412a4f07ef83892a5915fb2ab584be31e186e5a4f95ab5f6950fd4eb8694d7b\" \n  \n input.amber_sgx_mrenclave == \"bab91f200038076ac25f87de0ca67472443c2ebe17ed9ba95314e609038f51ab\" \n }"
      "user_id": "f04971b7-fb41-4a9e-a06e-4bf6e71f98b3",
      "policy_name": "Sample_Policy_SGX",
      "policy_type": "Appraisal policy",
      "service_offer_name": "SGX Attestation",
      "service_offer_id": "b04971b7-fb41-4a9e-a06e-4bf6e71f98bd"  
      }
```   

## List policies 

This command retrieves a list of your Project Amber policies.  

```bash  
tenantctl list policies -a < api key >
```    

### Sample call  

```bash
tenantctl list policy -a 556a88d022644733ab75f5c5fbc4db62 
``` 

### Sample response

```bash
      [
        {
          "policy_id": "41677eb9-023d-402d-9966-909cdfff0889",
          "policy": "default matches_sgx_policy = false \n\n matches_sgx_policy = true { \n input.amber_tee_is_debuggable == false \n input.amber_sgx_isvsvn == 0 \n input.amber_sgx_isvprodid == 0 \n input.amber_sgx_mrsigner ==  \"d412a4f07ef83892a5915fb2ab584be31e186e5a4f95ab5f6950fd4eb8694d7b\" \n  \n input.amber_sgx_mrenclave == \"bab91f200038076ac25f87de0ca67472443c2ebe17ed9ba95314e609038f51ab\" \n }",
          "policy_name": "Policy1SGXUsername",
          "policy_type": "Appraisal policy",
          "service_offer_id": "dca3fa42-a8e6-4eb6-bb3f-799d917bc529",
          "service_offer_name": "SGX Attestation",
          "creator_id": "00000000-0000-0000-0000-000000000000",
          "updater_id": "00000000-0000-0000-0000-000000000000",
          "deleted": false,
          "created_time": "2022-10-14T10:12:07.993695Z",
          "modified_time": "2022-10-14T10:12:07.993695Z",
          "policy_hash": "l5BKcCbuinJ7bPKnrp7g9TQhxQewoRRwK2ZKQJhUutKlrVeCHtyMvnV8ik2+iiEh",
          "policy_signature": "kIocVNQa9M6s5S9rE+cysBgNV63E6xLZ0bxKyExCAPkENWbXBU/9njt0FznpRuOziEUUKEaylx/7NpwdHsYKDUfirN3Owg26IbBUU/YOakqEFRWkkOJbvV//V/TCq4bNRIeschnIPWSVCmvOrAgexI5UwDbGgul6W+445TWm4WBOq+LIvZrQxFRVRPsdL1R0UqA3BGsggcCmOoSG3lK2dnV/SKM6EtHMiyT5e11M/TycJny04NQT7vm0jpkKV7hN2VdKorsAXsE4ffQp0BOwv/CjXoyRiuwWVEUwTQS5avS5vggyx1X46xApS7PWnYCZbRaBg/jmBNfNhCyvkD/EFAxubuNGwgDkppow1kltgKtRMtgvh8xLpRjmTLvhoZlzwrOV4136j49XFkhZE8zBl3qMh2k7OfWgYzzcfTb9kidCpMWX5Xf7x3eObyNPZ2GK1pql4ksOPG7SuaBqP75VOG40I0iGW33JxtMqZZaUZFRSJXzzVIlfvnuhgLhOwFQO"
        },
        {
          "policy_id": "4309afa9-9302-4efa-b95b-6a2e93e4f01a",
          "policy": "get_token_fields[token_fields] { \n token_fields := { \n \"dd-isv-svn\" : input.amber_sgx_isvsvn, \n } \n }",
          "policy_name": "policy3210",
          "policy_type": "Token customization policy",
          "service_offer_id": "6c5a8f51-6259-4819-8d18-919b19f7a2e4",
          "service_offer_name": "SGX Attestation",
          "creator_id": "00000000-0000-0000-0000-000000000000",
          "updater_id": "00000000-0000-0000-0000-000000000000",
          "deleted": false,
          "created_time": "2022-10-06T23:42:59.535117Z",
          "modified_time": "2022-10-06T23:42:59.535117Z",
          "policy_hash": "/jVF9bCseJqqhFqWHAK7miMjhun1AcP92G0vywxEnW9PFfMeDhMbsJUJLLDHt7AJ",
          "policy_signature": "FqIm1m6YzZNbdXXHeDiBvFI8/oLgy8z7NeRfodRTMPu+r6I7IRd6KHL0W7+4wkKjWtG/HUmifQmDl1YUzicHgY3RjJcej7yXo1U2oYXlbYchrdPu4tRyLGW+YvZJv7wE/xB95IDIRRTR5d+CfZkL7/rDB6y4b1QBmO4x9FF0GkGITrdJJFXXFtLO+BCZqEcmavC9iqJns8QFjo03rO3fmTpUpr2kkN7kl35o0tD4nBlEZYqi2KYsgzvTh5StK7/2rQ3Fb/AcLEe3b84SJM6wEqoMG91z1XV52Wr5SI1zm2CAGeOb239pnLKKLFajvBRAK66FQ53wRYvRzC9jAPvzMOiRplsh3Gw5dHpaRZTjkpveH5X6fkZ24bgzZPeJYKsqI4J/GWaiMfN9BzkNfXIW73mY4ogVU2qIOCnTa6p2zucTusPrYmfTQn0bCFYpAZVgWOhvS9Nqt0r4OSfU7SXdvvPADY2ac0Yt5qDDQc6ypN3acIwKbsav/NKHfGPjR47K"
        }
      ]
```

## List policy by ID

This command retrieves a specific Project Amber policy by `policy ID`.

```bash
tenantctl list policy -a < api key > -p < policy id >
```

### Sample call

```bash
tenantctl list policy -a 678w77g678359021fg91d4b8thm6qz94 -p e6g83s7k-l82n-3o6z-537w-g4gt35khjfd4
```

### Sample response

```bash
{
  "policy_id": "c3c77a2d-f71c-4e1c-96b5-c6fc29fecee2",
  "policy": "default matches_sgx_policy = false \n\n\n matches_sgx_policy = true {   \n input.amber_sgx_mrenclave == \"30d4e819861adef6ffb2a4865efea9337b91ed30fa33491b17f0d5d9e8204410\" \n input.amber_sgx_mrsigner == \"83d719e77deaca1470f6baf62a4d774303c899db69020f9c70ee1dfc08c7ce9e\" \n input.amber_sgx_is_debuggable == false } ",
  "policy_name": "Policy935",
  "policy_type": "Appraisal policy",
  "service_offer_id": "80898b5f-f8e3-4240-a6ad-8cbe72f23110",
  "service_offer_name": "SGX Attestation",
  "creator_id": "00000000-0000-0000-0000-000000000000",
  "updater_id": "00000000-0000-0000-0000-000000000000",
  "deleted": false,
  "created_time": "2022-10-14T01:19:27.987355Z",
  "modified_time": "2022-10-14T01:19:27.987355Z",
  "policy_hash": "ekbxp/xHnmxD/R3wojOcl5m1t1c6CV7xGDhnQGs0vEKyv3jyLhC1s4l8zXnjWT9o",
  "policy_signature": "SCkuDxFFjB4KTEEWssdwVjjurK4Y+8tbT4SKSNWJO816mJyndG+UU8sMlsPTWXncG53vSa6sF1zh/L86lHzMQXwcRobQWFjTWmjNqYzV+apUilPLGH/ksXw8iEmAh8+cZUzdv/l4O6lO1J6lXdkFI65QIUsbDk6Nr3SEOjX/Ord3ShoTZvEYf7/Vp+U5+IBaxEu/NVWnjlFkpUwa6UGiC6b9L6vVo11jHY7SJs/weeSTYKvlT/o7TVHHDoZMUZPAFaZt6VoQuQ/WBBSU31T+tq1ZUMQLdt5YzyLaC418ueuROVkwrM8DMs8+69f+dpvZ53udSu9bnOKhm8erYnCHecoukZANLZRR9P1lRA6ZXUPcfZSm+WwGR/28PX16KcMyVpYWYrUvmHcTFzPy8aK0a+yFaNRbXdsl/YrCNfChzT9Fdg2SY0QeuHIrW5DRXcZAMZa8VL0jDULtVndxSlnPCBGueV/i0g8TqdDs8N6u4LhhvddyDEnB8Y998FFjXdJc"
}
```

## Delete policy  

This command deletes a specific policy.

```bash
tenantctl delete policy -a < api key > -p < policy id >
```

### Sample call

```bash
tenantctl delete policy -a 678w77g678359021fg91d4b8thm6qz94 -p e6g83s7k-l82n-3o6z-537w-g4gt35khjfd4
```

### Sample response

```bash
Policy 41677eb9-023d-402d-9966-909cdfff0889 deleted
```

## Sample policy for `policy update` command  

```bash
      {
      "policy_id": "e48dabc5-9608-4ff3-aaed-f25909ab9de1",
      "policy": "default matches_sgx_policy = false \n\n matches_sgx_policy = true { \n  input.amber_tee_is_debuggable == false \n input.amber_sgx_isvsvn == 0 \n input. amber_sgx_isvprodid == 0 \n input.amber_sgx_mrsigner ==   \"d412a4f07ef83892a5915fb2ab584be31e186e5a4f95ab5f6950fd4eb8694d7b\" \n  \n input. amber_sgx_mrenclave ==  \"bab91f200038076ac25f87de0ca67472443c2ebe17ed9ba95314e609038f51ab\" \n }",
      "user_id": "f04971b7-fb41-4a9e-a06e-4bf6e71f98b3",
      "policy_name": "Sample_Policy_SGX",
      "policy_type": "Appraisal policy",
      "service_offer_name": "SGX Attestation",
      "service_offer_id": "b04971b7-fb41-4a9e-a06e-4bf6e71f98bd"
      }
```      