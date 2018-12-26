# yadeep
[![Build Status](https://travis-ci.org/terraindata/yadeep.svg?branch=master)](https://travis-ci.org/terraindata/yadeep)
[![version](https://img.shields.io/npm/v/yadeep.svg)](https://www.npmjs.org/package/yadeep)
[![dependencies](https://david-dm.org/terraindata/yadeep.svg)](https://david-dm.org/terraindata/yadeep)
[![devDependencies](https://david-dm.org/terraindata/yadeep/dev-status.svg)](https://david-dm.org/terraindata/yadeep#info=devDependencies)

_Yet another deep JSON getter/setter module_

## Why?

Several libraries already exist for getting and setting fields in deep JSON objects.  However,
none had the flexibility we needed for building an enterprise-quality ETL solution.

A key differentiator is that `yadeep` builds on top of [terrain-keypath](https://github.com/terraindata/terrain-keypath).
This means:

 * We can work with **any** field name in a JSON document.  Do you have field names like `'*'`, `'.'`, etc.?  yadeep has
   you covered.  Most libraries sacrifice this generality to use characters like those as waypoint delimiters,
   wildcards, etc.
 * We can get and set on **wildcard** keypaths.  Do you want to increment the values of all children of an object?  Or
   are you writing some aggregation function on deeply-nested sub-documents?  yadeep will help.
 * We can also handle some level of **nested wildcards**.  Keypath expressions with multiple wildcard tokens will try
   to behave in a semantically intuitive way (though expressions with more than two wildcard tokens are quite exotic
   in the real world and hence not well-tested).

yadeep also aims to provide the simplest syntax it can while still supporting this level of generality.  If you accept
our keypath conventions, you can immediately use `yadeep.get(object, keypath)` and `yadeep.set(object, keypath, value)`;
the aim is to make the API human readable and somewhat self-documenting.

Additional functions for searching in and traversing deep JSON objects can be found in the [code](https://github.com/terraindata/yadeep/blob/master/yadeep.ts).

TypeScript definitions included!

## Installation

    npm install yadeep

## Basic Usage

```js
const { KeyPath } = require('terrain-keypath');
const yadeep = require('yadeep');

const doc = {
  name: 'Bob',
  hardarr: [['a'], ['b', [{count: 1}, {count: 2}]]]
};

yadeep.get(doc, KeyPath(['hardarr', '1', '0'])); // 'b'
yadeep.get(doc, KeyPath(['hardarr', '1', '1', -1, 'count'])); // [1, 2]

yadeep.set(doc, KeyPath(['hardarr', '0', '0']), 'not a'); // changes 'a' -> 'not a'
yadeep.set(doc, KeyPath(['hardarr', '1', '1', -1, 'count']), 17); // changes both 'counts's to 17
```

See [yadeepTests.ts](https://github.com/terraindata/yadeep/blob/master/yadeepTests.ts) for a battery of additional tests and usages.

