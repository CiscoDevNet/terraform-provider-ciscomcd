# Resource: ciscomcd_profile_packet_capture
Resource for creating and managing a Packet Capture (PCAP) Profile

## Example Usage

### AWS
```hcl
resource "ciscomcd_profile_packet_capture" "awspcap1" {
  name                = "awspcap1"
  description         = "pcap description"
  csp_account         = "csp account name added via ciscomcd_cloud_account"
  storage_upload_path = "s3-bucket-name"
}
```

### Azure
```hcl
resource "ciscomcd_profile_packet_capture" "azurepcap1" {
  name                      = "azurepcap1"
  description               = "pcap description"
  csp_account               = "csp account name added via ciscomcd_cloud_account"
  azure_storage_accnt_name  = "storage1"
  azure_blob_container_name = "ctr1"
  azure_storage_access_key  = "key1"
}
```

### GCP
```hcl
resource "ciscomcd_profile_packet_capture" "gcppcap1" {
  name                = "gcppcap1"
  description         = "pcap description"
  csp_account         = "csp account name added via ciscomcd_cloud_account"
  storage_upload_path = "gcp-storage-bucket-name"
}
```

## Argument Reference
* `name` - (Required) Name of the Packet Capture Profile
* `description` - (Optional) Description of the Packet Capture Profile
* `csp_account` - (Required) Cloud account name added to the Controller (ciscomcd_cloud_account.name)
* `storage_upload_path` - (Required - AWS and GCP) Storage bucket name
* `azure_storage_accnt_name` - (Required - Azure) Storage account name
* `azure_blob_container_name` - (Required - Azure) Storage container name
* `azure_storage_access_key` - (Optional - Azure) Storage account access key, if required

## Attribute Reference
* `id` - ID of the Packet Capture Profile resource that can be referenced in other resources (e.g., *ciscomcd_gateway*)

## Import
Packet Capture (PCAP) Profile resources can be imported using the resource `id`:

```hcl
$ terraform import ciscomcd_profile_packet_capture.awspcap1 10
```