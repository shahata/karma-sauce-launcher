# karma-sauce-launcher

> Run your unit tests on [Sauce Labs](https://saucelabs.com/)' browser cloud!

## Installation

Install `karma-sauce-launcher` as a `devDependency` in your package.json:

```bash
npm install karma-sauce-launcher --save-dev
```

## Usage

This launcher is typically used in CI to run your unit tests across many browsers on Sauce Labs. However, you can also use it locally to run/debug tests in browsers not available on your machine.

### Adding launcher to an existing Karma config

To configure this launcher, you need to add two properties to your top-level karma config, `sauceLabs` and `customLaunchers`. The `sauceLabs` object defines global properties for each browser/OS you are testing on while the `customLaunchers` object configures individual browsers:

```js
// karma.conf-ci.js
module.exports = function(config) {
  // Example browsers to run on Sauce Labs
  var customLaunchers = {
    'SL_Chrome': {
      base: 'SauceLabs',
      browserName: 'chrome'
    },
    'SL_Firefox': {
      base: 'SauceLabs',
      browserName: 'firefox',
      version: '26'
    },
    'SL_Safari': {
      base: 'SauceLabs',
      browserName: 'safari',
      platform: 'OS X 10.9',
      version: '7'
    },
    'SL_IE_11': {
      base: 'SauceLabs',
      browserName: 'internet explorer',
      platform: 'Windows 8.1',
      version: '11'
    }
  };

  config.set({

    // The rest of your karma config is here
    // ...

    sauceLabs: {

      // These env vars should be set by your CI
      username: process.env.SAUCE_USERNAME,
      accessKey: process.env.SAUCE_ACCESS_KEY,

      testName: 'Web App Unit Tests',

      // Change this value if you are using a different CI than Travis
      build: process.env.TRAVIS_JOB_ID,
      tags: ['JavaScript', 'Unit Tests']
    },

    // Use customLaunchers object for two config properties
    customLaunchers: customLaunchers,
    browsers: Object.keys(customLaunchers),

    // Change to false if running locally and you want to run tests
    // on file change
    singleRun: true
  });
};
```

For example configs using this launcher, check out the [example directory](https://github.com/karma-runner/karma-sauce-launcher/tree/master/examples) in this project, the [karma-sauce-example repo](https://github.com/saucelabs/sauce-karma-example), or [AngularJS](https://github.com/angular/angular.js/blob/master/.travis.yml).

### `sauceLabs` object options

### username
Type: `String`

Your Sauce Labs username (if you don't have an account, you can sign up [here](https://saucelabs.com/signup/plan/free)). It is recommended to use `process.env.SAUCE_USERNAME` in CI to not have the username in plain text.

### accessKey
Type: `String`

Your Sauce Labs username (if you don't have an account, you can sign up [here](https://saucelabs.com/signup/plan/free)). It is recommended to use `process.env.SAUCE_ACCESS_KEY` in CI to not have the access key in plain text.

### startConnect
Type: `Boolean` Default: `true`

If `true`, Sauce Connect will be started automatically. Set this to `false` if you are launching tests locally and want to start Sauce Connect via [a binary](https://saucelabs.com/docs/connect) or the [Mac](https://saucelabs.com/mac) app in the background to improve test speed.

### tunnelIdentifier
Type: `String`

Sauce Connect can proxy multiple sessions, this is an id of a session.

### tags
Type: `Array of Strings`

Tags to use for filtering jobs in your Sauce Labs account.

### build
Type: `String`

Id of the build currently running. This should come from your CI.

### testName
Type: `String`

Name of the unit test group you are running.

## `customLaunchers` options

You can learn about the available browser/platform combos on the [Sauce Labs platforms page](https://saucelabs.com/platforms). You can copy the config from the node.js config directly to browser config.

### base
Type: `String`
This defines the base configuration for the launcher, in this case it should always be `SauceLabs` so that browsers can share the base Sauce Labs config.

### browserName
Type: `String`

Name of the browser.

### version
Type `String`

Version of the browser to use.

### platform
Type: `String`

Name of platform to run browser on.

### deviceOrientation
Type: `String`
Accepted Values: `['portrait', 'landscape']`

Set this string if you want your unit tests need to run on a particular device orientation on mobile.

### Global options
- `recordVideo` do you wanna record video of the session ? (defaults to `false`)
- `recordScreenshots` do you wanna take screenshots ? (defaults to `true`)

For more information on Karma see the [homepage](http://karma-runner.github.com).
