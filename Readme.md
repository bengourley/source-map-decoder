# @bengourley/source-map-decoder

Parse a source map and output the data in a way that makes sense to humans

This started life as [a gist](https://gist.github.com/bengourley/c3c62e41c9b579ecc1d51e9d9eb8b9d2) for the purpose of this [blog post](https://blog.bugsnag.com/source-maps/).

It reads a source map, and outputs an object with a similar structure, replacing the [VLQ](https://blogs.msdn.microsoft.com/davidni/2016/03/14/source-maps-under-the-hood-vlq-base64-and-yoda/) `mappings` property with a human-readable format:

```
"mappings": {
  "0": [
   ^
   └── the line number of the output file

    "231 => source.js 5:64 foo"
      ^        ^       ^    ^
      │        │       │    └── the symbol name from the source file
      │        │       │
      │        │       └── the line:column position in the source file
      │        │
      │        └── the name of the source file
      │
      └── the column number of the output file

  ]
}
```

## Installation

```
npm i -g @bengourley/source-map-decoder
```

## Usage

```
cat my-source-map.js.map | decode-source-map
```

This will output some not very pretty JSON so I recommend combining it with the [`jq`](https://stedolan.github.io/jq/) tool (available on a mac with `brew install jq`) to pretty-print and colourise the output:

```
cat my-source-map.js.map | decode-source-map | jq .
```

The above command will pretty print the entire source map structure, potentially including all sources. So to just view the mappings property, use jq's selector interface:

```
cat my-source-map.js.map | decode-source-map | jq .mappings
```

## License

ISC
