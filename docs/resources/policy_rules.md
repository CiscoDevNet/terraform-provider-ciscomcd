# Resource: ciscomcd_policy_rules
Resource for creating and managing the segmentation and security Policy Rules associated with a Policy Rule Set.

## Example Usage

### Ingress Policy Rules
```hcl
resource "ciscomcd_policy_rule_set" "ingress_policy" {
  name = "ingress-policy-ruleset"
}

resource "ciscomcd_policy_rules" "ingress_policy_rules" {
  rule_set_id = ciscomcd_policy_rule_set.ingress_policy.id
  rule {
    name        = "tcp-80"
    description = "listen port 80 to target port 80 on app1"
    type        = "ReverseProxy"
    action      = "Allow Log"
    service     = ciscomcd_service_object.app1-svc-http.id
    state       = "DISABLED"
  }
  rule {
    name        = "tcp-443"
    description = "listen port 443 to target port 443 on app1"
    type        = "ReverseProxy"
    action      = "Allow Log"
    service     = ciscomcd_service_object.app1-svc-https.id
    state       = "ENABLED"
  }
}
```

### Egress/East-West Policy Rules
```hcl
resource "ciscomcd_policy_rule_set" "egress_ew_policy" {
  name = "egress-ew-policy-ruleset"
}

data "ciscomcd_address_object" "any_ag" {
  name = "any"
}

data "ciscomcd_address_object" "any_private_rfc1918_ag" {
  name = "any-private-rfc1918"
}

data "ciscomcd_address_object" "internet_ag" {
  name = "internet"
}

resource "ciscomcd_policy_rules" "egress-ew-policy-rules" {
	rule_set_id = ciscomcd_policy_rule_set.egress_ew_policy.id
	rule {
		name        = "private-to-private"
		action      = "Allow Log"
		state       = "ENABLED"
		service     = ciscomcd_service_object.tcp_all_ports.id
		source      = data.ciscomcd_address_object.any_private_rfc1918_ag.id
		destination = data.ciscomcd_address_object.any_private_rfc1918_ag.id
		type        = "Forwarding"
	}
	rule {
		name        = "private-to-internet"
		action      = "Allow Log"
		state       = "ENABLED"
		service     = ciscomcd_service_object.tcp_all_ports.id
		source      = data.ciscomcd_address_object.any_private_rfc1918_ag.id
		destination = data.ciscomcd_address_object.internet_ag.id
		type        = "Forwarding"
	}
	rule {
		name        = "any-to-any"
		action      = "Deny Log"
		state       = "ENABLED"
		service     = ciscomcd_service_object.tcp_all_ports.id
		source      = data.ciscomcd_address_object.any_ag.id
		destination = data.ciscomcd_address_object.any_ag.id
		type        = "Forwarding"
	}
}
```

## Argument Reference
* `rule_set_id` - (Required) ID of the Policy Rule Set. (e.g., *ciscomcd_policy_rule_set.egress_ew_policy.id*).
* `rule` - A set of Rule blocks that define the segmentation and security Policy.  Each Rule is represented by a separate block.  Zero or more blocks can be specified, with a maximum of 2047 blocks. Structure is [documented below](#rule).

### Rule
* `name` - (Required) Name of the Rule
* `description` - (Optional) Description of the Rule
* `state` - (Required) `ENABLED` or `DISABLED`. Specifies whether the rule is enabled or disabled.
* `type` - (Required) `ReverseProxy`, `ForwardProxy`, and `Forwarding`.  Specifies how traffic will be processed by the Rule.
* `source` - (Optional) ID of the Address Object (ciscomcd_address_object). If not specified, the default value is the ID of the `any` Address Object.  To obtain the ID of the pre-defined Address Objects (`any`, `internet`, `any-private-rfc1918`), use the Address Object Data Source (see example usage above). (e.g., *ciscomcd_address_object.any_ag.id*).
* `destination` - (Optional) ID of the Address Object (ciscomcd_address_object). If not specified, the default value is the ID of the `any` Address Object.  To obtain the ID of the pre-defined Address Objects (`any`, `internet`, `any-private-rfc1918`), use the Address Object Data Source (see example usage above). (e.g., *ciscomcd_address_object.internet_ag.id*).
* `fqdn_match_profile` - (Optional) ID of the FQDN Match Profile
* `service` - (Required) ID of the Service Object (ciscomcd_service_object). The Service Object `service_type` argument must match the Rule `type` argument. (e.g., *ciscomcd_service_object.tcp_all_ports.id*).
* `action` - (Required) Action to take when traffic matches the Rule. Applicable values: `Allow Log` (log the event), `Allow No Log` (do not log the event), `Deny Log` (log the event), `Deny No Log` (do not log the event). Action names were changed in release 22.06 of the Multicloud Defense Terraform Provider. The previous action names (`ALLOW` `ALLOW_LOG`, `DENY`, `DENY_LOG`) are still valid, but Terraform will interpret the previous values as configuration changes until the new values are used.
* `send_deny_reset` - (Optional) Specifies whether a TCP Reset packet is sent by the Gateway when the session traffic is denied. Applies only if the Rule `action` or FQDN Filter Profile `policy` is set to `Deny Log` or `Deny No Log`, and if the Rule `type` is set to `Forwarding`. Applicable values: `true` (send a TCP Reset) or `false` (do not send a TCP Reset).  If not specified, the default value is `false`.
* `network_intrusion_profile` - (Optional) ID of the IDS/IPS Profile (ciscomcd_profile_network_intrusion). (e.g., *ciscomcd_profile_network_intrusion.ips1.id*).
* `dlp_profile` - (Optional) ID of the DLP Profile (ciscomcd_profile_dlp). (e.g., *ciscomcd_profile_dlp.dlp1.id*).
* `web_protection_profile` - (Optional) ID of the WAF Profile (ciscomcd_profile_web_protection). (e.g., *ciscomcd_profile_web_protection.waf1.id*).
* `fqdn_filter_profile` - (Optional) ID of the FQDN Filter Profile (ciscomcd_profile_fqdn_filter). (e.g., *ciscomcd_profile_fqdn_filter.waf1.id*).
* `anti_malware_profile` - (Optional) ID of the AM Profile (ciscomcd_profile_anti_malware). (e.g., *ciscomcd_profile_anti_malware.waf1.id*).
* `url_filter` - (Optional) ID of the URL Filter Profile (ciscomcd_profile_url_filter). (e.g., *ciscomcd_profile_url_filter.url1.id*).
* `malicious_ip_profile` - (Optional) ID of the Malicious IP Profile (ciscomcd_profile_malicious_ip_profile). (e.g., *ciscomcd_profile_malicious_ip_profile.mip1.id*).
* `packet_capture_enabled` - (Optional) `true` or `false`. Specifies the Packet Capture Profile to enable packet capture for each session that is processed by the Rule.  If not specified, the default value is `false`.

## Import
Policy Rules resources can be imported using the Policy Rule Set resource `id`:

```hcl
$ terraform import ciscomcd_policy_rules.ingress_policy_standalone 10
```