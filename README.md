# gofmtline

gofmtline is a fork of the Go programming language that modifies the `gofmt` tool to allow and preserve single-line `if` statements, making it easier to write and maintain concise error handling and guard clauses in Go code.

## What is different?

In standard Go, `gofmt` will always expand `if` statements to multiple lines, even if they were originally written on a single line. With gofmtline, if you write a single-line `if` statement, it will remain on a single line after formatting, as long as the body is small enough. Multi-line `if` statements and more complex cases are formatted as usual.

## Example

**Input (single-line if):**
```go
if err != nil { return err }
if x := foo(); x > 0 { bar() }
```

**Output with gofmtline:**
```go
if err != nil { return err }
if x := foo(); x > 0 { bar() }
```

**Input (multi-line if):**
```go
if err != nil {
    log.Println("error")
    return err
}
```

**Output with gofmtline:**
```go
if err != nil {
    log.Println("error")
    return err
}
```

**Input (if-else, single-line):**
```go
if err != nil { return err } else { return nil }
```

**Output with gofmtline:**
```go
if err != nil { return err } else { return nil }
```

**Input (if-else, multi-line):**
```go
if err != nil {
    return err
} else {
    return nil
}
```

**Output with gofmtline:**
```go
if err != nil {
    return err
} else {
    return nil
}
```

## Usage

This repository is a full Go toolchain fork. To use the new formatting behavior, build the toolchain and use the `gofmt` binary from this repository:

```sh
git clone https://github.com/alarbada/gofmtline.git
cd gofmtline/src
./make.bash
./../bin/gofmt -w yourfile.go
```

## VS Code Integration

To use gofmtline as your default Go formatter in VS Code, add the following to your `settings.json` (replace the path as needed):

```jsonc
"go.formatTool": "custom",
"go.alternateTools": {
  "customFormatter": "/path/to/gofmtline/bin/gofmt"
},
```

This will make VS Code use your custom gofmt binary for formatting Go files after you build the toolchain.

## Compatibility

- All other formatting rules and Go syntax are preserved.
- Only the formatting of single-line `if` statements is changed.

## Contributing

While (I hope) this is pretty much done, if for some reason you want to change the implementation, do this:

- First, make your changes. I did modify the `go/printer/nodes.go`, you would start from there.
- The `.input` and `.golden` files will ensure that your formatting works. The ones added are `ifstmt.input` and `ifstmt.golden`.
- To run tests and ensure that it works, run this:

```sh
cd src
./make.bash # or use the make file corresponding to your OS
cd ./cmd/gofmt && ../../../bin/go test -v -run TestRewrite
```

## License

This project is a fork of the Go programming language and is distributed under the same BSD-style license as Go.