# Overview

Multicloud Defense provider is used to create and manage Multicloud Defense resources such as Service VPCs/VNets, Gateways, Policy Rulesets, Address Objects, Service Objects, etc.

Use the navigation on the left to read about each of the available resources.

## Authentication using Multicloud Defense API Key

To create Multicloud Defense resources using the Provider you need to authenticate with the Multicloud Defense Controller using an API Key generated within the Multicloud Defense Portal. The key can be generated only by a user with Role set to `admin_super` or `admin_rw`.  The Role assigned to the key will be specified when creating the key.

## Generate Key

1. Login to the Multicloud Defense Portal with your username and password
1. Navigate to **ADMINISTRATION**.
1. Under **MANAGEMENT** click **API Keys**
1. Click **Create API Key** button
1. Provide a **Name**
1. Assign the **Role** as `admin_rw` (required since Terraform will create resources)
1. Specify the **API Key Lifetime (days)** (max of 365 days) if you prefer the key to expire in less than 365 days
1. Click **Save**
1. Once the key is created, click **Download** to download the key file
1. *NOTE: API Key and Secret are not used for terraform*
1. Use the downloaded key file while initializing the Multicloud Defense Provider

## Using Key

```hcl
terraform {
  required_providers {
    ciscomcd = {
      source = "CiscoDevNet/terraform-provider-ciscomcd"
      version = "0.2.3"
    }
  }
}

provider "ciscomcd" {
  api_key_file = file(var.ciscomcd_api_key_file)
}
```

Contact Cisco Support at tac@cisco.com for information on the Multicloud Defense Tenant assigned to you.  Or visit the [Cisco Worldwide Support Contacts](https://www.cisco.com/c/en/us/support/web/tsd-cisco-worldwide-contacts.html) page for additional ways to contact Cisco Support.