# Community Cookbook Team

The Community Cookbook Team is responsible for improving the Chef owned community cookbooks, and for working with the community to resolve issues and review / merge pull requests.  

## Our Goals
- Increased velocity of releases for CHEF-managed cookbooks on the Supermarket
- Improved ability for community members to find and write awesome cookbooks
- Decreased time required for customers to automate key production system


## Design Principals

### Resources come first

When possible we believe in shipping cookbooks as a collection of resources that end users can use to compose their own cookbooks.  The resources do the heavy lifting and allow users the ability to customize the installation and configuration of the application for their particular environment.

### Build for wrapping

Sometimes applications don't lend themselves to a pure resource model.  When that's the case cookbooks should be designed with the expectation that users will wrap consume them in their own wrapper cookbook.

### Test Everything

Cookbooks need to be high quality and often contributions come in from users developing for a single platform.  In order to ensure we don't regress functionality or platform support we must test all the things.  This includes linting with rubocop, unit testing with ChefSpec, and integration testing with Test Kitchen. These tests should run on every PR and commit in order to ensure the master stays in a shipable state. For Linux platforms Test Kitchen should be run using Docker within Travis CI for integration testing of every PR.

### Wide platform support

Not everyone runs RHEL and not everyone runs Ubuntu. We aim to provide support for as many platforms as possible.  Priority is given to Tier 1 supported platforms supported by Chef itself.

### Support the 95% use case

Unfortunetly new functionality comes with a maintenance cost and because of this we must be mindful of all new functionality we introduce. Community cookbooks aim to support the use cases of 95% of users. Sometimes edge cases introduce extreme code complexity and maintenance work. In this case we'll defer the change to a wrapper cookbook or purpose built fork.

## Contacting Us

The community cookbook team can be e-mailed directly at cookbooks@chef.io.You can also find a list of maintainers, some of whom are Chef employees, in each cookbook repository at /MAINTAINERS.md

## Getting Involved

If you want to get involved, we want to make it as easy as possible.  We have information on the contributing process at https://github.com/chef-cookbooks/community_cookbook_documentation/blob/master/CONTRIBUTING.MD.  Feel free to contact us if you need help or additional details.
