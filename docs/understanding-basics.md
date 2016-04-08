# Project Organization: High level

In the [Pre-event setup instructions][pre-event] section you generated a new ember project called `workshop`. Let's step inside and start exploring:

```console
$ cd workshop
$ ls -1
README.md
app
bower.json
bower_components
config
ember-cli-build.js
node_modules
package.json
public
testem.js
tests
vendor
```

## Node Moduules

The first thing to notice is the file `package.json` and the directory `node_modules`. These are from [NPM][npm], and if you're new to NPM, take a look at what is in the `package.json` file. This file contains information about what NPM modules are required to run and develop our app. You'll see that the packages needed for broccoli and Ember-CLI are specified here. When using Ember-CLI you won't often edit this directly. If you were to install any Ember-CLI addons yourself, you would see them show up in here as well. The packages specified in `package.json` will be installed in the `node_modules` directory.

## Bower Components

The next thing to look at is the file `bower.json` and the `bower_components` directory. These are similar to `package.json` and `node_modules`. Bower has become the standard for front-end package management and our Ember-CLI application will use it to manage some of our dependencies. If you open up `bower.json` you'll see that our application comes out of the box with not only Ember but Ember Data (for data persistence), and QUnit (for testing).

## Tests

Ember-CLI comes out-of-the box with a testing framework and provides some helpers to make testing easier. You can test models, routes, controllers and components, and you can test user flows.

Unit tests allow us to focus on specific functionality of a module and do not require the entire Ember application be running. Acceptance tests, also called integration or acceptance tests, are used to test the flow of your app. They emulate user interactions throughout your application and using helpers you can make assertions about the expected functionality.

## Public and Vendor

You may be wondering where images, fonts and other miscellaneous asset files should go. The answer is the `public` directory. These will be served at the root of your application.

Similarly, you may have JavaScript or CSS dependencies that are not in bower. These can be stored in the `vendor` directory. Loading vendor files is not something we will cover in this workshop.

## App

The `app` directory is where we're going to put all of our own application code.  It is carefully structured with an appropriate place for each type of module:

```console
$ ls -1
app.js
components
controllers
helpers
index.html
models
resolver.js
router.js
routes
styles
templates
```

Some of these may sound familiar to you, while others may be brand new.  Don't worry yet if you don't know what all of these different pieces are.  We'll get to them one by one.

[pre-event]: index.md#pre-event-setup-instructions
[npm]: https://www.npmjs.com/