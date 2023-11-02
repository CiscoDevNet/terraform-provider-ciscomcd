# DataSource: ciscomcd_profile_urlfilter
Data source for obtaining attributes of a URL Filtering Profile resource.  The attributes can be used in the arguments of a Policy Rules resource.

## Example Usage
```hcl
data "ciscomcd_profile_urlfilter" "url1" {
  name = "url1"
}

resource "ciscomcd_policy_rules" "egress_policy_rules" {
	rule_set_id = ciscomcd_policy_rule_set.egress_policy.id
	rule {
		name       = "any-to-any"
		type       = "ForwardProxy"
		state      = "ENABLED"
		action     = "Allow Log"
		service    = ciscomcd_service_object.tcp_443.id
    url_filter = data.ciscomcd_profile_urlfilter.url1.id
	}
}
```

## Argument Reference
* `name` - (Required) Name of the URL Filtering Profile resource

## Attributes Reference
* `id` - ID of the URL Filtering Profile resource