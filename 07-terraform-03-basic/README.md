# 7.3. Основы и принцип работы Терраформ

Вывод команды terraform workspace list

![alt text](https://i2.paste.pics/e2107d09a7acf5b8374512388177f423.png)

main.tf c count. 

	provider "aws" {
	  access_key = ""
	  secret_key = ""
	  region = "us-east-1"
	}

	data "aws_ami" "amazon_linux" {
		most_recent = true
		owners      = ["amazon"]
		filter {
			name = "name"
			values = ["amzn-ami-hvm-*-x86_64-gp2"]
		}

		filter {
			name = "owner-alias"
			values = ["amazon"]
		}
	}

	locals {

		instances = {
			"t3.micro" = data.aws_ami.amazon_linux.id
			"t3.large" = data.aws_ami.amazon_linux.id
		}

		web_instance_type_map = {
			stage = "t3.micro"
			prod = "t3.large"
		}

		web_instance_count_map = {
			stage = 1
			prod = 2
		}
	}

	resource "aws_instance" "web" {
		ami = data.aws_ami.amazon_linux.id
		instance_type = local.web_instance_type_map[terraform.workspace]
		count = local.web_instance_count_map[terraform.workspace]

		tags = {
			Name = "HelloWorld"
		}

		lifecycle {
			create_before_destroy = true
		}
	}


	resource "aws_instance" "web1" {

		for_each = local.instances
		ami = each.value
		instance_type = each.key

		tags = {
			Name = "HelloWorld"
		}
	}


Вывод команды terraform plan для воркспейса prod

	Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
	  + create

	Terraform will perform the following actions:

	  # aws_instance.web[0] will be created
	  + resource "aws_instance" "web" {
	      + ami                                  = "ami-0a2c275b42dee0b81"
	      + arn                                  = (known after apply)
	      + associate_public_ip_address          = (known after apply)
	      + availability_zone                    = (known after apply)
	      + cpu_core_count                       = (known after apply)
	      + cpu_threads_per_core                 = (known after apply)
	      + disable_api_termination              = (known after apply)
	      + ebs_optimized                        = (known after apply)
	      + get_password_data                    = false
	      + host_id                              = (known after apply)
	      + id                                   = (known after apply)
	      + instance_initiated_shutdown_behavior = (known after apply)
	      + instance_state                       = (known after apply)
	      + instance_type                        = "t3.large"
	      + ipv6_address_count                   = (known after apply)
	      + ipv6_addresses                       = (known after apply)
	      + key_name                             = (known after apply)
	      + monitoring                           = (known after apply)
	      + outpost_arn                          = (known after apply)
	      + password_data                        = (known after apply)
	      + placement_group                      = (known after apply)
	      + placement_partition_number           = (known after apply)
	      + primary_network_interface_id         = (known after apply)
	      + private_dns                          = (known after apply)
	      + private_ip                           = (known after apply)
	      + public_dns                           = (known after apply)
	      + public_ip                            = (known after apply)
	      + secondary_private_ips                = (known after apply)
	      + security_groups                      = (known after apply)
	      + source_dest_check                    = true
	      + subnet_id                            = (known after apply)
	      + tags                                 = {
		  + "Name" = "HelloWorld"
		}
	      + tags_all                             = {
		  + "Name" = "HelloWorld"
		}
	      + tenancy                              = (known after apply)
	      + user_data                            = (known after apply)
	      + user_data_base64                     = (known after apply)
	      + vpc_security_group_ids               = (known after apply)

	      + capacity_reservation_specification {
		  + capacity_reservation_preference = (known after apply)

		  + capacity_reservation_target {
		      + capacity_reservation_id = (known after apply)
		    }
		}

	      + ebs_block_device {
		  + delete_on_termination = (known after apply)
		  + device_name           = (known after apply)
		  + encrypted             = (known after apply)
		  + iops                  = (known after apply)
		  + kms_key_id            = (known after apply)
		  + snapshot_id           = (known after apply)
		  + tags                  = (known after apply)
		  + throughput            = (known after apply)
		  + volume_id             = (known after apply)
		  + volume_size           = (known after apply)
		  + volume_type           = (known after apply)
		}

	      + enclave_options {
		  + enabled = (known after apply)
		}

	      + ephemeral_block_device {
		  + device_name  = (known after apply)
		  + no_device    = (known after apply)
		  + virtual_name = (known after apply)
		}

	      + metadata_options {
		  + http_endpoint               = (known after apply)
		  + http_put_response_hop_limit = (known after apply)
		  + http_tokens                 = (known after apply)
		}

	      + network_interface {
		  + delete_on_termination = (known after apply)
		  + device_index          = (known after apply)
		  + network_interface_id  = (known after apply)
		}

	      + root_block_device {
		  + delete_on_termination = (known after apply)
		  + device_name           = (known after apply)
		  + encrypted             = (known after apply)
		  + iops                  = (known after apply)
		  + kms_key_id            = (known after apply)
		  + tags                  = (known after apply)
		  + throughput            = (known after apply)
		  + volume_id             = (known after apply)
		  + volume_size           = (known after apply)
		  + volume_type           = (known after apply)
		}
	    }

	  # aws_instance.web[1] will be created
	  + resource "aws_instance" "web" {
	      + ami                                  = "ami-0a2c275b42dee0b81"
	      + arn                                  = (known after apply)
	      + associate_public_ip_address          = (known after apply)
	      + availability_zone                    = (known after apply)
	      + cpu_core_count                       = (known after apply)
	      + cpu_threads_per_core                 = (known after apply)
	      + disable_api_termination              = (known after apply)
	      + ebs_optimized                        = (known after apply)
	      + get_password_data                    = false
	      + host_id                              = (known after apply)
	      + id                                   = (known after apply)
	      + instance_initiated_shutdown_behavior = (known after apply)
	      + instance_state                       = (known after apply)
	      + instance_type                        = "t3.large"
	      + ipv6_address_count                   = (known after apply)
	      + ipv6_addresses                       = (known after apply)
	      + key_name                             = (known after apply)
	      + monitoring                           = (known after apply)
	      + outpost_arn                          = (known after apply)
	      + password_data                        = (known after apply)
	      + placement_group                      = (known after apply)
	      + placement_partition_number           = (known after apply)
	      + primary_network_interface_id         = (known after apply)
	      + private_dns                          = (known after apply)
	      + private_ip                           = (known after apply)
	      + public_dns                           = (known after apply)
	      + public_ip                            = (known after apply)
	      + secondary_private_ips                = (known after apply)
	      + security_groups                      = (known after apply)
	      + source_dest_check                    = true
	      + subnet_id                            = (known after apply)
	      + tags                                 = {
		  + "Name" = "HelloWorld"
		}
	      + tags_all                             = {
		  + "Name" = "HelloWorld"
		}
	      + tenancy                              = (known after apply)
	      + user_data                            = (known after apply)
	      + user_data_base64                     = (known after apply)
	      + vpc_security_group_ids               = (known after apply)

	      + capacity_reservation_specification {
		  + capacity_reservation_preference = (known after apply)

		  + capacity_reservation_target {
		      + capacity_reservation_id = (known after apply)
		    }
		}

	      + ebs_block_device {
		  + delete_on_termination = (known after apply)
		  + device_name           = (known after apply)
		  + encrypted             = (known after apply)
		  + iops                  = (known after apply)
		  + kms_key_id            = (known after apply)
		  + snapshot_id           = (known after apply)
		  + tags                  = (known after apply)
		  + throughput            = (known after apply)
		  + volume_id             = (known after apply)
		  + volume_size           = (known after apply)
		  + volume_type           = (known after apply)
		}

	      + enclave_options {
		  + enabled = (known after apply)
		}

	      + ephemeral_block_device {
		  + device_name  = (known after apply)
		  + no_device    = (known after apply)
		  + virtual_name = (known after apply)
		}

	      + metadata_options {
		  + http_endpoint               = (known after apply)
		  + http_put_response_hop_limit = (known after apply)
		  + http_tokens                 = (known after apply)
		}

	      + network_interface {
		  + delete_on_termination = (known after apply)
		  + device_index          = (known after apply)
		  + network_interface_id  = (known after apply)
		}

	      + root_block_device {
		  + delete_on_termination = (known after apply)
		  + device_name           = (known after apply)
		  + encrypted             = (known after apply)
		  + iops                  = (known after apply)
		  + kms_key_id            = (known after apply)
		  + tags                  = (known after apply)
		  + throughput            = (known after apply)
		  + volume_id             = (known after apply)
		  + volume_size           = (known after apply)
		  + volume_type           = (known after apply)
		}
	    }

	  # aws_instance.web1["t3.large"] will be created
	  + resource "aws_instance" "web1" {
	      + ami                                  = "ami-0a2c275b42dee0b81"
	      + arn                                  = (known after apply)
	      + associate_public_ip_address          = (known after apply)
	      + availability_zone                    = (known after apply)
	      + cpu_core_count                       = (known after apply)
	      + cpu_threads_per_core                 = (known after apply)
	      + disable_api_termination              = (known after apply)
	      + ebs_optimized                        = (known after apply)
	      + get_password_data                    = false
	      + host_id                              = (known after apply)
	      + id                                   = (known after apply)
	      + instance_initiated_shutdown_behavior = (known after apply)
	      + instance_state                       = (known after apply)
	      + instance_type                        = "t3.large"
	      + ipv6_address_count                   = (known after apply)
	      + ipv6_addresses                       = (known after apply)
	      + key_name                             = (known after apply)
	      + monitoring                           = (known after apply)
	      + outpost_arn                          = (known after apply)
	      + password_data                        = (known after apply)
	      + placement_group                      = (known after apply)
	      + placement_partition_number           = (known after apply)
	      + primary_network_interface_id         = (known after apply)
	      + private_dns                          = (known after apply)
	      + private_ip                           = (known after apply)
	      + public_dns                           = (known after apply)
	      + public_ip                            = (known after apply)
	      + secondary_private_ips                = (known after apply)
	      + security_groups                      = (known after apply)
	      + source_dest_check                    = true
	      + subnet_id                            = (known after apply)
	      + tags                                 = {
		  + "Name" = "HelloWorld"
		}
	      + tags_all                             = {
		  + "Name" = "HelloWorld"
		}
	      + tenancy                              = (known after apply)
	      + user_data                            = (known after apply)
	      + user_data_base64                     = (known after apply)
	      + vpc_security_group_ids               = (known after apply)

	      + capacity_reservation_specification {
		  + capacity_reservation_preference = (known after apply)

		  + capacity_reservation_target {
		      + capacity_reservation_id = (known after apply)
		    }
		}

	      + ebs_block_device {
		  + delete_on_termination = (known after apply)
		  + device_name           = (known after apply)
		  + encrypted             = (known after apply)
		  + iops                  = (known after apply)
		  + kms_key_id            = (known after apply)
		  + snapshot_id           = (known after apply)
		  + tags                  = (known after apply)
		  + throughput            = (known after apply)
		  + volume_id             = (known after apply)
		  + volume_size           = (known after apply)
		  + volume_type           = (known after apply)
		}

	      + enclave_options {
		  + enabled = (known after apply)
		}

	      + ephemeral_block_device {
		  + device_name  = (known after apply)
		  + no_device    = (known after apply)
		  + virtual_name = (known after apply)
		}

	      + metadata_options {
		  + http_endpoint               = (known after apply)
		  + http_put_response_hop_limit = (known after apply)
		  + http_tokens                 = (known after apply)
		}

	      + network_interface {
		  + delete_on_termination = (known after apply)
		  + device_index          = (known after apply)
		  + network_interface_id  = (known after apply)
		}

	      + root_block_device {
		  + delete_on_termination = (known after apply)
		  + device_name           = (known after apply)
		  + encrypted             = (known after apply)
		  + iops                  = (known after apply)
		  + kms_key_id            = (known after apply)
		  + tags                  = (known after apply)
		  + throughput            = (known after apply)
		  + volume_id             = (known after apply)
		  + volume_size           = (known after apply)
		  + volume_type           = (known after apply)
		}
	    }

	  # aws_instance.web1["t3.micro"] will be created
	  + resource "aws_instance" "web1" {
	      + ami                                  = "ami-0a2c275b42dee0b81"
	      + arn                                  = (known after apply)
	      + associate_public_ip_address          = (known after apply)
	      + availability_zone                    = (known after apply)
	      + cpu_core_count                       = (known after apply)
	      + cpu_threads_per_core                 = (known after apply)
	      + disable_api_termination              = (known after apply)
	      + ebs_optimized                        = (known after apply)
	      + get_password_data                    = false
	      + host_id                              = (known after apply)
	      + id                                   = (known after apply)
	      + instance_initiated_shutdown_behavior = (known after apply)
	      + instance_state                       = (known after apply)
	      + instance_type                        = "t3.micro"
	      + ipv6_address_count                   = (known after apply)
	      + ipv6_addresses                       = (known after apply)
	      + key_name                             = (known after apply)
	      + monitoring                           = (known after apply)
	      + outpost_arn                          = (known after apply)
	      + password_data                        = (known after apply)
	      + placement_group                      = (known after apply)
	      + placement_partition_number           = (known after apply)
	      + primary_network_interface_id         = (known after apply)
	      + private_dns                          = (known after apply)
	      + private_ip                           = (known after apply)
	      + public_dns                           = (known after apply)
	      + public_ip                            = (known after apply)
	      + secondary_private_ips                = (known after apply)
	      + security_groups                      = (known after apply)
	      + source_dest_check                    = true
	      + subnet_id                            = (known after apply)
	      + tags                                 = {
		  + "Name" = "HelloWorld"
		}
	      + tags_all                             = {
		  + "Name" = "HelloWorld"
		}
	      + tenancy                              = (known after apply)
	      + user_data                            = (known after apply)
	      + user_data_base64                     = (known after apply)
	      + vpc_security_group_ids               = (known after apply)

	      + capacity_reservation_specification {
		  + capacity_reservation_preference = (known after apply)

		  + capacity_reservation_target {
		      + capacity_reservation_id = (known after apply)
		    }
		}

	      + ebs_block_device {
		  + delete_on_termination = (known after apply)
		  + device_name           = (known after apply)
		  + encrypted             = (known after apply)
		  + iops                  = (known after apply)
		  + kms_key_id            = (known after apply)
		  + snapshot_id           = (known after apply)
		  + tags                  = (known after apply)
		  + throughput            = (known after apply)
		  + volume_id             = (known after apply)
		  + volume_size           = (known after apply)
		  + volume_type           = (known after apply)
		}

	      + enclave_options {
		  + enabled = (known after apply)
		}

	      + ephemeral_block_device {
		  + device_name  = (known after apply)
		  + no_device    = (known after apply)
		  + virtual_name = (known after apply)
		}

	      + metadata_options {
		  + http_endpoint               = (known after apply)
		  + http_put_response_hop_limit = (known after apply)
		  + http_tokens                 = (known after apply)
		}

	      + network_interface {
		  + delete_on_termination = (known after apply)
		  + device_index          = (known after apply)
		  + network_interface_id  = (known after apply)
		}

	      + root_block_device {
		  + delete_on_termination = (known after apply)
		  + device_name           = (known after apply)
		  + encrypted             = (known after apply)
		  + iops                  = (known after apply)
		  + kms_key_id            = (known after apply)
		  + tags                  = (known after apply)
		  + throughput            = (known after apply)
		  + volume_id             = (known after apply)
		  + volume_size           = (known after apply)
		  + volume_type           = (known after apply)
		}
	    }

	Plan: 4 to add, 0 to change, 0 to destroy.


