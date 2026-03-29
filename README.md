# simplesamlphp-module-duouniversal

Two-factor authentication module using Duo Security Universal Prompt for SimpleSAMLphp.

** This module is still in development and is not production-ready, use at your own risk! **

## Installation

Once you have installed SimpleSAMLphp, installing this module is very simple.
Just execute the following command in the root of your SimpleSAMLphp
installation:

```bash
composer require 0x0fbc/simplesamlphp-module-duouniversal:dev-feature/upgrade-2.0
```

where `dev-feature/upgrade-2.0` instructs Composer to install the `feature/upgrade-2.0` branch from the Git repository.

Next thing you need to do is to enable the module: in `config.php`,
search for the `module.enable` key and set `duouniversal` to true:

```php
    'module.enable' => [
         'duouniversal' => true,
         …
    ],
```

A datastore other than SimpleSAMLphp's default `phpsession` must be configured to use this module. Information on datastore configuration can be found in the [SimpleSAMLphp documentation](https://simplesamlphp.org/docs/stable/simplesamlphp-maintenance.html#session-management).

## Configuration

1. Copy `module_duouniversal.php.dist` from the `config` directory of this repo to the config directory of your SimpleSAMLphp deployment.
2. Create (if you haven't already) a Duo Universal WebSDKv4 application in the "applications" section of your Duo deployment's admin console and set the following values in the `defaultDuoApp` section of the config:
   1. `clientID` to the "Client ID"
   2. `clientSecret` to the "Client Secret"
   3. `apiHost` to the "API hostname"
   4. `usernameAttribute` to the SAML attribute which correlates to usernames in your Duo deployment.
3. Add an entry into your authentication processing filter chain with the following contents:

```php
[
    'class' => 'duouniversal:DuoUniversal',
],
```

This will enable the module for the IdP/SP of your choice (or globally if you insert it into the authproc chain in the SimpleSAML global config.php).

The Duo application config used by a particular SP can be changed from the default by adding additional named entries to the `alternateDuoApps` section of the config and then mapping SP EntityIDs to application names in `spDuoOverrides`. Duo can be bypassed per-SP by mapping the SP's EntityID to 'bypass' in the same section. See the comments in the `config-templates/module_duouniversal.php` file for examples.

Based on the original Duo Security module by Kevin Nastase, as forked by Scott Carlson.

- https://github.com/knastase/simplesamlphp-duosecurity
- https://github.com/scottcarlson/simplesamlphp-duosecurity
