# How We Release Software

This guide for our maintainers lists the process for taking community contributions and releasing cookbook artifacts for consumption by the community.

## Release on every contribution

Smaller changes incur less risk, and releasing after each contribution ensures we get the changes to users and contributors as quickly as possible. We aim to ship new cookbook releases after any bugfix or feature enhancements are merged. For cookbooks where automated testing has not been implemented there be some delay, as manual testing is necessary.

## Follow Semver

Infrastructure stability for our users is critical so we aim to release predictably versioned cookbooks. Following Semver allows users to pin cookbook requirements in wrapper cookbooks or environment files, and avoid breaking changes. See <http://semver.org/>

Cookbooks follow a `MAJOR.MINOR.PATCH` versioning scheme based on [Semantic Versioning](http://semver.org/).  

Given a version number `MAJOR.MINOR.PATCH`:

    `MAJOR` version releases (e.g. 1.x -> 2.x) will include breaking or backwards-incompatible changes.
        Example: When updating a cookbook to use new chef-client features that are not available in older versions.
        Example: When removing support for a platform, or version of a platform.
    `MINOR` version releases (e.g. 1.1 -> 1.2) will include new features, bug fixes, and will be backwards-compatible to the best of the Maintainers' abilities.
        Example: When adding a new custom resource to a cookbook.
    `PATCH` version releases (e.g. 1.1.1 -> 1.1.2) will include backwards-compatible bug fixes.
        Example: Fixing a misspelled attribute in a recipe.

When incrementing a cookbook version, the following conditions will apply:

    When `MAJOR` increases, `MINOR` and `PATCH` will be reset to zero (e.g. 11.X.X -> 12.0.0)
        Note: New features that did not exist in version 1.1.0 may be released in 2.0.0 without any intermediary releases.
    When `MINOR` increases, `PATCH` will be reset to zero (e.g. 11.3.x -> 11.4.0)

## Test Before Releasing

Many cookbooks have integration tests that run using kitchen-dokken in Travis CI. For these cookbooks we're preforming integration testing on every commit. For others integration tests must be run locally and this should be done before any release to ensure high quality releases.

## Use Community Cookbook Releaser for version/changelog bumping

Bumping versions, writing changelogs, properly formatting dates and versions. It's all repetive and error prone. To aid in releasing cookbooks we created the `community_cookbook_releaser` gem that provides the `ccr` command for prepping cookbooks for release. This is a fairly simple script packaged as a gem that eliminates some of the manual steps in releasing a cookbook. It prompts you for the new version (autofills a patch bump), bumps the metadata version, and then writes out a changelog with the proper version / date / changes. Why not release the changes also? Well the changelog is auto generated with the title of every commit since the last release and that's often not the most useful list of changes for end users. A well written changelog leaves out trivial changes like formatting changes, and provides deep context for other commits. It's something you can't entirely auto generate. This generated changelog should serve as a starting point which you can edit to appropriately convey the importance of changes to the community.

### Example

```
# cookbooks/sql_server(git:master)≻ ccr
Community Cookbook Releaser
A simple script to aid in version bumps and changelog generation for Chef managed community cookbooks

New version to release [autobump to 5.3.2]:

Version bumped & changelog written out with each commit as an item. Now edit, commit, and release!
```

## Use Stove for tag management / file pruning

Cookbook artifacts on Github are rather large and include files such as Gemfiles, test cookbooks, and large numbers of other development files. These files are not necessary for the cookbook to function in an end user environment, and we've chosen to use Stove to package a minimal cookbook artifact for upload to the Supermarket. Stove includes an internal whitelist of files, which will be included in the Supermarket artifact, and also handles tagging the git repo on release.
