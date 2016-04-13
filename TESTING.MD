# Cookbook TESTING doc

## Testing Prerequisites

Chef cookbooks require either a working ChefDK installation set as your system's default ruby or Ruby 2.0+ with bundler installed. Using ChefDK provides a consistent Ruby install, and is the suggested method. ChefDK can be downloaded at <<https://downloads.chef.io/chef-dk/>

To ensure all gems are updated to their latest releases run `bundle install; bundle update` before running any Rake testing tasks.

Integration testing relies on both Hashicorp's [Vagrant](https://www.vagrantup.com/downloads.html) and Oracle's [Virtualbox](https://www.virtualbox.org/wiki/Downloads), which must be installed first.

## Rakefile

The Rakefile ships with a number of tasks, each of which can be ran individually, or in groups. Typing "rake" by itself will perform style checks with Rubocop and Foodcritic, ChefSpec with rspec, and integration with Test Kitchen using the Vagrant driver by default.Alternatively, integration tests can be ran with Test Kitchen cloud drivers.

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
bundle exec rubocop
```

or

```shell
rake style:ruby
```

Chef style/correctness tests can be performed with Foodcritic by issuing either

```shell
bundle exec foodcritic
```

or

```shell
rake style:chef
```

## Spec Testing

Unit testing is done by running Rspec examples. Rspec will test any libraries, then test recipes using ChefSpec. This works by compiling a recipe (but not converging it), and allowing the user to make assertions about the resource_collection.

## Integration Testing

Integration testing is performed by Test Kitchen. Test Kitchen will use either the Vagrant driver or various cloud drivers to instantiate machines and apply cookbooks. After a successful converge, tests are uploaded and ran out of band of Chef. Tests should be designed to ensure that a recipe has accomplished its goal.

## Integration Testing using Vagrant

Integration tests can be performed on a local workstation using Virtualbox or VMWare. Detailed instructions for setting this up can be found at the [Bento](https://github.com/chef/bento) project web site.

Integration tests using Vagrant can be performed with either

```shell
bundle exec kitchen test
```

or

```shell
rake integration:vagrant
```

<<<<<<< a2522b87718a266938e656c7597c34e6fa1571f3

## Private Images

Some operating systems have specific licenses that prevent us from making those images available freely. We either comment these out in the .kitchen.yml or create a separate .kitchen.vmware.yml file for Chef internal use.

Images include:

- Windows Server 2008
- Windows Server 2012
- Windows 7 Professional
- Windows 8.1 Professional
- Mac OS X 10.7-10.11
- Solaris 10.11
- Integration Testing using Cloud providers

Integration tests can be performed on cloud providers using Test Kitchen plugins. If the cookbook ships with a `.kitchen.cloud.yml` that references environmental variables present in the shell that `kitchen test` is ran from. These usually contain authentication tokens for driving IaaS APIs, as well as the paths to ssh private keys needed for Test Kitchen log into them after they've been created.

Examples of environment variables being set in `~/.bash_profile`:

```shell
# digital_ocean
export DIGITAL_OCEAN_CLIENT_ID='your_bits_here'
export DIGITAL_OCEAN_API_KEY='your_bits_here'
export DIGITAL_OCEAN_SSH_KEY_IDS='your_bits_here'

# aws
export AWS_ACCESS_KEY_ID='your_bits_here'
export AWS_SECRET_ACCESS_KEY='your_bits_here'
export AWS_KEYPAIR_NAME='your_bits_here'
```

Integration tests using cloud drivers can be performed with either

```shell
export KITCHEN_YAML=.kitchen.cloud.yml
bundle exec kitchen test
```

or

```shell
rake integration:cloud
```

## Digital Ocean Hint

At the time of this writing, you cannot find the numerical values needed for your SSH_KEY_IDS from the GUI. Instead, you will need to access the API from the command line.

```shell
curl -L 'https://api.digitalocean.com/ssh_keys/?client_id=your_bits_here&api_key=your_bits_here'
```

## Words about .travis.yml

In order for Travis to perform integration tests on public cloud providers, two major things need to happen. First, the environment variables referenced by `.kitchen.cloud.yml` need to be made available. Second, the private half of the ssh keys needed to log into machines need to be dropped off on the machine.

The first part is straight forward. The travis gem can encrypt environment variables against the public key on the Travis repository and add them to the .travis.yml.

```
        > Format markdown
```

If you are a Chef employee, you will need to get VMWare Fusion, [Vagrant plugin for VMWare](https://www.vagrantup.com/vmware/), and set up a [Hashicorp Atlas](https://www.hashicorp.com/atlas.html) account that has access to the Chef images.

<<<<<<< a2522b87718a266938e656c7597c34e6fa1571f3 Once you have the required software, login to Atlas from the command line. Uncomment the suites in .kitchen.yml file, or set the environment variable KITCHEN_LOCAL_YAML to kitchen.vmware.yml.

Otherwise if you are not a Chef employee, in order to test these operating systems you will need to obtain the appropriate licenses and images as needed.

======= The second part is a little more complicated. Travis ENV variables are restricted to 90 bytes, and will not fit an entire SSH key. This can be worked around by breaking them up into 90 byte chunks, stashing them into ENV variables, then digging them out in the `before_install` section of .travis.yml

Here is an AWK script to do the encoding.

```
base64 ~/.ssh/travisci_cook_digitalocean.pem | \
awk '{
  j=0;
  for( i=1; i<length; i=i+90 ) {
    system("travis encrypt DO_KEY_CHUNK_" j "=" substr($0, i, 90) " --add");
    j++;
  }
}'

base64 ~/.ssh/travisci_cook_ec2.pem | \
awk '{
  j=0;
  for( i=1; i<length; i=i+90 ) {
    system("travis encrypt EC2_KEY_CHUNK_" j "=" substr($0, i, 90)" --add");
    j++;
  }
}'
```

Then in .travis.yml:

```yaml
before_install:
- echo -n $DO_KEY_CHUNK_{0..30} >> ~/.ssh/id_do.base64
- cat ~/.ssh/id_do.base64 | tr -d ' ' | base64 --decode >  ~/.ssh/id_do.pem
  - echo -n $EC2_KEY_CHUNK_{0..30} >> ~/.ssh/id_ec2.base64
  - cat ~/.ssh/id_ec2.base64 | tr -d ' ' | base64 --decode > ~/.ssh/id_ec2.pem
```

```
        > Format markdown
```
