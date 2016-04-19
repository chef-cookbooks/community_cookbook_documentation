# Cookbook TESTING doc

This document describes the process for testing Chef community cookbooks using ChefDK. Cookbooks can be tested using the test dependencies defined in cookbook Gemfiles alone, but that process will not be covered in this document in order to maintain simplicity.

## Testing Prerequisites

A working ChefDK installation set as your system's default ruby. ChefDK can be downloaded at <<https://downloads.chef.io/chef-dk/>

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

## Rakefile

Each cookbook contains a Rakefile which includes a number of tasks, each of which can be ran individually, or in groups. Typing "rake" by itself will perform the default checks: style checks (Rubocop and Foodcritic), and unit tests (Chefspec). To see a complete list of available tasks run `Rake -T`

```
$ rake -T
rake integration:vagrant      # Run Test Kitchen with Vagrant
rake maintainers:generate     # Generate MarkDown version of MAINTAINERS file
rake spec                     # Run ChefSpec examples
rake style                    # Run all style checks
rake style:chef               # Run Chef style checks
rake style:ruby               # Run Ruby style checks
rake style:ruby:auto_correct  # Auto-correct RuboCop offenses
rake travis                   # Run all tests on Travis
```

## Style Testing

Ruby style tests can be performed by Rubocop by issuing either

```shell
rake style:ruby
```

Chef style/correctness tests can be performed with Foodcritic by issuing either

```shell
rake style:chef
```

## Spec Testing

Unit testing is done by running Rspec examples. Rspec will test any libraries, then test recipes using ChefSpec. This works by compiling a recipe (but not converging it), and allowing the user to make assertions about the resource_collection.

## Integration Testing

Integration testing is performed by Test Kitchen. After a successful converge, tests are uploaded and ran out of band of Chef. Tests should be designed to ensure that a recipe has accomplished its goal.

## Integration Testing using Vagrant

Integration tests can be performed on a local workstation using either Virtualbox or VMWare as the virtualization hypervisor. To run tests against all available instances run:

```shell
rake integration:vagrant
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

- Windows Server 2008
- Windows Server 2012
- Windows 7 Professional
- Windows 8.1 Professional
- Mac OS X 10.7-10.11
- Solaris 10.11

If you are a Chef employee, you will need to get VMWare Fusion, [Vagrant plugin for VMWare](https://www.vagrantup.com/vmware/), and set up a [Hashicorp Atlas](https://www.hashicorp.com/atlas.html) account that has access to the Chef images.

Once you have the required software, login to Atlas from the command line. Use the private instances from the .kitchen.yml file, or set the environment variable KITCHEN_LOCAL_YAML to kitchen.vmware.yml.

Otherwise if you are not a Chef employee, in order to test these operating systems you will need to obtain the appropriate licenses and images as needed.
