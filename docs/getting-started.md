# Ready to code!

Now that we have reviewed the contents ember-cli sets us up with for development, we are ready to get coding. One of the many commands ember-cli provides us is `serve` which will launch the server with live-reload. Any time we make changes to the application we should see this instantly reflected in any of the browser we have open to the server.

0. `ember serve`
0. Visit [http://localhost:4200](http://localhost:4200) in your browser

# Install Bootstrap

Let's use Bootstrap to make our website look nice. This step isn't strictly necessary but it'll make our application snazzier without us having to do much styling work.

```console
$ bower install bootstrap --save
```

Now we need to include the Bootstrap CSS into our build process.  Let's add the following to our `ember-cli-build.js` just above `return app.toTree();`:

```js
app.import('bower_components/bootstrap/dist/css/bootstrap.css');
```

Our `ember-cli-build.js` should look something like this now:

```js
/*jshint node:true*/
/* global require, module */
var EmberApp = require('ember-cli/lib/broccoli/ember-app');

module.exports = function(defaults) {
  var app = new EmberApp(defaults, {
    // Add options here
  });

  // Use `app.import` to add additional libraries to the generated
  // output files.
  //
  // If you need to use different assets in different
  // environments, specify an object as the first parameter. That
  // object's keys should be the environment name and the values
  // should be the asset to use in that environment.
  //
  // If the library that you are including contains AMD or ES6
  // modules that you would like to import into your application
  // please specify an object with the list of modules as keys
  // along with the exports of each module as its value.

  // Add bootstrap to our build
  app.import('bower_components/bootstrap/dist/css/bootstrap.css');

  return app.toTree();
};
```

**ProTip™** As we mentioned before, ember-cli uses live-reloading to persist changes to the browser without the need of reloading. As long as `http://localhost:4200/` is open, you'll see changes immediately. Neat! However, if you edit `ember-cli-build.js`, like we just did in this step, you will need to **kill** and **restart** `ember serve`. Then refresh `http://localhost:4200/`.

The font of our header should have changed.

Now let's add a big header introducing our blog.  Let's update our `application.hbs` file to add a jumbotron header and wrap our page content in a Bootstrap `container`:

```handlebars
{% raw %}
<div class="jumbotron">
  <div class="container">
    <h1>Bernice's Blog</h1>
  </div>
</div>

<div class="container">
  {{outlet}}
</div>
{% endraw %}
```

Our site should have refreshed in our web browser now, revealing a big header for our blog.

# Checkout our completed version

Throughout the workshop if you need to compare your app at a point in time to the steps in this workshop, head on over to the [ember-101-app repository][ember-101-app] where each step has been tagged to match this walk-through.

If you wanted to checkout the code at the end of this step, check out the `getting-started` [tag][getting-started-tag].

[ember-101-app]: https://github.com/sandiego-ember/ember-101-app/tree/master
[getting-started-tag]: https://github.com/sandiego-ember/ember-101-app/tree/getting-started