# PostCSS Parser Tests

<img align="right" width="95" height="95"
     title="Philosopher’s stone, logo of PostCSS"
     src="http://postcss.github.io/postcss/logo.svg">

This project contains base tests for every [PostCSS] CSS parser, including:

* 24 CSS files to test extreme cases of the CSS specification.
* Integration tests by popular website styles to test CSS from the wild.

These tests are useful for any CSS parser, not just parsers within the PostCSS ecosystem.

[PostCSS]: https://github.com/postcss/postcss


## Cases

You can iterate through all test cases using the `cases.each` method:

```js
const cases = require('postcss-parser-tests')

cases.each((name, css, ideal) => {
  it('parses ' + name, () => {
    const root = parse(css, { from: name })
    const json = cases.jsonify(root)
    expect(json).toEquql(ideal)
  })
})
```

This returns the case name, CSS string, and PostCSS AST JSON.

If you create a non-PostCSS parser, just compare if the input CSS is equal to the output CSS after parsing.

You can also get the path to some specific test cases using the `cases.path(name)` method.


## Integration

Integration tests are packed into a Gulp task:

```js
const cases = require('postcss-parser-tests')

cases.real(css => {
  return parser(css).toResult({ map: { annotation: false } })
})
```

Your callback must parse CSS and stringify it back. The plugin will then compare the input
and output CSS.

You can add extra sites using an optional third argument:

```js
cases.real(css => {
  return parser(css).toResult({ map: { annotation: false } })
}, [
  'http://browserhacks.com/'
])
```
