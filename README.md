# gpm-local: Set local package info for GPM

This package provides utlities for managing aspects of your local Go
packages with [GPM](https://github.com/pote/gpm).

## Use Cases

This plugin is designed to help you manage a large package (with
subpackages) that also has external dependencies.

The GPM and GVP tools provide a way to handle package-local `$GOPATH`.
Already, they provide excellent support for external packages.

This plugin makes it possible for `go` tools to find subpackages in the 
present package by fully qualified package name.

For example, assume you have a package named
`github.com/technosophos/foo`. Say you have this package checked out in
a local directory named `foo/`. With GPM/GVP, your `$GOPATH` will point
to `foo/.godeps`.

Now say you have a subpackage in `foo/` called
`github.com/technosophos/foo/bar`. In order to correctly import this
package, you will need the full package name to show up in your
`$GOPATH`. That's where `gpm-local` comes in:

```bash
$ go test
server.go:25:2: cannot find package "github.com/technosophos/foo/bar" in any of:
...
# Uh-oh! The subpackage cannot be found!
$ gpm local name github.com/technosophos/foo
$ go test
# works!
```

Above, we used `gpm local name` to tell `gpm` what the name of our local
package is. With this information, it can configure the `$GOPATH` so
that the standard Go tools will be able to find subpackages.

## Installation

Clone this repo and run:

```bash
$ make install
```

## Usage

```bash
$ gpm local name PACKAGE_NAME
```

In the above, `PACKAGE_NAME` is the name of the present package. This
will make it possible for the go tools to locate all of the subpackages
of the present package.

## How It Works

This works in a rather uninspiring way: It creates a symbolic link from
the local package to `.godeps/src/` so that the Go tools find
subpackages in `$GOPATH`.

## See Also...

* [gpm-git](https://github.com/technosophos/gpm-git)
