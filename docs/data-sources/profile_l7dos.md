# DataSource: ciscomcd_profile_l7dos
Data source for obtaining attributes of a L7DOS Profile resource.  The attributes can be used in the arguments of a Service Object resource.

## Example Usage
```hcl
data "ciscomcd_profile_l7dos" "l7dos1" {
  name = "l7dos1"
}

resource "ciscomcd_service_object" "app1_svc_https" {
  name           = "app1-svc-https"
  description    = "app1 service: listen on TCP/443 and proxy to HTTPS/443"
  service_type   = "ReverseProxy"
  protocol       = "TCP"
  transport_mode = "HTTPS"
  port {
    destination_ports = "443"
    backend_ports     = "443"
  }
  backend_address_group = ciscomcd_address_object.app1-ag.id
  tls_profile           = ciscomcd_profile_decryption.decryption_profile_1.id
  l7dos_profile         = data.ciscomcd_profile_l7dos.l7dos1.id
}
```

## Argument Reference
* `name` - (Required) Name of the L7DOS Profile resource

## Attributes Reference
* `id` - ID of the Network L7DOS Profile resource
