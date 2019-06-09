## v1.3.0
* Add explicit empty `go.sum` file, no dependencies, yay
* No per-element reflection for diffing byte arrays 
* Byte arrays/slices are formatted in hex now, 1 diff element instead of per-byte.
* Added support for ignoring the difference of an empty slice and nil slice.
* Fork, go-module now

## v1.2.0
* Added support for ignoring fields.

## v1.1.0

* Added support for recursive data structures.
* Fixed bug with embedded fixed length arrays in structs.
* Added `example/` directory.
* Minor test bug fixes for future go versions.
* Added change log.

## v1.0.0

Initial tagged release release.
