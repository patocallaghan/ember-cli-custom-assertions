# ember-cli-custom-assertions # [![Build Status](https://secure.travis-ci.org/DockYard/ember-cli-custom-assertions.svg?branch=master)](http://travis-ci.org/DockYard/ember-cli-custom-assertions)

**[ember-cli-custom-assertions is built and maintained by DockYard, contact us for expert Ember.js consulting](https://dockyard.com/ember-consulting)**.

## About ##

Add custom assertions to your Ember test suite.

## Installing ##

`ember install ember-cli-custom-assertions`

## Looking for help? ##

If it is a bug [please open an issue on GitHub](https://github.com/dockyard/ember-cli-custom-assertions/issues).

## Usage ##

Add new assertions to `test/assertions`. Then use on the `assert` object in your test suite.

For example:

```javascript
// tests/assertions/contains.js
export default function(context, element, text, message) {
  message = message || `${element} should contain "${text}"`;
  let actual = context.$(element).text();
  let expected = text;
  let result = !!actual.match(new RegExp(expected));

  this.pushResult({ result, actual, expected, message });
}

// tests/acceptance/foo-test.js
test('foo is bar', function(assert) {
  visit('/');

  andThen(function() {
    assert.contains('.foo', 'Foo Bar');
  });
});
```

*Note: hyphenated file names like `tests/assertions/double-trouble.js`
will be camelized: `assert.doubleTrouble`*

### Blueprint

You can generate a new assertion by using the `assertion` blueprint:

```
ember g assertion double-trouble
```

### Assertion

A `context` is always injected as the first argument. You don't need to
pass a context when calling the assertion.

```js
// good
assert.contains('.foo', 'Foo bar');

// bad
assert.contains(app, '.foo', 'Foo bar');
```

### Setup

You must inject the assertions to use them in tests.

#### New testing API

```javascript
import { module, test } from 'qunit';
import { setupTest } from 'ember-qunit';
import setupCustomAssertions from 'ember-cli-custom-assertions/test-support';

module('default setup', function(hooks) {
  setupTest(hooks);
  setupCustomAssertions(hooks);

  test('can inject custom assertions', function(assert) {
    assert.contains('.foo', 'Foo bar');
  });
});
```

#### Old testing API

```javascript
// ...
import { assertionInjector, assertionCleanup } from 'ember-cli-custom-assertions/test-support';

module('Acceptance | foo', {
  beforeEach: function() {
    var application = startApp();
    assertionInjector(application);
  },

  afterEach: function() {
    Ember.run(application, 'destroy');
    assertionCleanup(application);
  }
});
```

## Authors ##

* [Brian Cardarella](http://twitter.com/bcardarella)

[We are very thankful for the many contributors](https://github.com/dockyard/ember-cli-custom-assertions/graphs/contributors)

## Versioning ##

This library follows [Semantic Versioning](http://semver.org)

## Want to help? ##

Please do! We are always looking to improve this library. Please see our
[Contribution Guidelines](https://github.com/dockyard/ember-cli-custom-assertions/blob/master/CONTRIBUTING.md)
on how to properly submit issues and pull requests.

## Legal ##

[DockYard](http://dockyard.com/ember-consulting), Inc &copy; 2015

[@dockyard](http://twitter.com/dockyard)

[Licensed under the MIT license](http://www.opensource.org/licenses/mit-license.php)
