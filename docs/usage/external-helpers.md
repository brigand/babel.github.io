---
layout: docs
title: External helpers
description: How to use the external helpers.
permalink: /docs/usage/external-helpers/
---

## Details

babel has a few helper functions that'll be placed at the top of the generated
code if needed so it's not inlined multiple times throughout that file. This may
become an issue if you have multiple files, especially when you're sending them
to the browser. gzip alleviates most of this concern but it's still not ideal.

You can tell babel to not place any declarations at the top of your files and
instead just point them to a reference contained within the external helpers.

## Usage

```sh
$ babel --external-helpers
```

```js
babel.transform("code", { externalHelpers: true });
```

### Getting the external helpers

```sh
$ babel-external-helpers [options]
```

or

```js
require("babel").buildExternalHelpers();
```

or from an npm release in `external-helpers.js` from the babel directory.

#### Options

| Option                     | Default              | Description                                 |
| -------------------------- | -------------------- | ------------------------------------------- |
| `-t, --output-type [type]` | `global`             | Set output format: `global`, `umd` or `var` |
| `-l, --whitelist`          |                      | Whitelist of helpers to ONLY include        |

### Output formats

#### global

`global` output format sets helpers as global variable by adding `babelHeleprs` to `global` or `this`.

#### umd

`umd` output format wraps helpers in UMD compatible with browsers, CommonJS and AMD.

#### var

`var` outputs variable `babelHelpers` (`var babelHelpers = {}`) and helpers are assigned to it. This output format is suitable for additional processing.

### Injecting the external helpers

#### Node

```js
require("babel/external-helpers");
```

This injects the external helpers into `global`.

#### Browser

```html
<script type="application/javascript" src="your-path-to/babel/external-helpers.js"></script>
```

In a browser environment you can use a `<script>` tag to inject the `babelHelpers` into the `window` object.

### Selective builds

You can pass the option `returnUsedHelpers` to `babel.transform()` in order to
get a list of helpers that were used for that file:

```javascript
require("babel").transform("code", { returnUsedHelpers: true }).usedHelpers;
```

This will be an array of helpers that you can then pass to
`buildExternalHelpers` like so:

```javascript
require("babel").buildExternalHelpers(usedHelpers);
```
