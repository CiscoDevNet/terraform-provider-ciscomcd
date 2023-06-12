# AWS Multicloud Defense IAM
In order for Multicloud Defense to discover inventory and traffic, create Service VPCs and Spoke VPC protection, and deploy Multicloud Defense Gateways, the AWS Accounts need to be prepared with a set of IAM Roles and other configurations to grant the Multicloud Defense Controller the necessary permissions.  Multicloud Defense has an AWS Cloud Formation template that can be used to prepare the Accounts.  Terraform can also be used if the desire is to orchestrate the Accounts preparation and onboarding.  This is useful in order to automate the deployment of IAM Roles across a large number of AWS Accounts.

The module can be found at [Terraform AWS Setup](https://github.com/valtix-security/terraform-aws-setup)
