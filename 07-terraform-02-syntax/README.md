# 7.2. Облачные провайдеры и синтаксис Terraform.

## 1 задание

	Зарегистрировалась в Yandex.Cloud
	

## 2 задание

Получился такой main.tf. 

		variable "YC_TOKEN" {
		  type = string
		}

		variable "YC_CLOUD_ID" {
		  type = string
		}

		variable "YC_FOLDER_ID" {
		  type = string
		}

		variable "YC_ZONE" {
		  type = string
		}


		terraform {
		  required_providers {
		    yandex = {
		      source  = "yandex-cloud/yandex"
		      version = "0.61.0"
		    }
		  }
		}

		provider "yandex" {
		  token     = "${var.YC_TOKEN}"
		  cloud_id  = "${var.YC_CLOUD_ID}"
		  folder_id = "${var.YC_FOLDER_ID}"
		  zone      = "${var.YC_ZONE}"
		}

		resource "yandex_compute_instance" "default" {
		  name        = "test"
		  platform_id = "standard-v1"
		  zone        = "${var.YC_ZONE}"

		  resources {
		    cores  = 2
		    memory = 4
		  }

		  boot_disk {
		    initialize_params {
		      image_id = "${data.yandex_compute_image.ubuntu.id}"
		    }
		  }

		  network_interface {
		    subnet_id = "${yandex_vpc_subnet.foo.id}"
		  }

		  metadata = {
		    foo      = "bar"
		    ssh-keys = "ubuntu:${file("~/.ssh/id_rsa.pub")}"
		  }
		}

		resource "yandex_vpc_network" "foo" {
		}

		resource "yandex_vpc_subnet" "foo" {
		  zone = "${var.YC_ZONE}"
		  network_id = "${yandex_vpc_network.foo.id}"
		  v4_cidr_blocks = ["10.0.0.0/22"]
		}

		data "yandex_compute_image" "ubuntu" {
		  family = "ubuntu-1804-lts"
		}

Параметры сохранила в файле .tfvars и передавала через атрибут -var-file при запуске terraform apply

		terraform apply -var-file=".tfvars"

В результате terraform apply виртуальная машина загрузилась в облако:

![alt text](https://i2.paste.pics/56bb14e76ded0380e6626e54c2451d48.png)

Ответ на вопрос: при помощи какого инструмента (из разобранных на прошлом занятии) можно создать свой образ ami?

	CloudFormation

Ссылку на репозиторий с исходной конфигурацией терраформа.

	https://github.com/yuggoth/devops-netology/tree/main/terraform
