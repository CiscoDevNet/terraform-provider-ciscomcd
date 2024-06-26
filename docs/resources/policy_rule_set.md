# Resource: ciscomcd_policy_rule_set

Resource for creating and managing a Policy Rule Set.  A Policy Rule Set is a list of Policy Rules that define the segmentation and security policy for protecting traffic.  The Policy Rule Set can be applied to multiple Gateways across multiple clouds to accommodate a dynamic multi-cloud policy.

## Example Usage

### Standalone Policy Rule Set
```hcl
resource "ciscomcd_policy_rule_set" "ingress_policy_standalone" {
  name = "ingress_rule_set_standalone"
}
```

### Group Policy Rule Set
```hcl
resource "ciscomcd_policy_rule_set" "ingress_policy_group" {
  name               = "ingress_rule_set_group"
  type               = "GROUP"
  group_member_ids = [
    ciscomcd_policy_rule_set.ingress_policy1.id,
    ciscomcd_policy_rule_set.ingress_policy3.id,
    ciscomcd_policy_rule_set.ingress_policy2.id
  ]
}
```

## Argument Reference
* `name` - (Required) Name of the Policy Rule Set
* `description` - (Optional) Description of the Policy Rule Set
* `type` - (Optional) Specifies whether the Policy Rule Set is a Group Policy Rule Set.  The only applicable value is `GROUP`.  If not specified, the Policy Rule Set operates as a Standalone (non-Group) Policy Rule Set.
* `group_member_ids` - (Required - Group). Ordered list of Policy Ruleset (Standalone) IDs that make up the components of the Policy Rulset (Group).  This argument only applies when `type` is set to `GROUP`.  The list can contain one or more IDs and is limited to a maximum of 100 IDs.

## Attribute Reference
* `id` - ID of the Policy Rule Set resource that can be referenced in other resources (e.g., *ciscomcd_policy_rules*, *ciscomcd_policy_rule_set*)

## Import
Policy Rule Set resources can be imported using the resource `id`:

```hcl
$ terraform import ciscomcd_policy_rule_set.ingress_policy_standalone 10
```