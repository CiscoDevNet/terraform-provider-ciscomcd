# DataSource: ciscomcd_profile_application_threat
Data source for obtaining attributes of a Application Threat (WAF) Profile resource.  The attributes can be used in the arguments of a Policy Rules resource.

## Example Usage
```hcl
data "ciscomcd_profile_application_threat" "waf1" {
  name = "waf1"
}

resource "ciscomcd_policy_rules" "ingress_policy_rules" {
  rule_set_id = ciscomcd_policy_rule_set.ingress_policy.id
  rule {
    name                   = "tcp-443"
    type                   = "ReverseProxy"
    state                  = "ENABLED"
    action                 = "Allow Log"
    service                = ciscomcd_service_object.app1-svc-https.id
    web_protection_profile = data.ciscomcd_profile_application_threat.waf1.id
  }
}
```

## Argument Reference
* `name` - (Required) Name of the Application Threat (WAF) Profile resource

## Attributes Reference
* `id` - ID of the Application Threat (WAF) Profile resource
