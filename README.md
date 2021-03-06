Terraform FlexibleEngine Provider
============================

- Website: https://www.terraform.io
- Tools: comming soon...
- Mailing list: comming soon...

<img src="https://cdn.rawgit.com/hashicorp/terraform-website/master/content/source/assets/images/logo-hashicorp.svg" width="600px">

Requirements
------------

-	[Terraform](https://www.terraform.io/downloads.html) 0.10.x
-	[Go](https://golang.org/doc/install) 1.8 (to build the provider plugin)


Building The Provider
---------------------

Clone repository to: `$GOPATH/src/github.com/Karajan-project/terraform-provider-flexibleengine`

```sh
$ go get github.com/Karajan-project/terraform-provider-flexibleengine
```

Enter the provider directory and build the provider

```sh
$ cd $GOPATH/src/github.com/Karajan-project/terraform-provider-flexibleengine
$ make build
```

## Exact steps on clean Ubuntu 16.04

```sh
# prerequisites are sudo privileges, unzip, make, wget and git.  Use apt install if missing.
$ wget https://storage.googleapis.com/golang/go1.9.1.linux-amd64.tar.gz
$ sudo tar -C /usr/local -xzf go1.9.1.linux-amd64.tar.gz
$ export PATH=$PATH:/usr/local/go/bin # You should put in your .profile or .bashrc
$ go version # to verify it runs and version #
$ go get github.com/Karajan-project/terraform-provider-flexibleengine
$ cd ~/go/src/github.com/Karajan-project/terraform-provider-flexibleengine/
$ make build
$ export PATH=$PATH:~/go/bin # You should put in your .profile or .bashrc
$ wget https://releases.hashicorp.com/terraform/0.10.7/terraform_0.10.7_linux_amd64.zip
$ unzip terraform_0.10.7_linux_amd64.zip
$ mv terraform ~/go/bin
$ terraform version # to verify it runs and version #
$ vi test.tf # paste in Quick Start contents, fix authentication information
$ terraform init
$ terraform plan
$ terraform apply # Should all work if everything is correct.

```

## Quick Start

```hcl
# Configure the FlexibleEngine Provider
# This will work with a single defined/default network, otherwise you need to specify network
# to fix errrors about multiple networks found.
provider "flexibleengine" {
  user_name   = "user"
  tenant_name = "tenant"
  domain_name = "domain"
  password    = "pwd"
  # the auth url format follows: https://iam.{region_id}.prod-cloud-ocb.orange-business.com:443/v3
  auth_url    = "https://iam.eu-west-0.prod-cloud-ocb.orange-business.com:443/v3"
  region      = "eu-west-0"
}

# Create a web server
resource "flexibleengine_compute_instance_v2" "test-server" {
  name		  = "test-server"
  image_name  = "Standard_CentOS_7_latest"
  flavor_name = "s1.medium"
}
```

### Full Example
----------------------
Please see full example at https://github.com/Karajan-project/fe-terraform, 
you must fill in the required variables in variables.tf.

Using the provider
----------------------
Please see the documentation at [provider usage](website/docs/index.html.markdown).

Developing the Provider
---------------------------

If you wish to work on the provider, you'll first need [Go](http://www.golang.org) installed on your machine (version 1.8+ is *required*). You'll also need to correctly setup a [GOPATH](http://golang.org/doc/code.html#GOPATH), as well as adding `$GOPATH/bin` to your `$PATH`.

To compile the provider, run `make build`. This will build the provider and put the provider binary in the `$GOPATH/bin` directory.

```sh
$ make build
...
$ $GOPATH/bin/terraform-provider-flexibleengine
...
```

In order to test the provider, you can simply run `make test`.

```sh
$ make test
```

In order to run the full suite of Acceptance tests, run `make testacc`.

*Note:* Acceptance tests create real resources, and often cost money to run.

```sh
$ make testacc
```
