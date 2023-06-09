# Resource: ciscomcd_spoke_vpc
Resource for creating and managing Spoke VPC protection.

In AWS, the Spoke VPC is attached to the Transit Gateway and appropriate routing is setup in Transit Gateway route tables, so the traffic reaches the Multicloud Defense Service VPC. (Customer has to set the default route in the Spoke VPC routing tables to point to the Transit Gateway).

In Azure, VNet peering is setup between the Service VNet and the Spoke VNet.

In GCP, VPC peering is setup between the Service VPC and the Spoke VPC.

## Example Usage

### AWS Spoke VPC (Service VPC and Spoke VPC are in the same AWS Account)
```hcl
resource "ciscomcd_spoke_vpc" "ciscomcd_spoke" {
  service_vpc_id = ciscomcd_service_vpc.service_vpc.id
  spoke_vpc_id   = "vpc-12345678912345678"
}
```

### AWS Spoke VPC (Service VPC and Spoke VPC are in different AWS Accounts)
```hcl
resource "ciscomcd_spoke_vpc" "ciscomcd_spoke" {
  service_vpc_id             = ciscomcd_service_vpc.service_vpc.id
  spoke_vpc_id               = "vpc-12345678912345678"
  spoke_vpc_csp_account_name = "ciscomcd-csp-account2-name"
  spoke_vpc_region           = "us-west-2"
}
```

### Azure Spoke VNet
```hcl
resource "ciscomcd_spoke_vpc" "ciscomcd_spoke" {
  service_vpc_id = ciscomcd_service_vpc.service_vpc.id
  spoke_vpc_id   = "/subscriptions/subid/resourceGroups/rgname/providers/Microsoft.Network/virtualNetworks/spoke1"
}
```

### GCP Spoke VPC (Service VPC and Spoke VPC are in the same GCP Project)
```hcl
resource "ciscomcd_spoke_vpc" "ciscomcd_spoke" {
  service_vpc_id = ciscomcd_service_vpc.service_vpc.id
  spoke_vpc_id   = "https://www.googleapis.com/compute/v1/projects/project-004-281801/global/networks/gcp-auto-spoke3"
}
```

### GCP Spoke VPC (Service VPC and Spoke VPC are in different GCP Projects)
```hcl
resource "ciscomcd_spoke_vpc" "ciscomcd_spoke" {
  service_vpc_id             = ciscomcd_service_vpc.service_vpc.id
  spoke_vpc_id               = "https://www.googleapis.com/compute/v1/projects/project-003-281801/global/networks/gcp-auto-spoke6"
  spoke_vpc_csp_account_name = "gcp-account-1"
}
```

## Argument Reference
* `service_vpc_id` - (Required) Service VPC/VNet ID
* `spoke_vpc_id` - (Required) Spoke VPC/VNet ID
* `spoke_vpc_csp_account_name` - (Optional - AWS, GCP) Multicloud Defense CSP Account Name where the Spoke VPC resides (if different from the Multicloud Defense CSP Account Name in which the Service VPC resides)
* `spoke_vpc_region` - (Optional - AWS) Region where the spoke vpc exists (if different from the CSP account name in which Service VPC is created)
* `transit_gateway_attachment_subnets` - (Optional - AWS) List of Subnet Ids used to attach to the Transit Gateway. By default, a random subnet in each availability zone is selected

## Attribute Reference
* `id` - ID of the Spoke VPC resource

## Import
Spoke VPC resources can be imported using the resource `id`:

```hcl
$ terraform import ciscomcd_spoke_vpc.ciscomcd_spoke 10
```
