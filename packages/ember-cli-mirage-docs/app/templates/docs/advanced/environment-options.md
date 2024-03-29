# Environment options

Set these options via the `ENV['ember-cli-mirage']` variable in your `config/environment.js` file:

```js
// config/environment.js
...
if (environment === 'production') {
  ENV['ember-cli-mirage'] = {
    enabled: true
  };
}
```

## enabled

By default, your Mirage server will run in test mode, and in development mode as long as the `--proxy` option isn't passed. To change this default behavior, set `enabled` to either true or false in your ENV config.

For example, to enable in production (e.g. to share a working prototype before your server is ready):

```js
// config/environment.js
...
if (environment === 'production') {
  ENV['ember-cli-mirage'] = {
    enabled: true
  };
}
```

To disable in development,

```js
// config/environment.js
...
if (environment === 'development') {
  ENV['ember-cli-mirage'] = {
    enabled: false
  };
}
```

## trackRequests

A boolean that controls whether [Pretender's `trackRequests` feature](https://github.com/pretenderjs/pretender#tracking-requests) is enabled. By default it is disabled to avoid memory issues during long development sessions.

This should be set in the `mirage/config.js` options. Defaults to `false`.

```js
export default function(config) {
  let finalConfig = {
    ...config,
    trackRequests: true,
    models: { ...discoverEmberDataModels(), ...config.models },
    routes,
  };

  return createServer(finalConfig);
}
```

This feature is useful for asserting against HTTP requests and responses during tests. See the "Asserting against handled requests and responses" section of the {{docs-link 'Assertions guide' 'docs.testing.assertions'}} to learn more.


## excludeFilesFromBuild

Defaults to `false`.

By default, Mirage's files are included in your Ember app's build in non-production environments. This is in case you want to use Mirage via `ember serve` by visiting `/tests`, since that's an app with a build-time environment of `development` but a run-time environment of `test`.

You can explicilty exclude Mirage's files from your Ember app's build by setting `excludeFilesFromBuild` to `true`.

## useDefaultPassthroughs

Defaults to `true`.

If true, Mirage will add some default passthrough routes to your server. Currently we add a single route

- http://localhost:0/chromecheckurl

which is used by iOS for URL verification.

## directory

Configure which directory contains your Mirage server definition. The default directory is `/mirage` (from the root of your project).

For example, to have your server definition under `/app/mirage`,

```js
// config/environment.js
...
ENV['ember-cli-mirage'] = {
  directory: 'app/mirage'
};
```

## discoverEmberDataModels

Tells Mirage whether to automatically infer its schema from the host application's Ember Data models and relationships.

Defaults to true. If Ember Data models are present, predefines Mirage's models and relationships. If Ember Data models are not present, has no effect.

You can disable by setting to `false`. You might want to do this if you run into an edge case/issue where the autodiscovery code causes issues in your app's environment (for example, if you have a complex engines setup).

```js
// config/environment.js
...
ENV['ember-cli-mirage'] = {
  discoverEmberDataModels: false
};
```
