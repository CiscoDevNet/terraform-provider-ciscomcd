# DataSource: ciscomcd_profile_dlp
Data source for obtaining attributes of a Data Loss Prevention (DLP) Profile resource.  The attributes can be used in the arguments of a Policy Rules resource.

## Example Usage
```hcl
data "ciscomcd_profile_dlp" "dlp1" {
  name = "dlp1"
}

resource "ciscomcd_policy_rules" "egress_policy_rules" {
	rule_set_id = ciscomcd_policy_rule_set.egress_policy.id
	rule {
		name        = "any-to-any"
		type        = "ForwardProxy"
		state       = "ENABLED"
		action      = "Allow Log"
		service     = ciscomcd_service_object.tcp_443.id
    dlp_profile = data.ciscomcd_profile_dlp.dlp1.id
	}
}
```

## Argument Reference
* `name` - (Required) Name of the Data Loss Prevention (DLP) IP Profile resource

## Attributes Reference
* `id` - ID of the Data Loss Prevention (DLP) Profile resource
