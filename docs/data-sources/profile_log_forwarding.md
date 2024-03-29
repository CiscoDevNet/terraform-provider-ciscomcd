# DataSource: ciscomcd_profile_log_forwarding
Data source for obtaining attributes of a Log Forwarding Profile resource.  The attributes can be used in the arguments of a Gateway resource.

## Example Usage
```hcl
data "ciscomcd_profile_log_forwarding" "datadog" {
  name = "datadog"
}

resource "ciscomcd_gateway" "aws_hub_gw1" {
  name                  = "aws-hub-gw1"
  description           = "AWS Gateway 1"
  csp_account_name      = ciscomcd_cloud_account.aws_act.name
  instance_type         = "AWS_M5_2XLARGE"
  gateway_image         = var.gateway_image
  gateway_state         = "ACTIVE"
  mode                  = "HUB"
  security_type         = "EGRESS"
  policy_rule_set_id    = ciscomcd_policy_rule_set.egress_policy_rule_set.id
  ssh_key_pair          = "ssh_keypair1"
  aws_iam_role_firewall = "iam_role_name_for_firewall"
  region                = "us-east-1"
  vpc_id                = ciscomcd_service_vpc.service_vpc.id
  log_profile           = data.ciscomcd_profile_log_forwarding.datadog.id
  aws_gateway_lb        = true
}
```

## Argument Reference
* `name` - (Required) Name of the Log Forwarding Profile resource

## Attributes Reference
* `id` - ID of the Log Forwarding Profile resource