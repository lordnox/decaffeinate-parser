# decaffeinate-parser

This project uses the [official CoffeeScript
parser](https://github.com/jashkenas/coffeescript) to parse CoffeeScript source
code, then maps the AST generated by the parser to one more suitable for the
[decaffeinate project](https://github.com/eventualbuddha/decaffeinate) based on
the AST generated by
[CoffeeScriptRedux](https://github.com/michaelficarra/CoffeeScriptRedux).

This project might be useful to anyone who wants to work with a CoffeeScript
AST and prefers the AST generated by CoffeeScriptRedux, but wants to avoid the
source compatibility issues introduced by using that project. Note that it is
not 100% compatible with CoffeeScriptRedux:

* Single-line functions, `if` statements, etc. have blocks for bodies.
* Virtual nodes (such as the `LogicalNotOp` generated by an `unless`) are
  marked as such with a `virtual: true` property.
* `for-in` loops do not have an implicit `step` property.
* Ranges of programs with indented blocks are correct.
* `super` is supported in classes.
* `extends` is usable as a binary operator and in class declarations.
* `//` (floor division) is supported.

## Install

```
$ npm install --save-dev decaffeinate-parser
```

## Usage

This example gets the names of the parameters in the `add` function:

```js
import { parse } from 'decaffeinate-parser';
const program = parse('add = (a, b) -> a + b');
const assignment = program.body.statements[0];
const fn = assignment.expression;
console.log(fn.parameters.map(param => param.data)); // [ 'a', 'b' ]
```
