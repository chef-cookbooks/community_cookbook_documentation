# How We Release Software

This guide for our maintainers lists the process for taking community contributions and releasing cookbook artifacts for consumption by the community.

## Release on every contribution

Smaller changes incur less risk, and releasing after each contribution ensures we get the changes to users and contributors as quickly as possible. We aim to ship new cookbook releases after any bugfix or feature enhancements are merged. For cookbooks where automated testing has not been implemented there be some delay, as manual testing is necessary.

## Follow Semver

Infrastructure stability for our users is critical so we aim to release predictably versioned cookbooks. Following Semver allows users to pin cookbook requirements in wrapper cookbooks or environment files, and avoid breaking changes. See <http://semver.org/>

## Test Before Releasing

Many cookbooks have integration tests that run using kitchen-dokken in Travis CI. For these cookbooks we're preforming integration testing on every commit. For others integration tests must be run locally and this should be done before any release to ensure high quality releases.

## Always update the changelog

Understanding changes between releases is important for users consuming our cookbooks. Each release should include a dated changelog entry that includes detailed descriptions of changes. Breaking changes should be clearly called out, and best effort should be made to describe the impact of changes instead of just including PR titles.

## Use Stove for tag management / file pruning

Cookbook artifacts on Github are rather large and include files such as Gemfiles, test cookbooks, and large numbers of other development files. These files are not necessary for the cookbook to function in an end user environment, and we've chosen to use Stove to package a minimal cookbook artifact for upload to the Supermarket. Stove includes an internal whitelist of files, which will be included in the Supermarket artifact, and also handles tagging the git repo on release.
