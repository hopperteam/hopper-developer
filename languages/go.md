# Go specific instructions
## Installation
You can install the library with `go get`:
```
go get github.com/hopperteam/hopper-api/golang
```

## Differences
All functions in `hopperApi` are capitalized in go in order to be exported. Also, optional data is always provided via pointer types. There are functions in `hopperApi` to return pointers for `string`s, `int64`s and `bool`s to allow for a shorter notation.

Update functions receive a struct with all not-to-be-set parameters set as `nil`. See examples in the [documentation](/documentation.md).

## Exceptions
The library returns errors from all methods that can go wrong, as usual in go. See examples in the [documentation](/documentation.md).
