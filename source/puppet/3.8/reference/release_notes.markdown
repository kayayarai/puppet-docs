---
layout: default
title: "Puppet 3.8 Release Notes"
description: "Puppet release notes for version 3.8"
canonical: "/puppet/latest/reference/release_notes.html"
---

[upgrade]: /guides/install_puppet/upgrading.html
[puppet_3]: /puppet/3/reference/release_notes.html

[puppet.conf]: ./config_file_main.html
[main manifest]: ./dirs_manifest.html
[env_api]: /references/3.7.latest/developer/file.http_environments.html
[file_system_redirect]: ./lang_windows_file_paths.html#file-system-redirection-when-running-32-bit-puppet-on-64-bit-windows
[environment.conf]: ./config_file_environment.html
[default_manifest]: /references/3.7.latest/configuration.html#defaultmanifest
[disable_per_environment_manifest]: /references/3.7.latest/configuration.html#disableperenvironmentmanifest
[directory environments]: ./environments.html
[future]: ./experiments_future.html

This page tells the history of the Puppet 3.8 series.

Elsewhere: release notes for:

* [Puppet 3.7](/puppet/3.7/reference/release_notes.html)
* [Puppet 3.6](/puppet/3.6/reference/release_notes.html)
* [Puppet 3.5](/puppet/3.5/reference/release_notes.html)
* [Puppet 3.0 -- 3.4][puppet_3]

Puppet's version numbers use the format X.Y.Z, where:

* X must increase for major backwards-incompatible changes
* Y may increase for backwards-compatible new functionality
* Z may increase for bug fixes

How to Upgrade
-----

Before upgrading, **look at the table of contents above and see if there are any "UPGRADE WARNING" or "Upgrade Note" items for the new version.** Although it's usually safe to upgrade from any 3.x version to any later 3.x version, there are sometimes special conditions that can cause trouble.

We always recommend that you **upgrade your Puppet master servers before upgrading the agents they serve.**

If you're upgrading from Puppet 2.x, please [learn about major upgrades of Puppet first!][upgrade] We have important advice about upgrade plans and package management practices. The short version is: test first, roll out in stages, give yourself plenty of time to work with. Also, read the [release notes for Puppet 3][puppet_3] for a list of all the breaking changes made between the 2.x and 3.x series.


## Puppet 3.8.1

Released May 26, 2015.

Puppet 3.8.1 is a bug fix release (with future parser changes) in the Puppet 3.8 series. It's the first official open source release in the 3.8 series.

The main focus of this release is to make sure the 3.8 future parser is forward-compatible with the Puppet language as of Puppet 4.1. It also fixes several bugs.

### Bug Fixes: Major

The initial 3.8.0 release partially broke the per-environment `parser` setting added in 3.7.5, requiring some contortions to make per-environment parser changes work. This is now fixed.

* [PUP-4636: Manifest with future parser code fails in a non-production environment with environment.conf parser=future setting](https://tickets.puppetlabs.com/browse/PUP-4636)

### Improvements: Future Parser

This release improves the Puppet language with a new `\u{xxxxxx}` escape sequence for Unicode characters and a new NotUndef data type. It also adds a feature to the 4.x function API.

* [PUP-4438: Add required_repeated_param to 4.x function API](https://tickets.puppetlabs.com/browse/PUP-4438)
* [PUP-4483: Add NotUndef type to the set of Puppet types](https://tickets.puppetlabs.com/browse/PUP-4483)
* [PUP-4385: Can't write WOMANS HAT emoji with \uXXXX unicode escapes](https://tickets.puppetlabs.com/browse/PUP-4385)


### Bug Fixes: Future Parser

This release fixes several bugs with the Puppet language that were also fixed in Puppet 4.1.0.

* [PUP-4178: defined() function returns true for undef and empty string, and false for "main"](https://tickets.puppetlabs.com/browse/PUP-4178)
* [PUP-4374: Splatting attributes into an amended attribute block isn't supported](https://tickets.puppetlabs.com/browse/PUP-4374)
* [PUP-4398: Splat unfolding not supported in method call syntax](https://tickets.puppetlabs.com/browse/PUP-4398)
* [PUP-4428: The 'err' logging function cannot be called as a statement](https://tickets.puppetlabs.com/browse/PUP-4428)
* [PUP-4462: Single backslash before $ blocks interpolation in heredoc with no escapes enabled](https://tickets.puppetlabs.com/browse/PUP-4462)
* [PUP-4520: Future parser is not correctly handling the default case of a case statement](https://tickets.puppetlabs.com/browse/PUP-4520)


### Bug Fixes: Resource Types and Providers

* [PUP-3829: pip package provider is broken on EL (where osfamily is RedHat)](https://tickets.puppetlabs.com/browse/PUP-3829)
* [PUP-4604: Port fix for pip provider on more recent RedHat operating systems.](https://tickets.puppetlabs.com/browse/PUP-4604)

### Bug Fixes: Misc

* [PUP-4461: manifest changes are ignored when using hiera_include](https://tickets.puppetlabs.com/browse/PUP-4461)
* [PUP-4437: Update the "puppet-agent" Repo for 4.0.1 to Incorporate Fix for PUP-4390](https://tickets.puppetlabs.com/browse/PUP-4437)


## Puppet 3.8.0

Released April 28, 2015, as part of Puppet Enterprise 3.8.0. The first official open source release in the 3.8 series will be 3.8.1.

Puppet 3.8.0 is a backward-compatible features and fixes release in the Puppet 3 series.

### New Feature: Back-end Support for Upgrade Previews

This version includes several backend changes to support the PE-only compilation preview module.

* [PUP-4113: Migrate and Preview 3.8 Language to 4.0](https://tickets.puppetlabs.com/browse/PUP-4113)

### New Feature: Logging as JSON

In any of the Puppet subcommands that take the `--logdest` command line option, you can now specify a path to a JSON file and Puppet will log a (partial) JSON array of message objects to that file.

* [PUP-4201: Add support for structured logging](https://tickets.puppetlabs.com/browse/PUP-4201)
* [PUP-4336](https://tickets.puppetlabs.com/browse/PUP-4336) and [PUP-4341](https://tickets.puppetlabs.com/browse/PUP-4341) cover bugs in this feature that were fixed before release.


### Improvements: Resource Types and Providers

* [PUP-1628: Add mount provider for AIX](https://tickets.puppetlabs.com/browse/PUP-1628)
* [PUP-1291: scheduled_task : add support for "every X minutes or hours" mode](https://tickets.puppetlabs.com/browse/PUP-1291)


### Bug Fixes: Resource Types and Providers

#### Windows

* On Windows, the `group` resource type would ignore the `auth_membership` attribute and always treat the list of members as a complete list, removing any users not listed in that `group` resource. This was also fixed in Puppet 4.0.0. [PUP-4185: Backport ability to add a member to group to Puppet 3.8](https://tickets.puppetlabs.com/browse/PUP-4185)
* [PUP-1279: Windows Group and User fail during deletion even though it is successful](https://tickets.puppetlabs.com/browse/PUP-1279)
* [PUP-3653: Unable to create/force empty Windows groups](https://tickets.puppetlabs.com/browse/PUP-3653)
* [PUP-3804: User resource cannot add DOMAIN\User style accounts (through Active Directory) and should emit error message](https://tickets.puppetlabs.com/browse/PUP-3804)
* [PUP-4390: Regression: Windows service provider fails to retrieve current state on 2003](https://tickets.puppetlabs.com/browse/PUP-4390)
* Weekly `scheduled_task` resources were logging incorrect change notifications. This was also fixed in Puppet 4.0.0. [PUP-4186: Backport Weekly tasks always notify 'trigger changed' to Puppet 3.8](https://tickets.puppetlabs.com/browse/PUP-4186)

#### Other Operating Systems

* [PUP-4345: Port PUP-3388 "Issue Creating Multiple Mirrors in Zpool Resource" to PUP 3.8](https://tickets.puppetlabs.com/browse/PUP-4345)
* [PUP-4090: Zypper provider doesn't work correctly on SLES 10.4 with install_options set](https://tickets.puppetlabs.com/browse/PUP-4090)

### Improvements: Future Language

* [PUP-4277: Case and Selector match should be deep](https://tickets.puppetlabs.com/browse/PUP-4277)

### Bug Fixes: Future Language

* [PUP-4379: Future parser interpolation with [] after variable prefixed with underscore](https://tickets.puppetlabs.com/browse/PUP-4379)
* [PUP-4278: unless with else when then part is empty produces nil result (future parser)](https://tickets.puppetlabs.com/browse/PUP-4278)

### Bug Fixes: General

* [PUP-927: puppet apply on Windows always uses *nix style newlines from templates](https://tickets.puppetlabs.com/browse/PUP-927)
* [PUP-3863: hiera('some::key', undef) returns empty string](https://tickets.puppetlabs.com/browse/PUP-3863)
* [PUP-4334: hiera_include stopped working](https://tickets.puppetlabs.com/browse/PUP-4334)
* [PUP-4190: CLONE - Puppet device displays credentials in plain text when run manually](https://tickets.puppetlabs.com/browse/PUP-4190)
* [PUP-4187: Backport Puppet should execute ruby.exe not cmd.exe when running as a Windows to Puppet 3.8](https://tickets.puppetlabs.com/browse/PUP-4187)



