# messagediff

A library for doing diffs of arbitrary Golang structs. Fork of [d4l3k/messagediff](https://github.com/d4l3k/messagediff).
Go-moduled, more options.

If the unsafe package is available messagediff will diff unexported fields in
addition to exported fields. This is primarily used for testing purposes as it
allows for providing informative error messages.

Options:

- fields in structs can be tagged as `testdiff:"ignore"` to make messagediff skip it when doing the comparison (see example).
- fields can also be ignored by passing an option: `PrettyDiff(a, b, IgnoreStructField("X"), ...)`
- ignore the differences in empty slices and nil slices. `PrettyDiff(a, b, SliceWeakEmptyOption{}, ...)`


## Example Usage

```go
package main

import "github.com/protolambda/messagediff"

type someStruct struct {
    A, b int
    C []int
}

func main() {
    a := someStruct{1, 2, []int{1}}
    b := someStruct{1, 3, []int{1, 2}}
    diff, equal := messagediff.PrettyDiff(a, b)
    /*
        diff =
        `added: .C[1] = 2
        modified: .b, from = 2; to = 3`

        equal = false
    */
}

```

### Test usage example

```go
import "github.com/protolambda/messagediff"

...

type someStruct struct {
    A, b int
    C []int
}

func TestSomething(t *testing.T) {
    want := someStruct{1, 2, []int{1}}
    got := someStruct{1, 3, []int{1, 2}}
    if diff, equal := messagediff.PrettyDiff(want, got); !equal {
        t.Errorf("Unexpected difference in something: %s", got, diff)
    }
}
```

### Ignore struct field with tag

To ignore a field in a struct, just annotate it with testdiff:"ignore" like
this:
```go
package main

import "github.com/protolambda/messagediff"

type someStruct struct {
    A int
    B int `testdiff:"ignore"`
}

func main() {
    a := someStruct{1, 2}
    b := someStruct{1, 3}
    diff, equal := messagediff.PrettyDiff(a, b)
    /*
        equal = true
        diff = ""
    */
}
```

See the `DeepDiff` function for using the diff results programmatically.

## License

Original: Copyright (c) 2015 [Tristan Rice](https://fn.lc) <rice@fn.lc>,
 GitHub: [d4l3k/messagediff](https://github.com/d4l3k/messagediff)

messagediff is licensed under the MIT license. See the LICENSE file for more information.

bypass.go and bypasssafe.go are borrowed from
[go-spew](https://github.com/davecgh/go-spew) and have a seperate copyright
notice.
