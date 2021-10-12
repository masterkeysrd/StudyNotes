# golang #

Golang is a open source project that allows programmers be product. Is an expressive, 
concise, clean, and efficient programming language. Have concurrency mechanisms that make
easy write progams that take advantage for multicore and network machines. Go is statically,
compiled language that feels like a dinamically typed, interpreted language.

## Installing GO ##

To install go please visit this [link](https://golang.org/doc/install).

## Dependency Tracking ##

When you are working in a go code that imports packages contained in other modules, this dependencies
are manage in your own code's module. That module is defined by `go.mod` file that tracks the dependencies
the modules provide those packages. That `go.mod` file stays with your code, included in your code repo.

To enable  the dependency tracking and create the `go.mod` file, run the command `go mod init <module name>`,
the module name will be the name of the module that your code will be in. The name is the module's path.
In most cases will be the repository localtion where your code will be in.

The next command enable the dependency tracking and creates `go.mod` file.

```bash
$ go mod init example.com/hello
go: creating new go.mod: module example.com/hello
```

## Hello, Go ##

After enable the dependency tracker in a directory you can create a file in this case `hello.go`
to write down simple hello world with go.

**`hello.go`**
```go
package main // declare `hello.go` file as a main package.

import "fmt" // import the `fmt` package.

// implement the main function
func main() {
	fmt.Println("Hello Wold!!") // use fmt package to print "Hello World!!" to the console.
}
```

### Running `hello.go` ###

To run the `hello.go` file just run the `go run` command. The example below show us how to use it.

```bash
$ go run .
Hello World!!
```

## Using external packages ##

As normal activity in programming we need to use things that are done by other people, a place
to look for 3rd party packages for go is [pkg.go.dev](https://pkg.go.dev).

For example a common package use as example in go is [rsc.io/quote](https://pkg.go.dev/rsc.io/quote) 
in the [pkg.go.dev](https://pkg.go.dev) the packages info about the functions, variables, constants,
and general information about the use of the package.

```go
package main

import "fmt"
import "rsc.io/quote" // importing the external module

func main() {
	fmt.Println(quote.Go()) // using the module
}
```

Now we need to add the new module requirements and the sums (a `go.sum` file) for use authenticating
the module. Run the following command to add the requirements and sums.

```bash
$ go mod tidy
go: finding module for package rsc.io/quote
go: downloading rsc.io/quote v1.5.2
go: found rsc.io/quote in rsc.io/quote v1.5.2
go: downloading rsc.io/sampler v1.3.0
go: downloading golang.org/x/text v0.0.0-20170915032832-14c0d48ead0c
```

Then you are able to run the code using the `go run` command.

```bash
$ go run .
Don't communicate by sharing memory, share memory by communicating.
```

## Modules ##

A module is a collection of packages that constains a set of discrete and useful functions.

> The go code is grouped into packages, and packages are grouped into a modules

