# Resource: ciscomcd_vpn_connection_set
Resource for creating and managing a VPN Connection Set resource

## Example Usage
```hcl
resource "ciscomcd_vpn_connection_set" "vpn_connection_set1" {
	name = "vpn_connection_set1"
}
```

## Argument Reference
* `name` - (Required) Name of the VPN Connection Set

## Attribute Reference
* `id` - ID of the VPN Connection Set resource that can be referenced in other resources (e.g., *ciscomcd_gateway*, *ciscomcd_vpn_connections*)

## Import
VPN Connection Set resources can be imported using the resource `id`:

```hcl
$ terraform import ciscomcd_vpn_connection_set.vpn_connection_set1 10
```