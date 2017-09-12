# Cookbook TESTING doc

This document describes the process for testing Chef community cookbooks using ChefDK.

## Testing Prerequisites

A working ChefDK installation set as your system's default ruby. ChefDK can be downloaded at <https://downloads.chef.io/chef-dk/>

Hashicorp's [Vagrant](https://www.vagrantup.com/downloads.html) and Oracle's [Virtualbox](https://www.virtualbox.org/wiki/Downloads) for integration testing.

## Installing dependencies

Cookbooks may require additional testing dependencies that do not ship with ChefDK directly. These can be installed into the ChefDK ruby environment with the following commands

Install dependencies:

```shell
chef exec bundle install
```

Update any installed dependencies to the latest versions:

```shell
chef exec bundle update
```

## Local Delivery

For consistent testing across the 100+ Chef managed community cookbooks we use the Delivery CLI tool and the local delivery mode. This allows us to remotely host a single [Delivery project.toml file](https://github.com/chef-cookbooks/community_cookbook_tools/blob/master/delivery/project.toml) that applies to all of our cookbooks.

Delivery defines testing in phases and cookbooks utilize `lint`, `syntax`, and `unit` phases.

To run an individual phase:

```shell
delivery local unit
```

To run all phases:

```shell
delivery local all
```

### lint stage

The lint stage runs Ruby specific code linting using cookstyle (<https://github.com/chef/cookstyle>). Cookstyle offers a tailored RuboCop configuration enabling / disabling rules to better meet the needs of cookbook authors. Cookstyle ensures that projects with multiple authors have consistent code styling.

### syntax stage

The syntax stage runs Chef cookbook specific linting and syntax checks with Foodcritic (<http://www.foodcritic.io/>). Foodcritic tests for over 60 different cookbook conditions and helps authors avoid bad patterns in their cookbooks.

### unit stage

The unit stage runs unit testing with ChefSpec (<http://sethvargo.github.io/chefspec/>). ChefSpec is an extension of Rspec, specially formulated for testing Chef cookbooks. Chefspec compiles your cookbook code and converges the run in memory, without actually executing the changes. The user can write various assertions based on what they expect to have happened during the Chef run. Chefspec is very fast, and quick useful for testing complex logic as you can easily converge a cookbook many times in different ways.

## Integration Testing

Integration testing is performed by Test Kitchen. After a successful converge, tests are uploaded and ran out of band of Chef. Tests should be designed to ensure that a recipe has accomplished its goal.

## Integration Testing using Vagrant

Integration tests can be performed on a local workstation using either VirtualBox or VMWare as the virtualization hypervisor. To run tests against all available instances run:

```shell
chef exec kitchen test
```

To see a list of available test instances run:

```shell
chef exec kitchen list
```

To test specific instance run:

```shell
chef exec kitchen test INSTANCE_NAME
```

## Private Images

Some operating systems have specific licenses that prevent us from making those images available freely. We either comment these the .kitchen.yml or create a separate .kitchen.vmware.yml file for Chef internal use.

Images include:

- Windows Server 2008 R2
- Windows Server 2012 R2
- Windows Server 2016
- Windows 7 Professional
- Windows 8.1 Professional
- Windows 10 Enterprise
- Mac OS X 10.7-10.12
- SLES 12 / 12SP1
- Solaris 10.11

If you are a Chef employee, you will need to get VMWare Fusion, [Vagrant plugin for VMWare](https://www.vagrantup.com/vmware/), and set up a [Hashicorp Atlas](https://www.hashicorp.com/atlas.html) account that has access to the Chef images.

Once you have the required software, login to Atlas from the command line. Use the private instances from the .kitchen.yml file, or set the environment variable KITCHEN_LOCAL_YAML to kitchen.vmware.yml.

Otherwise if you are not a Chef employee, in order to test these operating systems you will need to obtain the appropriate licenses and images as needed.
