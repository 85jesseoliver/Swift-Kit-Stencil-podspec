# StencilSwiftKit

[![CircleCI](https://circleci.com/gh/SwiftGen/StencilSwiftKit/tree/stable.svg?style=svg)](https://circleci.com/gh/SwiftGen/StencilSwiftKit/tree/stable)
[![CocoaPods Compatible](https://img.shields.io/cocoapods/v/StencilSwiftKit.svg)](https://img.shields.io/cocoapods/v/StencilSwiftKit.svg)
[![Platform](https://img.shields.io/cocoapods/p/StencilSwiftKit.svg?style=flat)](http://cocoadocs.org/docsets/StencilSwiftKit)
![Swift 5.0](https://img.shields.io/badge/Swift-5.0-orange.svg)

`StencilSwiftKit` is a framework bringing additional [Stencil](https://github.com/stencilproject/Stencil) nodes & filters dedicated to Swift code generation.

## Tags

* [Macro](Documentation/tag-macro.md) & [Call](Documentation/tag-call.md)
  * `{% macro <Name> <Params> %}…{% endmacro %}`
  * Defines a macro that will be replaced by the nodes inside of this block later when called
  * `{% call <Name> <Args> %}`
  * Calls a previously defined macro, passing it some arguments
* [Set](Documentation/tag-set.md)
  * `{% set <Name> %}…{% endset %}`
  * Renders the nodes inside this block immediately, and stores the result in the `<Name>`  variable of the current context.
* [Import](Documentation/tag-import.md)
  * `{% import "common.stencil" %}`
  * Imports any macro & set definitions from `common.stencil` into the current context.
* [Map](Documentation/tag-map.md)
  * `{% map <Variable> into <Name> using <ItemName> %}…{% endmap %}`
  * Apply a `map` operator to an array, and store the result into a new array variable `<Name>` in the current context.
  * Inside the map loop, a `maploop` special variable is available (akin to the `forloop` variable in `for` nodes). It exposes `maploop.counter`, `maploop.first`, `maploop.last` and `maploop.item`.

## Filters

* [String filters](Documentation/filters-strings.md):
  * `basename`: Get the filename from a path.
  * `camelToSnakeCase`: Transforms text from camelCase to snake_case. By default it converts to lower case, unless a single optional argument is set to "false", "no" or "0".
  * `contains`: Check if a string contains a specific substring.
  * `dirname`: Get the path to the parent folder from a path.
  * `escapeReservedKeywords`: Escape keywords reserved in the Swift language, by wrapping them inside backticks so that the can be used as regular escape keywords in Swift code.
  * `hasPrefix` / `hasSuffix`: Check if a string starts/ends with a specific substring.
  * `lowerFirstLetter`: Lowercases only the first letter of a string.
  * `lowerFirstWord`: Lowercases only the first word of a string.
  * `replace`: Replaces instances of a substring with a new string.
  * `snakeToCamelCase`: Transforms text from snake_case to camelCase. By default it keeps leading underscores, unless a single optional argument is set to "true", "yes" or "1".
  * `swiftIdentifier`: Transforms an arbitrary string into a valid Swift identifier (using only valid characters for a Swift identifier as defined in the Swift language reference). In "pretty" mode, it will also apply the snakeToCamelCase filter afterwards, and other manipulations if needed for a "prettier" but still valid identifier.
  * `upperFirstLetter`: Uppercases only the first character
* [Number filters](Documentation/filters-numbers.md):
  * `int255toFloat`
  * `hexToInt`
  * `percent`

## Stencil.Extension & swiftStencilEnvironment

This framework also contains [helper methods for `Stencil.Extension` and `Stencil.Environment`](https://github.com/SwiftGen/StencilSwiftKit/blob/stable/Sources/StencilSwiftKit/Environment.swift), to easily register all the tags and filters listed above on an existing `Stencil.Extension`, as well as to easily get a `Stencil.Environment` preconfigured with both those tags & filters `Extension`.

## Parameters

This framework contains an additional parser, meant to parse a list of parameters from the CLI. For example, using [Commander](https://github.com/kylef/Commander), if you receive a `[String]` from a `VariadicOption<String>`, you can use the parser to convert it into a structured dictionary. For example:

```swift
["foo=1", "bar=2", "baz.qux=hello", "baz.items=a", "baz.items=b", "something"]
```

will become

```swift
[
  "foo": "1",
  "bar": "2",
  "baz": [
    "qux": "hello",
    "items": [
      "a",
      "b"
    ]
  ],
  something: true
]
```

For easier use, you can use the `StencilContext.enrich(context:parameters:environment:)` function to add the following variables to a context:

- `param`: the parsed parameters using the parser mentioned above.
- `env`: a dictionary with all available environment variables (such as `PATH`).

---

# Licence

This code and tool is under the MIT Licence. See the `LICENCE` file in this repository.
