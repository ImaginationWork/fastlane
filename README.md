<h3 align="center">
  <a href="https://github.com/fastlane/fastlane">
    <img src="assets/fastlane.png" width="150" />
    <br />
    fastlane
  </a>
</h3>
<p align="center">
  <a href="https://github.com/fastlane/deliver">deliver</a> &bull; 
  <a href="https://github.com/fastlane/snapshot">snapshot</a> &bull; 
  <a href="https://github.com/fastlane/frameit">frameit</a> &bull; 
  <a href="https://github.com/fastlane/pem">pem</a> &bull; 
  <a href="https://github.com/fastlane/sigh">sigh</a> &bull; 
  <b>produce</b> &bull;
  <a href="https://github.com/fastlane/cert">cert</a> &bull;
  <a href="https://github.com/fastlane/spaceship">spaceship</a> &bull;
  <a href="https://github.com/fastlane/pilot">pilot</a> &bull;
  <a href="https://github.com/fastlane/boarding">boarding</a> &bull;
  <a href="https://github.com/fastlane/gym">gym</a> &bull;
  <a href="https://github.com/fastlane/scan">scan</a> &bull;
  <a href="https://github.com/fastlane/match">match</a>
</p>
-------

<p align="center">
  <img src="assets/produce.png" height="110">
</p>

produce
============

[![Twitter: @FastlaneTools](https://img.shields.io/badge/contact-@FastlaneTools-blue.svg?style=flat)](https://twitter.com/FastlaneTools)
[![License](https://img.shields.io/badge/license-MIT-green.svg?style=flat)](https://github.com/fastlane/produce/blob/master/LICENSE)
[![Gem](https://img.shields.io/gem/v/produce.svg?style=flat)](http://rubygems.org/gems/produce)

###### Create new iOS apps on iTunes Connect and Dev Portal using your command line

Get in contact with the developer on Twitter: [@FastlaneTools](https://twitter.com/FastlaneTools)

-------
<p align="center">
    <a href="#features">Features</a> &bull;
    <a href="#installation">Installation</a> &bull;
    <a href="#usage">Usage</a> &bull;
    <a href="#how-does-it-work">How does it work?</a> &bull;
    <a href="#tips">Tips</a> &bull;
    <a href="#need-help">Need help?</a>
</p>

-------

<h5 align="center"><code>produce</code> is part of <a href="https://fastlane.tools">fastlane</a>: connect all deployment tools into one streamlined workflow.</h5>


# Features

- **Create** new apps on both iTunes Connect and the Apple Developer Portal
- **Modify** Application Services on the Apple Developer Portal
- **Create** App Groups on the Apple Developer Portal
- **Associate** apps with App Groups on the Apple Developer Portal
- Support for **multiple Apple accounts**, storing your credentials securely in the Keychain

##### [Like this tool? Be the first to know about updates and new fastlane tools](https://tinyletter.com/krausefx)

# Installation
    sudo gem install produce

# Usage

## Creating a new application

    produce

To get a list of all available parameters:

    produce --help

```
  Commands:
    associate_group  Associate with a group, which is create if needed or simply located otherwise
    create           Creates a new app on iTunes Connect and the Apple Developer Portal
    disable_services Disable specific Application Services for a specific app on the Apple Developer Portal
    enable_services  Enable specific Application Services for a specific app on the Apple Developer Portal
    group            Ensure that a specific App Group exists
    help             Display global or [command] help documentation

  Global Options:
    -u, --username STRING Your Apple ID Username (PRODUCE_USERNAME)
    -a, --app_identifier STRING App Identifier (Bundle ID, e.g. com.krausefx.app) (PRODUCE_APP_IDENTIFIER)
    -e, --bundle_identifier_suffix STRING App Identifier Suffix (Ignored if App Identifier does not ends with .*) (PRODUCE_APP_IDENTIFIER_SUFFIX)
    -q, --app_name STRING App Name (PRODUCE_APP_NAME)
    -z, --app_version STRING Initial version number (e.g. '1.0') (PRODUCE_VERSION)
    -y, --sku STRING     SKU Number (e.g. '1234') (PRODUCE_SKU)
    -m, --language STRING Primary Language (e.g. 'English', 'German') (PRODUCE_LANGUAGE)
    -c, --company_name STRING The name of your company. Only required if it's the first app you create (PRODUCE_COMPANY_NAME)
    -i, --skip_itc       Skip the creation of the app on iTunes Connect (PRODUCE_SKIP_ITC)
    -d, --skip_devcenter  Skip the creation of the app on the Apple Developer Portal (PRODUCE_SKIP_DEVCENTER)
    -b, --team_id STRING The ID of your team if you're in multiple teams (PRODUCE_TEAM_ID)
    -l, --team_name STRING The name of your team if you're in multiple teams (PRODUCE_TEAM_NAME)
    -h, --help           Display help documentation
    -v, --version        Display version information
```

## Enabling / Disabling Application Services

If you want to enable Application Services for an App ID (HomeKit and HealthKit in this example):

    produce enable_services --homekit --healthkit

If you want to disable Application Servies for an App ID (iCloud in this case):

    produce disable_services --icloud

If you want to create a new App Group:

    produce group -g group.krausefx -n "Example App Group"

If you want to associate an app with an App Group:

    produce associate_group -a com.krausefx.app group.krausefx

# Parameters

Get a list of all available options using

    produce enable_services --help

```
    --app-group          Enable App Groups
    --associated-domains Enable Associated Domains
    --data-protection STRING Enable Data Protection, suitable values are "complete", "unlessopen" and "untilfirstauth"
    --healthkit          Enable HealthKit
    --homekit            Enable HomeKit
    --wireless-conf      Enable Wireless Accessory Configuration
    --icloud STRING      Enable iCloud, suitable values are "legacy" and "cloudkit"
    --inter-app-audio    Enable Inter-App-Audio
    --passbook           Enable Passbook
    --push-notification  Enable Push notification (only enables the service, does not configure certificates)
    --vpn-conf           Enable VPN Configuration
```

    produce disable_services --help

```
    --app-group          Disable App Groups
    --associated-domains Disable Associated Domains
    --data-protection    Disable Data Protection
    --healthkit          Disable HealthKit
    --homekit            Disable HomeKit
    --wireless-conf      Disable Wireless Accessory Configuration
    --icloud             Disable iCloud
    --inter-app-audio    Disable Inter-App-Audio
    --passbook           Disable Passbook
    --push-notification  Disable Push notifications
    --vpn-conf           Disable VPN Configuration
```

## Environment Variables

All available values can also be passed using environment variables, run `produce --help` to get a list of all available parameters.

## [`fastlane`](https://github.com/fastlane/fastlane) Integration

Your `Fastfile` should look like this
```ruby
lane :appstore do
  produce(
    username: 'felix@krausefx.com',
    app_identifier: 'com.krausefx.app',
    app_name: 'MyApp',
    language: 'English',
    app_version: '1.0',
    sku: '123',
    team_name: 'SunApps GmbH' # only necessary when in multiple teams
  )

  deliver
end
```

To use the newly generated app in `deliver`, you need to add this line to your `Deliverfile`:

```ruby
apple_id ENV['PRODUCE_APPLE_ID']
```

This will tell `deliver`, which `App ID` to use, since the app is not yet available in the App Store.

You'll still have to fill out the remaining information (like screenshots, app description and pricing). You can use [deliver](https://github.com/fastlane/deliver) to upload your app metadata using a CLI

## How is my password stored?
`produce` uses the [password manager](https://github.com/fastlane/credentials_manager) from `fastlane`. Take a look the [CredentialsManager README](https://github.com/fastlane/credentials_manager) for more information.

# Tips
## [`fastlane`](https://fastlane.tools) Toolchain

- [`fastlane`](https://fastlane.tools): Connect all deployment tools into one streamlined workflow
- [`deliver`](https://github.com/fastlane/deliver): Upload screenshots, metadata and your app to the App Store
- [`snapshot`](https://github.com/fastlane/snapshot): Automate taking localized screenshots of your iOS app on every device
- [`frameit`](https://github.com/fastlane/frameit): Quickly put your screenshots into the right device frames
- [`pem`](https://github.com/fastlane/pem): Automatically generate and renew your push notification profiles
- [`sigh`](https://github.com/fastlane/sigh): Because you would rather spend your time building stuff than fighting provisioning
- [`cert`](https://github.com/fastlane/cert): Automatically create and maintain iOS code signing certificates
- [`spaceship`](https://github.com/fastlane/spaceship): Ruby library to access the Apple Dev Center and iTunes Connect
- [`pilot`](https://github.com/fastlane/pilot): The best way to manage your TestFlight testers and builds from your terminal
- [`boarding`](https://github.com/fastlane/boarding): The easiest way to invite your TestFlight beta testers
- [`gym`](https://github.com/fastlane/gym): Building your iOS apps has never been easier
- [`scan`](https://github.com/fastlane/scan): The easiest way to run tests of your iOS and Mac app
- [`match`](https://github.com/fastlane/match): Easily sync your certificates and profiles across your team using git

##### [Like this tool? Be the first to know about updates and new fastlane tools](https://tinyletter.com/krausefx)

# Need help?
Please submit an issue on GitHub and provide information about your setup

# License
This project is licensed under the terms of the MIT license. See the LICENSE file.

> This project and all fastlane tools are in no way affiliated with Apple Inc. This project is open source under the MIT license, which means you have full access to the source code and can modify it to fit your own needs. All fastlane tools run on your own computer or server, so your credentials or other sensitive information will never leave your own computer. You are responsible for how you use fastlane tools.
