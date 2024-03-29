# Resource: ciscomcd_certificate
Resource for creating and managing Certificates used in Decryption Profiles

## Example Usage

### Import a certificate and key that's on your local file system
```hcl
resource "ciscomcd_certificate" "import_certificate" {
  name              = "import_certificate"
  certificate_type  = "IMPORT_CONTENTS"
  certificate_body  = file("cert.pem")
  private_key       = file("key.pem")
  // body and key values can also be the strings
}
```

### Import a certificate from AWS Secrets Manager
```hcl
resource "ciscomcd_certificate" "aws_certificate_secret" {
  name             = "aws_certificate_secret"
  certificate_type = "AWS_SECRET"
  certificate_body = file("cert.pem")
  csp_account_name = "aws_account"
  region           = "us-east-1"
  aws_secret_name  = "my_key1"
}
```

### Import a certificate from AWS KMS
```hcl
resource "ciscomcd_certificate" "aws_certificate_kms" {
  name                    = "aws_certificate_kms"
  certificate_type        = "AWS_KMS"
  certificate_body        = file("cert.pem")
  csp_account_name        = "aws_account"
  region                  = "us-east-1"
  private_key_cipher_text = file("key.pem")
}
```

### Import a certificate from Azure Key Vault and Secret
```hcl
resource "ciscomcd_certificate" "azure_certificate_keyvault" {
  name                        = "azure_certificate_keyvault"
  certificate_type            = "AZURE_KEY_VAULT_SECRET"
  certificate_body            = file("cert.pem")
  csp_account_name            = "azure_account"
  region                      = "eastus"
  azure_key_vault_name        = "vault1"
  azure_key_vault_secret_name = "secret1"
}
```

### Import a certificate from GCP Secrets Manager
```hcl
resource "ciscomcd_certificate" "gcp_certificate_secret" {
  name             = "gcp_certificate_secret"
  certificate_type = "GCP_SECRET"
  certificate_body = file("cert.pem")
  csp_account_name = "gcp_account"
  secret_name      = "projects/012345678910/secrets/gcp-certificate-secret"
  secret_version   = "12"
}
```

## Argument Reference
* `name` - (Required) Name of the certificate
* `description` - Description of the certificate
* `certificate_type` - (Required) ["IMPORT_CONTENTS"](#import_contents), ["AWS_KMS"](#aws_kms), ["AWS_SECRET"](#aws_secret), ["AZURE_KEY_VAULT_SECRET"](#azure_key_vault_secret), ["GCP_SECRET"](#gcp_secret)
* `certificate_body` - (Required) Certificate content. Provide a string with the entire content or if located in a file use `file("filename")`
* `certificate_chain` - (Optional) Certificate chain content. Provide a string with the entire content or if it is located in a file use `file("filename")`. This is optional if the certificate has a separate chain that is not part of the certificate body.

### IMPORT_CONTENTS
* `private_key` - (Required for IMPORT_CONTENTS) Certificate content. Provide a string with the entire content or if it is located in a file use `file("filename")`

### AWS_KMS
* `csp_account_name` - (Required) To use a private key that is stored in AWS Key Management System (KMS), provide the csp_account_name (AWS) where the private key will be stored.  This is the friendly name defined in Multicloud Defense for the on-boarded CSP account.
* `region` - (Required) Region where the private key will be stored
* `private_key_cipher_text` - (Required) AWS KMS encrypted cipher text

### AWS_SECRET
* `csp_account_name` - (Required) To use a private key stored in AWS Secrets Manager, provide the csp_account_name (AWS) where the private key will be stored.  This is the friendly name defined in Multicloud Defense for the on-boarded CSP account.
* `region` - (Required) Region where the private key will be stored
* `aws_secret_name` - (Required) AWS Secrets Manager key name that contains the private key

### AZURE_KEY_VAULT_SECRET
* `csp_account_name` - (Required) To use a private key stored in Azure Key Vault, provide the csp_account_name (Azure) where the private key will be stored.  This is the friendly name defined in Multicloud Defense for the on-boarded CSP account.
* `region` - (Required) Region where the private key will be stored
* `azure_key_vault_name` - (Required) Azure Key Vault name where the private key will be stored
* `azure_key_vault_secret_name` - (Required) Azure Key Vault secret name that contains the private key

### GCP_SECRET
* `csp_account_name` - (Required) To use a private key stored in GCP Secrets Manager, provide the csp_account_name (GCP) where the private key will be stored.  This is the friendly name defined in Multicloud Defense for the on-boarded CSP account.
* `secret_name` - (Required) GCP Secrets Manager key name that contains the private key. The value must be the full path of the key name (e.g., `projects/012345678910/secrets/gcp-certificate-secret`)
* `secret_version` - (Optional) GCP Secrets Manager key version

## Attribute Reference
* `id` - ID of the Certificate resource that can be referenced in other resources (e.g. ciscomcd_profile_decryption)

## Import
Certificate resources can be imported using the resource `name`:

```hcl
$ terraform import ciscomcd_certificate.import_certificate import_certificate
```