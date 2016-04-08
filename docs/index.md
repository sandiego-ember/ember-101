Welcome to Ember-CLI 101 workshop hosted by [San Diego Ember][san diego ember].

# Pre-event setup instructions

0. [Install Git][git-scm]
0. [Install Node.js][node-install]
0. Setup NPM for non-sudo installation
    0. The easiest way to do this is by checking out [this awesome shell script][npm-no-sudo-script] that will do it for you
    0. NPM is the node package manager. It will automatically be installed when you install node.
    0. NPM installs packages locally (within the directory it is invoked in) for per-project modules, or globally for packages you want accessible everywhere.
    0. However, by default NPM installs global packages in a root-restricted location, requiring SUDO to install. This creates a huge headache. As an alternative, before you install any packages, follow [this guide][npm-g-without-sudo] to configure your NPM to install in your home directory without requiring sudo.
0. Install [Bower][bower]: `npm i -g bower`
0. Install [Ember-CLI][ember-cli]: `npm i -g ember-cli`
0. And create a new project named `workshop`: `ember new workshop`
0. Move into the `workshop` directory: `cd workshop`

# Reduce the glue

Web application development can involve a lot of repetition.  Attempts to reduce the repetition involved in web development has given rise to a variety of scaffolding tools and best practices. These scaffolding tools are all trying to do the same thing: reduce the amount of work necessary to "get started" by providing a set of "best practices" that are enabled default.  These choices include things like:

0. Application directory structure
0. Generators for common components
0. Modularity choices (AMD/node modules/etc)
0. Build system
0. Asset compilation & minification
0. Testing framework and setup

# Ember-CLI

[Ember-CLI][ember-cli] provides choices for all of the aforementioned areas.  We'll dive into some of these choices in more detail later but at a high level Ember-CLI builds in:

0. A directory structure which we'll explore more later
0. Generators for all common components
0. ES6 modules transpiled to AMD
0. Broccoli build tool for builds. (Fast and extensible with plugin architecture
0. Asset minification also via Broccoli
0. QUnit for testing

# Modules

Modules allow you to divide logical portions of code into smaller, functional pieces and include them as needed. As your application grows, smaller pieces of functional code become easier to manage, support, maintain and test. To learn more about JS Modules, check out [jsmodules.io][jsmodules]

# Naming best practices

0. Code
    0. `TitleCase` naming of classes
    0. `camelCase` naming of attributes
    0. use modules, avoid globals
    0. reusable code → components, mixins, add-ons
0. Files
    0. `kebab-case-naming.js`
    0. children in subdirectory → `routes/invoices/edit.js` & `routes/invoices/new.js`

[bower]: http://bower.io/
[ember-cli]: http://ember-cli.com
[san diego ember]: http://www.meetup.com/sandiego-ember/
[npm-g-without-sudo]: https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md
[node-install]: https://nodejs.org/download/
[git-scm]: http://git-scm.com/downloads
[npm-no-sudo-script]: https://github.com/glenpike/npm-g_nosudo
[jsmodules]: http://jsmodules.io
[ember-cli]: http://ember-cli.com