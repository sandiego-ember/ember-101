# Accessing our API with ember-data

Ember is a client side framework and so when we have data that we want to persist we need a back-end API.  We want an API to serve up our blog posts and allow users to view and submit comments.

We could use fixtures or a mock API but some friendly back-end developers have already made a working API for us so let's use that.

Our API is setup at https://sandiego-ember-cli-101.herokuapp.com supporting the following endpoints

<table class="table table-bordered table-striped">
  <colgroup>
    <col class="col-xs-1">
    <col class="col-xs-3">
    <col class="col-xs-5">
  </colgroup>
  <thead>
    <tr>
      <th>Verb</th><th>path</th><th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
        <td>GET</td><td>/blog-posts</td><td>List of blog posts</td>
    </tr>
    <tr>
        <td>GET</td><td>/blog-posts/:id</td><td>Retrieve a post</td>
    </tr>
    <tr>
        <td>PUT</td><td>/blog-posts/:id</td><td>Update a post</td>
    </tr>
    <tr>
        <td>DELETE</td><td>/blog-posts/:id</td><td>Delete a post</td>
    </tr>
    <tr>
        <td>GET</td><td>/comments</td><td>List of blog comments</td>
    </tr>
    <tr>
        <td>POST</td><td>/comments</td><td>Add a blog comment</td>
    </tr>
    <tr>
        <td>GET</td><td>/comments/:id</td><td>Retrieve a comment</td>
    </tr>
    <tr>
        <td>PUT</td><td>/comments/:id</td><td>Update a comment</td>
    </tr>
    <tr>
        <td>DELETE</td><td>/comments/:id</td><td>Delete a comment</td>
    </tr>
  </tbody>
</table>

Our API uses `snake_case` in the JSON it sends, the convention for Ruby on Rails APIs. Ember expects everything to be `camelCase`, so how can we connect these two nicely? Fortunately, we can use an Ember Data adapter to consumer our API and adapt it to the style we use in Ember.

## Application Adapter

We can set up an adapter at the level of an individual model, but since we'll be using the same API for all our models, let's set one up for the entire application:

```console
$ ember generate adapter application
version: 0.2.2
  installing
    create app/adapters/application.js
  installing
    create tests/unit/adapters/application-test.js
```

Let's open up that adapter and see what is there:

```js
import JSONAPIAdapter from 'ember-data/adapters/json-api';

export default JSONAPIAdapter.extend({
});
```

We're using an Ember Data built-in adapter called the JSONAPIAdapter. Building a custom adapter isn't too hard, but we don't need to because we're going to use the default.

Finally, to point our Ember app at the API we've set up, let's go back to our server, hit `CTRL-C` to stop it, and restart `ember serve` using the proxy option to point Ember to the API we want to access:

```console
$ ember serve --proxy https://ember-101-api.herokuapp.com
Proxying to https://ember-101-api.herokuapp.com
Livereload server on http://localhost:49152
Serving on http://localhost:4200/
```
