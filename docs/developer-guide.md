# Developer guide

### System requirements

**zulip-mobile** only supports development on OS X at the moment since it currently only targets iOS devices. In the future we hope to support development of an Android version on all major operating systems.

* Mac OS X (El Capitan recommended)
* Xcode (7+ recommended)
* Node (for `npm`)
* Google Chrome (for React Native debugger)


## Setting up a dev environment

Setting up a dev environment should be as simple as running the commands below in your terminal:
```
git clone https://github.com/zulip/zulip-mobile.git
cd zulip-mobile
npm install
```

Unlike the [Zulip](https://github.com/zulip/zulip) server project, we use the host machine directly for development instead of provisioning a VM.

You'll probably also want to install and provision a [Zulip dev VM](https://github.com/zulip/zulip/blob/master/README.dev.md) to use for testing.


## Running on iOS simulator
`npm start` will launch a new terminal with the React Native packager and open up the app in the iOS simulator.

It will also launch a browser tab in Chrome with the React Native debugger. `console.log` statements in React Native will end up in the JS console on this tab.

## Running on an iOS device
This process needs improvement and has too many manual steps at the moment.

First, you'll need to connect your dev machine and iOS device to the same network. If you're running Zulip inside of a VM, you may also need to configure your VM to use a public network. See more information on this [here](https://www.vagrantup.com/docs/networking/public_network.html).

Next, you'll need to change all instances of `localhost:9991` in both the `/src` directory and the Xcode iOS project (location in `/ios`) to point to the IP and port of your Vagrant VM.

Finally, run the Xcode project inside of `/ios` with your iOS device as the target.


## Contributing
**zulip-mobile** is in a very early experimental phase. We welcome contributions to help improve the foundation and to add features.

To contribute, browse open issues [here](https://github.com/zulip/zulip-mobile/issues) and submit a pull request. Please follow the [commit discipline](https://zulip.readthedocs.io/en/latest/code-style.html#version-control) of the Zulip server project.


## Architecture

### High-level design

**zulip-mobile** uses the Redux framework for state management and information flow. Please read the [Redux docs](http://redux.js.org/index.html) for more information on the Redux architecture and terminology (such as actions, reducers and stores).

At a high-level, all state in the app is immutable and stored in one place. Modifying state requires new copies of each data structure or structural sharing via [Immutable.js](https://facebook.github.io/immutable-js/).

### Code structure

#### Directories

* `/node_modules` - dependencies installed by `npm`
* `/android` - auto-generated by React Native
* `/ios` - auto-generated by React Native
* `/docs` - developer documentation
* `/src` - Javascript source code

In general most of the work will be inside of the `/src` directory. The only reason to touch the `/ios` or `/android` directories would be to add native modules (which we aren't using at the moment).

#### Top-level files
* `package.json` - specifies `npm` dependencies and scripts for the project
* `.babelrc` - config file for [Babel](https://babeljs.io/) transpiler (ES6 -> ES5)
* `.eslintrc` - config file for linting rules (we're using Airbnb rules)
* `.flowconfig` - config file for [Flow](https://flowtype.org/) static type checker (unused)
* `index.ios.js` - entry point for iOS app
* `index.android.js` - entry point for Android app (unused for now)

#### `/src` directory

The source directory is broken up into subdirectories corresponding to components of the app:
* `account` - login, logout, and user account
* `api` - clients for the Zulip server API
* `compose` - composing messages
* `lib` - miscellaneous shared libraries
* `message` - messages and groups of related messages
* `nav` - navigation
* `stream` - stream of messages
* `streamlist` - stream selection

`ZulipApp.js` contains the top-level React component for the app and `reducers.js` contains the top-level reducer.

## Testing

### Unit tests
`npm test` runs the unit test suite.

Our tests are written using the [Mocha](https://mochajs.org/) framework with the [Chai](http://chaijs.com/) assertion library.

To write a test, place a Javascript file with the `-test.js` suffix in the `__tests__` directory inside of any subfolder of `/src`. The test will be automatically picked up by the test runner.


### Functional tests
Functional tests have not been set up at the moment. We plan to use [Appium](http://appium.io/).


## Linting
`npm run lint` checks the codebase against our linting rules. We're using the AirBnB [ES6](https://github.com/airbnb/javascript) and [React](https://github.com/airbnb/javascript/tree/master/react) style guides.

Note: the master branch does not pass all lint rules at the moment.
