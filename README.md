# schemaify

Browserify transform for compiling JSON schemas at build time using AJV and `ajv-pack`

## Installation

The package is released to npm as `@khulnasoft/schemaify`:

```
npm install @khulnasoft/schemaify -D
```

## Usage

In the default configuration, the transform is applied to all files with a `.schema` extension. The transformed module will export the packed AJV `validate` function.

In your application:

```js
var validateFoo = require('./foo.schema')

var ok = validateFoo({ foo: true })
if (!ok) {
  console.log(validateFoo.errors)
  throw new Error('Foo did not validate')
}
```

When bundling:

```js
var browserify = require('browserify')

var b = browserify()
b.add('app.js')
b.transform('@khulnasoft/schemaify')
b.bundle(function (err, src) {
  // consume bundle
})
```

### Defining schemas

Schemas are expected to be defined in JSON format and saved as `.schema` files:

```json
{
  "type": "string",
  "maxLength": 128
}
```

## Options

The transform accepts the following options as its 2nd arguments:

### `secure`

By default, `schemaify` only compiles ["secure" schemas][secure]. This can be disabled by passing `secure: false` to the transform.

[secure]: https://github.com/ajv-validator/ajv/tree/521c3a53f15f5502fb4a734194932535d311267c#security-considerations

### `matcher`

By default, `schemaify` only compiles files with a `.schema` extension. If you have different requirements you can pass a Regexp string to `matcher` for the transform to use.

**Important caveat**: Due to the way that Browserify handles JSON files, you currently __cannot use `.json` files__ for storing your schemas, as this would make these files subject to another set of rules that would conflict with.

## Releasing a new version

New versions can be released using `npm version <patch|minor|major>`. Make sure you are authenticated against the `@khulnasoft` scope with npm.

## License

Copyright 2020 Md Sulaiman - Available under the MIT License
