---
layout: post
section-type: post
title: Title
category: tech
tags: [ 'terrform', 'module registry' ]
---

An awesome feature of Terraform is <a href="https://www.terraform.io/docs/modules/index.html">modules</a>.

<small><i>Modules in Terraform are self-contained packages of Terraform configurations that are managed as a group. Modules are used to create reusable components in Terraform as well as for basic code organization.</i></small>

The advantage of modules usage is, in addition to share, the possibility to optimize the code usage.
For example, instead of declaring two instances this way :

<pre><code data-trim class="hcl">
resource "openstack_compute_instance_v2" "instance01" {
  name            = "instance01"
  image_id        = "ad091b52-742f-469e-8f3c-fd81cadf0743"
  flavor_id       = "3"
  key_pair        = "my_key_pair_name"
  security_groups = ["default"]

  network {
    name = "my_network"
  }
}

resource "openstack_compute_instance_v2" "instance02" {
  name            = "instance02"
  image_id        = "ad091b52-742f-469e-8f3c-fd81cadf0743"
  flavor_id       = "3"
  key_pair        = "my_key_pair_name"
  security_groups = ["default"]

  network {
    name = "my_network"
  }
}
</code></pre>

You can simply define a module :

<pre><code data-trim class="hcl">
resource "openstack_compute_instance_v2" "instance" {
  name              = "${var.name}"
  image_name        = "${var.image_name}"
  flavor_name       = "${var.flavor_name}"
  key_pair          = "${var.key_pair}"
  security_groups   = ["${var.security_groups}"]
  network {
    name = "${var.network}"
  }
}
</code></pre>


An call it anytime you need it :

<pre><code data-trim class="hcl">
variable "image_id" {
	default = "ad091b52-742f-469e-8f3c-fd81cadf0743"
}
variable "flavor_id" {
	default = "3"
}
variable "key_pair" {
	default = "my_key_pair_name"
}
variable "security_groups" {
	type = "list"
	default = ["default"]
}

module "openstack_instance" {
  source = "julienlevasseur/instance/openstack"
  name              = "${var.name}"
  image_id        	= "${var.image_name}"
  flavor_name       = "${var.flavor_name}"
  key_pair          = "${var.key_pair}"
  security_groups   = "${var.security_groups}"
  network           = "${var.os_network}"
}
</code></pre>





<h2>Module Registry</h2>

<inmg src="https://www.datocms-assets.com/2885/1509734521-01-registry-home-ss.png"/>


<pre><code data-trim class="yaml">
me-img: "https://github.com/user_name.png?size=500"
</code></pre>