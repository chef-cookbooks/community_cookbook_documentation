# Community Cookbook Team

The Community Cookbook Team is responsible for improving the [Chef-managed community cookbooks](https://github.com/chef-cookbooks) and for working with the community to resolve issues and review / merge pull requests.

## Team Members

-

## Our Goals

- Increased velocity of releases for Chef-managed community cookbooks on the Supermarket
- Improved ability for community members to find and write awesome cookbooks
- Decreased the time required for customers to automate key production system

## Design Principles

### Resources come first

When possible we believe in shipping cookbooks as a collection of resources that cookbook consumers can use to compose their own cookbooks. The resources do the heavy lifting and allow consumers the ability to customize the installation and configuration of the application for their particular environment.

### Build for wrapping

Sometimes applications don't lend themselves to a pure resource model. When that's the case cookbooks should be designed with the expectation that users will wrap them in their own wrapper cookbook. Each cookbook meant for wrapping should have an example wrapper cookbook included in the form of a test cookbook for integration testing.

### Test Everything

Cookbooks need to be high-quality and often contributions come in from users developing for a single platform. In order to ensure we don't regress functionality or platform support we must test all the things. This includes linting with Cookstyle, unit testing with ChefSpec, and integration testing with Test Kitchen. These tests should run on every pull request (PR) and commit in order to ensure the master stays in a shippable state. For Linux platforms Test Kitchen should be run using kitchen-dokken within Travis CI for integration testing of every PR.

### Wide platform support

Not everyone runs RHEL and not everyone runs Ubuntu. We aim to provide support for as many platforms as possible. Priority is given to [Foundation Platforms](https://docs.chef.io/platforms.html#chef-client-foundational-platforms) supported by Chef itself.

### Support the 95% use case

Unfortunately new functionality comes with a maintenance cost and because of this we must be mindful of all new functionality we introduce. Chef-managed community cookbooks aim to support the use cases of 95% of users. Sometimes edge cases introduce extreme code complexity and maintenance burden. In this case we'll defer the change to a wrapper cookbook or purpose-built fork.

## Contacting Us

If you have questions about our team or our cookbooks feel free to reach us in the #cookbook-engineering channel of the [Chef Community Slack](https://community-slack.chef.io/).

Our team email address is [cookbooks@chef.io](mailto:cookbooks@chef.io).

## Getting Involved

If you want to get involved, we want to make it as easy as possible. Our contribution process is documented in [CONTRIBUTING.MD in the Community Cookbook Documentation repository on GitHub](https://github.com/chef-cookbooks/community_cookbook_documentation/blob/master/CONTRIBUTING.MD). Feel free to contact us if you need help or additional details.

The Cookbook Engineering team is present in the #cookbook-engineering channel on the [Chef Community Slack](http://community-slack.chef.io/). People are welcome to join and discuss cookbook usage and design.

Discussions about tools and cookbook releases also occur on the [Chef mailing lists on Discourse](https://discourse.chef.io/).

## Events

* [Chef Community Summits](https://www.chef.io/summits/)
