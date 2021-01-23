# svelte-snowpack-vest-bug

> :bug: Investigation repo for Snowpack + Vest bug

## What's the bug?

Importing specific Vest components does not work at the top import level.

```js
// in src/App.svelte

// this does not work
import { create, enforce, test } from 'vest';

// this works
import vest from 'vest';
const { create, enforce, test } = vest;
```

**TODO:** Try to understand where the problem is.

## To run this project

```text
$ npm i && npm start
```
