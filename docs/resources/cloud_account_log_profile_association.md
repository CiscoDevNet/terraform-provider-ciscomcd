# ciscomcd_cloud_account_log_profile_association
Resource for associating a Log Forwarding Profile with a CSP Account for forwarding Discovery Logs

~> **Note on ciscomcd_cloud_account_log_profile_association resource creation**
The [`ciscomcd_cloud_account`](../ciscomcd_cloud_account) and [`ciscomcd_profile_log_forwarding`](../ciscomcd_profile_log_forwarding)
resources must be created before the `ciscomcd_cloud_account_log_profile_association` resource can be created

## Example Usage
```hcl
resource "ciscomcd_cloud_account_log_profile_association" "csp_log_association" {
  csp_account_name = ciscomcd_cloud_account.aws_account1.name
  log_profile_id   = ciscomcd_profile_log_forwarding.s3bucket.id
}
```

## Argument Reference
* `csp_account_name` - (Required) The name of the CSP account onboarded into Multicloud Defense
* `log_profile_id` - (Required) The ID of the Log Forwarding Profile
