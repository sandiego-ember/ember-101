Now that we have posts, are able to list them, and view individual posts we would like to add functionality to comment on blog posts.

# Comments

We've seen how to use `ember generate model` before to create our models. In this case, we want the comment to be a `string`, and our Rails API defines the content of these comments as `content`.

```console
$ ember generate model comment content:string
installing model
  create app/models/comment.js
installing model-test
  create tests/unit/models/comment-test.js
```

We've seen this before.  Let's take a look at the model and see what it contains:

```js
import Model from 'ember-data/model';
import attr from 'ember-data/attr';

export default Model.extend({
  content: attr('string')
});
```

But our comments need to be aware of our blog posts, and vice versa. We're going to add a one-to-many relationship which is built into Ember Data for us. In order to use it, we need to import the base ember-data module.

The comments need a `belongsTo` since a comment belongs to a blog post:

```js
import Model from 'ember-data/model';
import attr from 'ember-data/attr';
import DS from 'ember-data';

const { belongsTo } = DS;

export default Model.extend({
  content: attr('string'),
  blogPost: belongsTo('blog-post')
});
```

Whereas the blog posts need a `hasMany` since a blog post has many comments:

```js
import Model from 'ember-data/model';
import attr from 'ember-data/attr';
import DS from 'ember-data';

const { hasMany } = DS;

export default Model.extend({
  title: attr('string'),
  body: attr('string'),
  publishedDate: attr('date'),
  comments: hasMany('comment')
});
```

Ember has a number of different ways to define relationships like this. You can [learn more here][ds-relationships].

## Testing with dependencies

Remember when we had only one model with no relationships to other models? Those are easy to unit test in isolation. At this point, unfortunately, your tests should be failing, because they can't find either the `comment` dependency or `blogPost` dependency. When the tests need related models, you must tell Ember's test runner to load these dependencies.

Find the following block in `tests/unit/models/blog-post-test.js`.

```js
moduleForModel('blog-post', 'Unit | Model | blog post', {
  // Specify the other units that are required for this test.
  needs: []
});
```

You must fill out the `needs` property to tell Ember about the comment dependency:

```js
  needs: ['model:comment']
```

For the same block in `tests/unit/models/comment-test.js`, to tell Ember about the blog post dependency:

```js
  needs: ['model:blogPost']
```

# Show comments on a blog post

Let's get comments to show up on a blog post by adding to our `app/templates/blog-post.hbs`:

```handlebars
<article>
  <header class="page-header">
    <h1>{{model.title}}</h1>
  </header>
  <p>{{model.body}}</p>

  <h2>Comments</h2>
  <ul>
  {{#each model.comments as |comment|}}
    <li>{{comment.content}}</li>
  {{/each}}
  </ul>
</article>
```

We first loop through all the `model.comments` with Ember's each syntax, defining `|comment|` as the local variable we use to access each `comment` model. Inside of this loop, we output the comment content we defined in our model with `content: attr('string')` with `{{comment.content}}`.

But we also want a good user experience for our readers. They need to know when comments are being loaded and when there aren't any comments at all!

For the loading case, we have to think about sort of object our `model.comments` happens to be. It's a [`PromiseManyArray`][promise-many-array], and in our templates we can hook into the loading states made available by [`PromiseProxyMixin`][promise-proxy-mixin]. The available states are:
* `isPending`
* `isSettled`
* `isRejected`
* `isFulfilled`

We can update our template to make use of this new property:

```handlebars
<h2>Comments</h2>
{{#if model.comments.isPending}}
  <p>Loading...</p>
{{else}}
  <ul>
  ...
  </ul>
{{/if}}
```

Now our users know when comments are still loading and aren't presented with an ugly empty list of comments. If we wanted to go above and beyond, we could add a loading indicator here to indicate the comments are being loaded, but we'll move on.

We also want to handle the empty case. We can make use of Handlebars' if construct here:

```handlebars
<h2>Comments</h2>
{{#if model.comments.isPending}}
  <p>Loading...</p>
{{else}}
  {{#if model.comments}}
    <ul>
    {{#each model.comments as |comment|}}
      <li>{{comment.content}}</li>
    {{/each}}
    </ul>
  {{else}}
    <p>There are no comments yet.</p>
  {{/if}}
{{/if}}
```

If there are comments, we iterate over them. If there are none, we add a helpful message to the user to let them know there aren't any comments yet.

# Submitting a comment

We can see existing comments on our blog but users have no way to submit comments yet, so let's make a form for users to submit comments.

Let's make a commenting component to handle the form and submission for a new comment. To begin, we'll use a generator.

```console
$ ember generate component comment-form
installing component
  create app/components/comment-form.js
  create app/templates/components/comment-form.hbs
installing component-test
  create tests/integration/components/comment-form-test.js
```

Let's add this new component to our `blog-post.hbs`

```handlebars
<hr>

{{comment-form}}
```

Now if we open up our `comment-form.hbs` component template we can add in our form. When we open this up, we'll initially see a `{{yeild}}` statement. Components can either be rendered alone or can wrap in a block form. For our example we're going to keep it simple and render only the component. Replace the existing `{{yield}}` statement with the fllowing:

```handlebars
<form {{action 'saveComment' on='submit'}}>
  <div class="form-group">
    {{textarea value=commentContent class='form-control' rows='3'}}
  </div>
  <button type="submit" class="btn btn-primary">Add My Comment!</button>
</form>
```

If we visit the browser, we should see our comment form now on an individual post page. Though, it won't be wired up yet to work.

## Component actions
In Ember-land you'll often hear the phrase "data down, actions up". This is applicable to components as well. We want to pass any data the component needs to render properly in when we call it, and any actions that the component needs to trigger will happen outside of the component.

For our comment form, we don't need to pass any data down but we will need to save the comment when the form submits. This is an action and should be handled outside of the component code, often in the `route`.

Let's create an action inside of our `blog-post` route. If you remember, we actually **deleted** our blog-post route (and test) in the last section. We can add it back in again easily. The `generate` command will prompt us when it encounters already existing files, like our template for `blog-post` which we kept, so that we don't overwrite it.

```console
$ ember generate route blog-post
installing route
? Overwrite app/templates/blog-post.hbs? No, skip
  create app/routes/blog-post.js
  skip app/templates/blog-post.hbs
updating router
  add route blog-post
installing route-test
  create tests/unit/routes/blog-post-test.js
```

Now we can open up our `blog-post` route and add an action to handle the comment submission:

```js
import Ember from 'ember';

export default Ember.Route.extend({
  actions: {
    saveComment(comment) {
      let blogPost = this.controller.get('model');
      this.store.createRecord('comment', {
        blogPost: blogPost,
        content: comment
      }).save();
    }
  }
});
```

Actions in Ember are handled in the `actions: {}` hash on an object. We're going to assume that our component will pass us the content of our comment to save. We can use `store.createRecord` which takes the type of model as well as the hash to save and returns a promise.

We have to pass this action into our component when we create it so it knows what action to use on submission so let's add it back in our `blog-post.hbs`:

```handlebars
{{comment-form saveComment='saveComment'}}
```

We're almost there! Now we just need to handle the action behavior inside the component. The component is still responsible for sending the action up. Let's open up our `comment-form.js` and add this:

```js
import Ember from 'ember';

export default Ember.Component.extend({
  commentContent: '',

  actions: {
    saveComment() {
      let comment = this.get('commentContent');
      this.sendAction('saveComment', comment);
      this.set('commentContent', '');
    }
  }
});
```

Now when we browse to an individual post we should be able to add a comment. Give it a try!

[ds-relationships]: https://guides.emberjs.com/v2.4.0/models/relationships/
[promise-many-array]: http://emberjs.com/api/data/classes/DS.PromiseManyArray.html
[promise-proxy-mixin]: http://emberjs.com/api/classes/Ember.PromiseProxyMixin.html