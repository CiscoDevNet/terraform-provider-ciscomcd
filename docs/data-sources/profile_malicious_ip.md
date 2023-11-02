# DataSource: ciscomcd_profile_malicious_ip
Data source for obtaining attributes of a Malicious IP Profile resource.  The attributes can be used in the arguments of a Policy Rules resource.

## Example Usage
```hcl
data "ciscomcd_profile_malicious_ip" "mips1" {
  name = "mips1"
}

resource "ciscomcd_policy_rules" "egress_policy_rules" {
	rule_set_id = ciscomcd_policy_rule_set.egress_policy.id
	rule {
		name                 = "any-to-any"
		type                 = "ForwardProxy"
		state                = "ENABLED"
		action               = "Allow Log"
		service              = ciscomcd_service_object.tcp_443.id
    malicious_ip_profile = data.ciscomcd_profile_malicious_ip.mips1.id
	}
}
```

## Argument Reference
* `name` - (Required) Name of the Malicious IP Profile resource

## Attributes Reference
* `id` - ID of the Malicious IP Profile resource