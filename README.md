# Terraform-aws-ec2

# Terraform AWS Cloud EC2 Module

## Table of Contents
- [Introduction](#introduction)
- [Usage](#usage)
- [Examples](#examples)
- [License](#license)
- [Author](#Author)
- [Inputs](#inputs)
- [Outputs](#outputs)

## Introduction
This Terraform module creates an AWS Elastic Compute Cloud (EC2) along with additional configuration options.
## Usage
To use this module, you should have Terraform installed and configured for AWS. This module provides the necessary Terraform configuration for creating AWS resources, and you can customize the inputs as needed. Below is an example of how to use this module:

# Examples

# Example: default

```hcl
# Create EC2 instances
module "ec2" {
  source               = "git::https://github.com/navneetbishnoi/terraform-aws-ec2.git?ref=v1.0.0"
  name                 = "ec2"
  environment          = local.environment
  vpc_id               = module.vpc.vpc_id
  ssh_allowed_ip       = ["0.0.0.0/0"]
  ssh_allowed_ports    = [22]
  instance_count       = 2
  ami                  = "ami-01dd271720c1ba44f"
  instance_type        = "t2.micro"
  public_key           = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDmPzqVYDVlaExxJBwbrXT2jG44xMM1U6Z+nCPkjStGj4ayo7Lkz8SzzFbAS0uYaXaTJfc/ZI980BwtLAeWZeaFYDABDROJHS748cTwyByUe1S+yNMrz7wbyeZllQGt6EtnxcOADuMvtBGioTgfXIzJNYb73TlPEUVlTOEDbQd+8oDpO+u7SJFgT+q5OZE5XFyYvp5hHSdLgZhXurRfjvFpqVBwiTojL5o0Q2xqXxTFOFfdoDKcjbMwFwr4vdJ5Edqqa2gcl9nRtCL4vo0m/St0ekbZ3yT9h3gRgP3+u9L0rc0f4XZxNW3b0ljWC1dEd/pAVw1k1y1xRnYKKwNaT6nZcKqFawT/G4S9fj6LrD+RPJsEgMXcIaAcGeidQolVZce4fWyAJc5Dx0ALKTkHNN7NyyTXopuK63YJ5lUEwWOYc6q9l/xM49i9hdpMD0TafqM0rWXFY3ALR9z/U0CMWwtlQ33iInGEYRqd+wupm48nuHII359uNe/GKhjqCLU5K4E= rahul@rahul"
  subnet_ids           = tolist(module.public_subnets.public_subnet_id)
  iam_instance_profile = module.iam-role.name

  root_block_device = [
    {
      volume_type           = "gp2"
      volume_size           = 15
      delete_on_termination = true
    }
  ]

  ebs_volume_enabled = true
  ebs_volume_type    = "gp2"
  ebs_volume_size    = 30

  instance_tags = { "snapshot" = true }

  #Mount EBS With User Data
  user_data = file("user-data.sh")
}
```

This example demonstrates how to create various AWS resources using the provided modules. Adjust the input values to suit your specific requirements.

# Example: spot_instance

```hcl
module "spot-ec2" {
  source      = "git::https://github.com/navneetbishnoi/terraform-aws-ec2.git?ref=v1.0.0"
  name        = "ec2"
  environment = "test"

  ##======================================================================================
  ## Below A security group controls the traffic that is allowed to reach and leave the resources that it is associated with.
  ##======================================================================================
  vpc_id            = module.vpc.vpc_id
  ssh_allowed_ip    = ["0.0.0.0/0"]
  ssh_allowed_ports = [22]

  #Keypair
  public_key = "ssh-ejllt6FE/X7jf/RubFCUm0zFeB77xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx+1nuUG57KoMORsZoHGptg7i/QXs32pqlxftTqEschCpitGuBN4NxwybES6FdkYLXFZYWiv7uuujVl"

  # Spot-instance
  spot_price                          = "0.3"
  spot_wait_for_fulfillment           = true
  spot_type                           = "persistent"
  spot_instance_interruption_behavior = "terminate"
  spot_instance_enabled               = true
  spot_instance_count                 = 1
  instance_type                       = "c4.xlarge"

  #Networking
  subnet_ids = tolist(module.public_subnets.public_subnet_id)

  #Root Volume
  root_block_device = [
    {
      volume_type           = "gp2"
      volume_size           = 15
      delete_on_termination = true
    }
  ]

  #EBS Volume
  ebs_volume_enabled = true
  ebs_volume_type    = "gp2"
  ebs_volume_size    = 30

  #Tags
  spot_instance_tags = { "snapshot" = true }
}
```

## Examples
For detailed examples on how to use this module, please refer to the [example](https://github.com/navneetbishnoi/terraform-aws-ec2/tree/main/example) directory within this repository.

## License
This Terraform module is provided under the **MIT** License. Please see the [LICENSE](https://github.com/navneetbishnoi/terraform-aws-ec2/blob/master/LICENSE) file for more details.

## Author
Your Name
Replace **MIT** and **navneetbishnoi** with the appropriate license and your information. Feel free to expand this README with additional details or usage instructions as needed for your specific use case.

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.8.5 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | 6.5.0 |
| <a name="requirement_tls"></a> [tls](#requirement\_tls) | 4.1.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | 6.5.0 |
| <a name="provider_tls"></a> [tls](#provider\_tls) | 4.1.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_labels"></a> [labels](#module\_labels) | git::https://github.com/navneetbishnoi/terraform-aws-labels.git | v1.0.0 |

## Resources

| Name | Type |
|------|------|
| [aws_ebs_volume.default](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/ebs_volume) | resource |
| [aws_eip.default](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/eip) | resource |
| [aws_iam_instance_profile.default](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/iam_instance_profile) | resource |
| [aws_instance.default](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/instance) | resource |
| [aws_key_pair.default](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/key_pair) | resource |
| [aws_kms_alias.default](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/kms_alias) | resource |
| [aws_kms_key.default](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/kms_key) | resource |
| [aws_route53_record.default](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/route53_record) | resource |
| [aws_security_group.default](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/security_group) | resource |
| [aws_security_group_rule.egress_ipv4](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.egress_ipv6](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.ingress](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.ssh_ingress](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/security_group_rule) | resource |
| [aws_spot_instance_request.default](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/spot_instance_request) | resource |
| [aws_volume_attachment.default](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/resources/volume_attachment) | resource |
| [tls_private_key.default](https://registry.terraform.io/providers/hashicorp/tls/4.1.0/docs/resources/private_key) | resource |
| [aws_ami.ubuntu](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/data-sources/ami) | data source |
| [aws_iam_policy_document.kms](https://registry.terraform.io/providers/hashicorp/aws/6.5.0/docs/data-sources/iam_policy_document) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_algorithm"></a> [algorithm](#input\_algorithm) | Name of the algorithm to use when generating the private key. Currently-supported values are: RSA, ECDSA, ED25519. | `string` | `"RSA"` | no |
| <a name="input_alias"></a> [alias](#input\_alias) | The display name of the alias. The name must start with the word `alias` followed by a forward slash. | `string` | `""` | no |
| <a name="input_allowed_ip"></a> [allowed\_ip](#input\_allowed\_ip) | List of allowed ip. | `list(any)` | <pre>[<br>  "0.0.0.0/0"<br>]</pre> | no |
| <a name="input_allowed_ports"></a> [allowed\_ports](#input\_allowed\_ports) | List of allowed ingress ports | `list(any)` | <pre>[<br>  80,<br>  443<br>]</pre> | no |
| <a name="input_ami"></a> [ami](#input\_ami) | The AMI to use for the instance. | `string` | `"ami-020cba7c55df1f615"` | no |
| <a name="input_assign_eip_address"></a> [assign\_eip\_address](#input\_assign\_eip\_address) | Assign an Elastic IP address to the instance. | `bool` | `true` | no |
| <a name="input_associate_public_ip_address"></a> [associate\_public\_ip\_address](#input\_associate\_public\_ip\_address) | Associate a public IP address with the instance. | `bool` | `true` | no |
| <a name="input_availability_zone"></a> [availability\_zone](#input\_availability\_zone) | AZ to start the instance in | `string` | `null` | no |
| <a name="input_capacity_reservation_specification"></a> [capacity\_reservation\_specification](#input\_capacity\_reservation\_specification) | Describes an instance's Capacity Reservation targeting option | `any` | `{}` | no |
| <a name="input_cpu_credits"></a> [cpu\_credits](#input\_cpu\_credits) | The credit option for CPU usage. Can be `standard` or `unlimited`. T3 instances are launched as unlimited by default. T2 instances are launched as standard by default. | `string` | `"standard"` | no |
| <a name="input_cpu_options"></a> [cpu\_options](#input\_cpu\_options) | Defines CPU options to apply to the instance at launch time. | `any` | `{}` | no |
| <a name="input_customer_master_key_spec"></a> [customer\_master\_key\_spec](#input\_customer\_master\_key\_spec) | Specifies whether the key contains a symmetric key or an asymmetric key pair and the encryption algorithms or signing algorithms that the key supports. Valid values: SYMMETRIC\_DEFAULT, RSA\_2048, RSA\_3072, RSA\_4096, ECC\_NIST\_P256, ECC\_NIST\_P384, ECC\_NIST\_P521, or ECC\_SECG\_P256K1. Defaults to SYMMETRIC\_DEFAULT. | `string` | `"SYMMETRIC_DEFAULT"` | no |
| <a name="input_default_instance_enabled"></a> [default\_instance\_enabled](#input\_default\_instance\_enabled) | Flag to control the instance creation. | `bool` | `true` | no |
| <a name="input_deletion_window_in_days"></a> [deletion\_window\_in\_days](#input\_deletion\_window\_in\_days) | Duration in days after which the key is deleted after destruction of the resource. | `number` | `7` | no |
| <a name="input_delimiter"></a> [delimiter](#input\_delimiter) | Delimiter to be used between `organization`, `environment`, `name` and `attributes`. | `string` | `"-"` | no |
| <a name="input_disable_api_termination"></a> [disable\_api\_termination](#input\_disable\_api\_termination) | If true, enables EC2 Instance Termination Protection. | `bool` | `false` | no |
| <a name="input_dns_enabled"></a> [dns\_enabled](#input\_dns\_enabled) | Flag to control the dns\_enable. | `bool` | `false` | no |
| <a name="input_dns_zone_id"></a> [dns\_zone\_id](#input\_dns\_zone\_id) | The Zone ID of Route53. | `string` | `"Z1XJD7SSBKXLC1"` | no |
| <a name="input_ebs_block_device"></a> [ebs\_block\_device](#input\_ebs\_block\_device) | Additional EBS block devices to attach to the instance | `list(any)` | `[]` | no |
| <a name="input_ebs_device_name"></a> [ebs\_device\_name](#input\_ebs\_device\_name) | Name of the EBS device to mount. | `list(string)` | <pre>[<br>  "/dev/xvdb",<br>  "/dev/xvdc",<br>  "/dev/xvdd",<br>  "/dev/xvde",<br>  "/dev/xvdf",<br>  "/dev/xvdg",<br>  "/dev/xvdh",<br>  "/dev/xvdi",<br>  "/dev/xvdj",<br>  "/dev/xvdk",<br>  "/dev/xvdl",<br>  "/dev/xvdm",<br>  "/dev/xvdn",<br>  "/dev/xvdo",<br>  "/dev/xvdp",<br>  "/dev/xvdq",<br>  "/dev/xvdr",<br>  "/dev/xvds",<br>  "/dev/xvdt",<br>  "/dev/xvdu",<br>  "/dev/xvdv",<br>  "/dev/xvdw",<br>  "/dev/xvdx",<br>  "/dev/xvdy",<br>  "/dev/xvdz"<br>]</pre> | no |
| <a name="input_ebs_iops"></a> [ebs\_iops](#input\_ebs\_iops) | Amount of provisioned IOPS. This must be set with a volume\_type of io1. | `number` | `3000` | no |
| <a name="input_ebs_optimized"></a> [ebs\_optimized](#input\_ebs\_optimized) | If true, the launched EC2 instance will be EBS-optimized. | `bool` | `false` | no |
| <a name="input_ebs_volume_enabled"></a> [ebs\_volume\_enabled](#input\_ebs\_volume\_enabled) | Flag to control the ebs creation. | `bool` | `false` | no |
| <a name="input_ebs_volume_size"></a> [ebs\_volume\_size](#input\_ebs\_volume\_size) | Size of the EBS volume in gigabytes. | `number` | `30` | no |
| <a name="input_ebs_volume_type"></a> [ebs\_volume\_type](#input\_ebs\_volume\_type) | The type of EBS volume. Can be standard, gp2 or io1. | `string` | `"gp2"` | no |
| <a name="input_egress_ipv4_cidr_block"></a> [egress\_ipv4\_cidr\_block](#input\_egress\_ipv4\_cidr\_block) | List of CIDR blocks. Cannot be specified with source\_security\_group\_id or self. | `list(string)` | <pre>[<br>  "0.0.0.0/0"<br>]</pre> | no |
| <a name="input_egress_ipv4_from_port"></a> [egress\_ipv4\_from\_port](#input\_egress\_ipv4\_from\_port) | Egress Start port (or ICMP type number if protocol is icmp or icmpv6). | `number` | `0` | no |
| <a name="input_egress_ipv4_protocol"></a> [egress\_ipv4\_protocol](#input\_egress\_ipv4\_protocol) | Protocol. If not icmp, icmpv6, tcp, udp, or all use the protocol number | `string` | `"-1"` | no |
| <a name="input_egress_ipv4_to_port"></a> [egress\_ipv4\_to\_port](#input\_egress\_ipv4\_to\_port) | Egress end port (or ICMP code if protocol is icmp). | `number` | `65535` | no |
| <a name="input_egress_ipv6_cidr_block"></a> [egress\_ipv6\_cidr\_block](#input\_egress\_ipv6\_cidr\_block) | List of CIDR blocks. Cannot be specified with source\_security\_group\_id or self. | `list(string)` | <pre>[<br>  "::/0"<br>]</pre> | no |
| <a name="input_egress_ipv6_from_port"></a> [egress\_ipv6\_from\_port](#input\_egress\_ipv6\_from\_port) | Egress Start port (or ICMP type number if protocol is icmp or icmpv6). | `number` | `0` | no |
| <a name="input_egress_ipv6_protocol"></a> [egress\_ipv6\_protocol](#input\_egress\_ipv6\_protocol) | Protocol. If not icmp, icmpv6, tcp, udp, or all use the protocol number | `string` | `"-1"` | no |
| <a name="input_egress_ipv6_to_port"></a> [egress\_ipv6\_to\_port](#input\_egress\_ipv6\_to\_port) | Egress end port (or ICMP code if protocol is icmp). | `number` | `65535` | no |
| <a name="input_egress_rule"></a> [egress\_rule](#input\_egress\_rule) | Enable to create egress rule | `bool` | `true` | no |
| <a name="input_enable"></a> [enable](#input\_enable) | Flag to control module creation. | `bool` | `true` | no |
| <a name="input_enable_key_pair"></a> [enable\_key\_pair](#input\_enable\_key\_pair) | A boolean flag to enable/disable key pair. | `bool` | `true` | no |
| <a name="input_enable_key_rotation"></a> [enable\_key\_rotation](#input\_enable\_key\_rotation) | Specifies whether key rotation is enabled. | `string` | `true` | no |
| <a name="input_enable_security_group"></a> [enable\_security\_group](#input\_enable\_security\_group) | Enable default Security Group with only Egress traffic allowed. | `bool` | `true` | no |
| <a name="input_enclave_options_enabled"></a> [enclave\_options\_enabled](#input\_enclave\_options\_enabled) | Whether Nitro Enclaves will be enabled on the instance. Defaults to `false` | `bool` | `null` | no |
| <a name="input_environment"></a> [environment](#input\_environment) | Environment (e.g. `prod`, `dev`, `staging`). | `string` | `""` | no |
| <a name="input_ephemeral_block_device"></a> [ephemeral\_block\_device](#input\_ephemeral\_block\_device) | Customize Ephemeral (also known as Instance Store) volumes on the instance. | `list(any)` | `[]` | no |
| <a name="input_get_password_data"></a> [get\_password\_data](#input\_get\_password\_data) | If true, wait for password data to become available and retrieve it | `bool` | `null` | no |
| <a name="input_hibernation"></a> [hibernation](#input\_hibernation) | hibernate an instance, Amazon EC2 signals the operating system to perform hibernation. | `bool` | `false` | no |
| <a name="input_host_id"></a> [host\_id](#input\_host\_id) | The Id of a dedicated host that the instance will be assigned to. Use when an instance is to be launched on a specific dedicated host. | `string` | `null` | no |
| <a name="input_hostname"></a> [hostname](#input\_hostname) | DNS records to create. | `string` | `"ec2"` | no |
| <a name="input_iam_instance_profile"></a> [iam\_instance\_profile](#input\_iam\_instance\_profile) | The IAM Instance Profile to launch the instance with. Specified as the name of the Instance Profile. | `string` | `""` | no |
| <a name="input_instance_count"></a> [instance\_count](#input\_instance\_count) | Number of instances to launch. | `number` | `1` | no |
| <a name="input_instance_initiated_shutdown_behavior"></a> [instance\_initiated\_shutdown\_behavior](#input\_instance\_initiated\_shutdown\_behavior) | n/a | `string` | `"terminate"` | no |
| <a name="input_instance_metadata_tags_enabled"></a> [instance\_metadata\_tags\_enabled](#input\_instance\_metadata\_tags\_enabled) | Whether the metadata tag is available. Valid values include enabled or disabled. Defaults to enabled. | `string` | `"disabled"` | no |
| <a name="input_instance_profile_enabled"></a> [instance\_profile\_enabled](#input\_instance\_profile\_enabled) | Flag to control the instance profile creation. | `bool` | `true` | no |
| <a name="input_instance_tags"></a> [instance\_tags](#input\_instance\_tags) | Instance tags. | `map(any)` | `{}` | no |
| <a name="input_instance_type"></a> [instance\_type](#input\_instance\_type) | The type of instance to start. Updates to this field will trigger a stop/start of the EC2 instance. | `string` | `"t2.micro"` | no |
| <a name="input_ipv6_address_count"></a> [ipv6\_address\_count](#input\_ipv6\_address\_count) | Number of IPv6 addresses to associate with the primary network interface. Amazon EC2 chooses the IPv6 addresses from the range of your subnet. | `number` | `null` | no |
| <a name="input_ipv6_addresses"></a> [ipv6\_addresses](#input\_ipv6\_addresses) | List of IPv6 addresses from the range of the subnet to associate with the primary network interface. | `list(any)` | `null` | no |
| <a name="input_is_enabled"></a> [is\_enabled](#input\_is\_enabled) | Specifies whether the key is enabled. | `bool` | `true` | no |
| <a name="input_is_external"></a> [is\_external](#input\_is\_external) | enable to udated existing security Group | `bool` | `false` | no |
| <a name="input_key_name"></a> [key\_name](#input\_key\_name) | Key name of the Key Pair to use for the instance; which can be managed using the aws\_key\_pair resource. | `string` | `""` | no |
| <a name="input_key_usage"></a> [key\_usage](#input\_key\_usage) | Specifies the intended use of the key. Defaults to ENCRYPT\_DECRYPT, and only symmetric encryption and decryption are supported. | `string` | `"ENCRYPT_DECRYPT"` | no |
| <a name="input_kms_description"></a> [kms\_description](#input\_kms\_description) | The description of the key as viewed in AWS console. | `string` | `"Parameter Store KMS master key"` | no |
| <a name="input_kms_key_enabled"></a> [kms\_key\_enabled](#input\_kms\_key\_enabled) | Specifies whether the kms is enabled or disabled. | `bool` | `true` | no |
| <a name="input_kms_key_id"></a> [kms\_key\_id](#input\_kms\_key\_id) | The ARN of the key that you wish to use if encrypting at rest. If not supplied, uses service managed encryption. Can be specified only if at\_rest\_encryption\_enabled = true. | `string` | `""` | no |
| <a name="input_kms_multi_region"></a> [kms\_multi\_region](#input\_kms\_multi\_region) | Indicates whether the KMS key is a multi-Region (true) or regional (false) key. | `bool` | `false` | no |
| <a name="input_label_order"></a> [label\_order](#input\_label\_order) | Label order, e.g. `name`,`application`. | `list(any)` | <pre>[<br>  "name",<br>  "environment"<br>]</pre> | no |
| <a name="input_launch_template"></a> [launch\_template](#input\_launch\_template) | Specifies a Launch Template to configure the instance. Parameters configured on this resource will override the corresponding parameters in the Launch Template | `map(string)` | `{}` | no |
| <a name="input_managedby"></a> [managedby](#input\_managedby) | n/a | `string` | `"navneetbishnoi"` | no |
| <a name="input_metadata_http_endpoint_enabled"></a> [metadata\_http\_endpoint\_enabled](#input\_metadata\_http\_endpoint\_enabled) | Whether the metadata service is available. Valid values include enabled or disabled. Defaults to enabled. | `string` | `"enabled"` | no |
| <a name="input_metadata_http_put_response_hop_limit"></a> [metadata\_http\_put\_response\_hop\_limit](#input\_metadata\_http\_put\_response\_hop\_limit) | The desired HTTP PUT response hop limit (between 1 and 64) for instance metadata requests. | `number` | `2` | no |
| <a name="input_metadata_http_tokens_required"></a> [metadata\_http\_tokens\_required](#input\_metadata\_http\_tokens\_required) | Whether or not the metadata service requires session tokens, also referred to as Instance Metadata Service Version 2 (IMDSv2). Valid values include optional or required. Defaults to optional. | `string` | `"optional"` | no |
| <a name="input_monitoring"></a> [monitoring](#input\_monitoring) | If true, the launched EC2 instance will have detailed monitoring enabled. (Available since v0.6.0). | `bool` | `false` | no |
| <a name="input_multi_attach_enabled"></a> [multi\_attach\_enabled](#input\_multi\_attach\_enabled) | Specifies whether to enable Amazon EBS Multi-Attach. Multi-Attach is supported on io1 and io2 volumes. | `bool` | `false` | no |
| <a name="input_name"></a> [name](#input\_name) | Name  (e.g. `app` or `cluster`). | `string` | `""` | no |
| <a name="input_network_interface"></a> [network\_interface](#input\_network\_interface) | Customize network interfaces to be attached at instance boot time | `list(map(string))` | `[]` | no |
| <a name="input_placement_group"></a> [placement\_group](#input\_placement\_group) | The Placement Group to start the instance in. | `string` | `""` | no |
| <a name="input_private_ip"></a> [private\_ip](#input\_private\_ip) | Private IP address to associate with the instance in a VPC | `string` | `null` | no |
| <a name="input_protocol"></a> [protocol](#input\_protocol) | The protocol. If not icmp, tcp, udp, or all use the. | `string` | `"tcp"` | no |
| <a name="input_public_key"></a> [public\_key](#input\_public\_key) | Name  (e.g. `ssh-rsa AAAAB3NzaC1ycAAADAQABAAABAQD3F6tyPEFEzV0LX3X8BsXdMsQ`). | `string` | `""` | no |
| <a name="input_repository"></a> [repository](#input\_repository) | Terraform current module repo | `string` | `""` | no |
| <a name="input_root_block_device"></a> [root\_block\_device](#input\_root\_block\_device) | Customize details about the root block device of the instance. See Block Devices below for details. | `list(any)` | `[]` | no |
| <a name="input_rsa_bits"></a> [rsa\_bits](#input\_rsa\_bits) | When algorithm is RSA, the size of the generated RSA key, in bits (default: 2048). | `number` | `4096` | no |
| <a name="input_secondary_private_ips"></a> [secondary\_private\_ips](#input\_secondary\_private\_ips) | A list of secondary private IPv4 addresses to assign to the instance's primary network interface (eth0) in a VPC. Can only be assigned to the primary network interface (eth0) attached at instance creation, not a pre-existing network interface i.e. referenced in a `network_interface block` | `list(string)` | `null` | no |
| <a name="input_sg_description"></a> [sg\_description](#input\_sg\_description) | The security group description. | `string` | `"Instance default security group (only egress access is allowed)."` | no |
| <a name="input_sg_egress_description"></a> [sg\_egress\_description](#input\_sg\_egress\_description) | Description of the egress and ingress rule | `string` | `"Description of the rule."` | no |
| <a name="input_sg_egress_ipv6_description"></a> [sg\_egress\_ipv6\_description](#input\_sg\_egress\_ipv6\_description) | Description of the egress\_ipv6 rule | `string` | `"Description of the rule."` | no |
| <a name="input_sg_ids"></a> [sg\_ids](#input\_sg\_ids) | of the security group id. | `list(any)` | `[]` | no |
| <a name="input_sg_ingress_description"></a> [sg\_ingress\_description](#input\_sg\_ingress\_description) | Description of the ingress rule | `string` | `"Description of the ingress rule use elasticache."` | no |
| <a name="input_source_dest_check"></a> [source\_dest\_check](#input\_source\_dest\_check) | Controls if traffic is routed to the instance when the destination address does not match the instance. Used for NAT or VPNs. | `bool` | `true` | no |
| <a name="input_spot_instance_count"></a> [spot\_instance\_count](#input\_spot\_instance\_count) | Number of instances to launch. | `number` | `0` | no |
| <a name="input_spot_instance_enabled"></a> [spot\_instance\_enabled](#input\_spot\_instance\_enabled) | Flag to control the instance creation. | `bool` | `true` | no |
| <a name="input_spot_instance_interruption_behavior"></a> [spot\_instance\_interruption\_behavior](#input\_spot\_instance\_interruption\_behavior) | Indicates Spot instance behavior when it is interrupted. Valid values are `terminate`, `stop`, or `hibernate` | `string` | `null` | no |
| <a name="input_spot_instance_tags"></a> [spot\_instance\_tags](#input\_spot\_instance\_tags) | Instance tags. | `map(any)` | `{}` | no |
| <a name="input_spot_launch_group"></a> [spot\_launch\_group](#input\_spot\_launch\_group) | A launch group is a group of spot instances that launch together and terminate together. If left empty instances are launched and terminated individually | `string` | `null` | no |
| <a name="input_spot_price"></a> [spot\_price](#input\_spot\_price) | The maximum price to request on the spot market. Defaults to on-demand price | `string` | `null` | no |
| <a name="input_spot_type"></a> [spot\_type](#input\_spot\_type) | If set to one-time, after the instance is terminated, the spot request will be closed. Default `persistent` | `string` | `null` | no |
| <a name="input_spot_valid_from"></a> [spot\_valid\_from](#input\_spot\_valid\_from) | The start date and time of the request, in UTC RFC3339 format(for example, YYYY-MM-DDTHH:MM:SSZ) | `string` | `null` | no |
| <a name="input_spot_valid_until"></a> [spot\_valid\_until](#input\_spot\_valid\_until) | The end date and time of the request, in UTC RFC3339 format(for example, YYYY-MM-DDTHH:MM:SSZ) | `string` | `null` | no |
| <a name="input_spot_wait_for_fulfillment"></a> [spot\_wait\_for\_fulfillment](#input\_spot\_wait\_for\_fulfillment) | If set, Terraform will wait for the Spot Request to be fulfilled, and will throw an error if the timeout of 10m is reached | `bool` | `false` | no |
| <a name="input_ssh_allowed_ip"></a> [ssh\_allowed\_ip](#input\_ssh\_allowed\_ip) | List of allowed ip. | `list(any)` | `[]` | no |
| <a name="input_ssh_allowed_ports"></a> [ssh\_allowed\_ports](#input\_ssh\_allowed\_ports) | List of allowed ingress ports | `list(any)` | `[]` | no |
| <a name="input_ssh_protocol"></a> [ssh\_protocol](#input\_ssh\_protocol) | The protocol. If not icmp, tcp, udp, or all use the. | `string` | `"tcp"` | no |
| <a name="input_ssh_sg_ingress_description"></a> [ssh\_sg\_ingress\_description](#input\_ssh\_sg\_ingress\_description) | Description of the ingress rule | `string` | `"Description of the ingress rule use elasticache."` | no |
| <a name="input_subnet_ids"></a> [subnet\_ids](#input\_subnet\_ids) | A list of VPC Subnet IDs to launch in. | `list(string)` | `[]` | no |
| <a name="input_tags"></a> [tags](#input\_tags) | Additional tags (e.g. map(`BusinessUnit`,`XYZ`). | `map(any)` | `{}` | no |
| <a name="input_tenancy"></a> [tenancy](#input\_tenancy) | The tenancy of the instance (if the instance is running in a VPC). An instance with a tenancy of dedicated runs on single-tenant hardware. The host tenancy is not supported for the import-instance command. | `string` | `"default"` | no |
| <a name="input_timeouts"></a> [timeouts](#input\_timeouts) | Define maximum timeout for creating, updating, and deleting EC2 instance resources | `map(string)` | `{}` | no |
| <a name="input_ttl"></a> [ttl](#input\_ttl) | The TTL of the record to add to the DNS zone to complete certificate validation. | `string` | `"300"` | no |
| <a name="input_type"></a> [type](#input\_type) | Type of DNS records to create. | `string` | `"CNAME"` | no |
| <a name="input_user_data"></a> [user\_data](#input\_user\_data) | (Optional) A string of the desired User Data for the ec2. | `string` | `""` | no |
| <a name="input_user_data_base64"></a> [user\_data\_base64](#input\_user\_data\_base64) | Can be used instead of user\_data to pass base64-encoded binary data directly. Use this instead of user\_data whenever the value is not a valid UTF-8 string. For example, gzip-encoded user data must be base64-encoded and passed via this argument to avoid corruption | `string` | `null` | no |
| <a name="input_user_data_replace_on_change"></a> [user\_data\_replace\_on\_change](#input\_user\_data\_replace\_on\_change) | When used in combination with user\_data or user\_data\_base64 will trigger a destroy and recreate when set to true. Defaults to false if not set | `bool` | `null` | no |
| <a name="input_vpc_id"></a> [vpc\_id](#input\_vpc\_id) | The ID of the VPC that the instance security group belongs to. | `string` | `""` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_arn"></a> [arn](#output\_arn) | The ARN of the instance. |
| <a name="output_az"></a> [az](#output\_az) | The availability zone of the instance. |
| <a name="output_instance_id"></a> [instance\_id](#output\_instance\_id) | The instance ID. |
| <a name="output_ipv6_addresses"></a> [ipv6\_addresses](#output\_ipv6\_addresses) | A list of assigned IPv6 addresses. |
| <a name="output_key_name"></a> [key\_name](#output\_key\_name) | The key name of the instance. |
| <a name="output_name"></a> [name](#output\_name) | Name of SSH key. |
| <a name="output_placement_group"></a> [placement\_group](#output\_placement\_group) | The placement group of the instance. |
| <a name="output_private_ip"></a> [private\_ip](#output\_private\_ip) | Private IP of instance. |
| <a name="output_public_ip"></a> [public\_ip](#output\_public\_ip) | Public IP of instance (or EIP). |
| <a name="output_spot_bid_status"></a> [spot\_bid\_status](#output\_spot\_bid\_status) | The current bid status of the Spot Instance Request |
| <a name="output_spot_instance_id"></a> [spot\_instance\_id](#output\_spot\_instance\_id) | The instance ID. |
| <a name="output_subnet_id"></a> [subnet\_id](#output\_subnet\_id) | The EC2 subnet ID. |
| <a name="output_tags"></a> [tags](#output\_tags) | The instance ID. |
| <a name="output_vpc_security_group_ids"></a> [vpc\_security\_group\_ids](#output\_vpc\_security\_group\_ids) | The associated security groups in non-default VPC. |
<!-- END_TF_DOCS -->