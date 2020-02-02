---
title: "Build packages"
linkTitle: "Build packages"
date: 2019-12-14
description: >
  How to build packages with luet
---

Building a package with Luet requires a definition only. It can be self-contained and be only composed of one spec, or a group of them, forming a Luet tree.

The `build` help command is the following:

```
build packages or trees from luet tree definitions. Packages are in [category]/[name]-[version] form

Usage:
  luet build <package name> <package name> <package name> ... [flags]

Flags:
      --all                  Build all packages in the tree
      --backend string       backend used (docker,img) (default "docker")
      --clean                Build all packages without considering the packages present in the build directory (default true)
      --compression string   Compression alg: none, gzip (default "none")
      --database string      database used for solving (memory,boltdb) (default "memory")
      --destination string   Destination folder (default currentpath)
  -h, --help                 help for build
      --privileged           Privileged (Keep permissions)
      --revdeps              Build with revdeps
      --tree string          Source luet tree

Global Flags:
      --concurrency int   Concurrency (default 8)
      --config string     config file (default is $HOME/.luet.yaml)
      --fatal             Enables Warnings to exit
  -v, --verbose           verbose output
```

Build accepts a list of packages to build, which syntax is in the `category/name-version` format (following Gentoo syntax). 

## Flags

Flags available for the build command

- **--all**: Boolean which instruct Luet to build all the specs contained in a tree
- **--backend**: String that specifies the backend to use. Currently supported are : `docker` and `img`. Defaults to `docker`
- **--clean**: Boolean that when enabled perform a clean build without taking in consideration already existing artifacts
- **--compression**: Specify a compression algorithm. Currently available only tgz. Defaults to no compression
- **--database**: Type of database which is used for solving. This setting is particularly helpful for big trees composed of many packages on small devices where available memory is limited. Currently available `boltdb` and `memory`. Defaults to `memory`.
- **--destination**: Output folder where the generated artifacts are written.
- **--privileged**: Boolean which indicates that the build is privileged and file permission bits in the packages are copeid as-is. 
- **--revdeps**: Boolean which indicates to build also all the packages which depends on the targets
- **--tree**: Path to the tree specfiles, it defaults to the current directory.

General flags:

- **--concurrency**: Indicates the number of concurrent package builds
- **--config**: Indicates a config file to read
- **--fatal**: Treats all warnings as fatal
- **--verbose**: Enables verbose output

## Examples

Create a simple package definition and build it:

```
$> mkdir package

$> cat <<EOF > package/build.yaml
image: busybox
steps:
- echo "foo" > /foo
EOF

$> cat <<EOF > package/definition.yaml
name: "foo"
version: "0.1"
category: "bar" # optional!
EOF

$> luet build --all

📦  Selecting  foo 0.1
📦  Compiling foo version 0.1 .... ☕
🐋  Downloading image luet/cache-foo-bar-0.1-builder
🐋  Downloading image luet/cache-foo-bar-0.1
📦   foo Generating 🐋  definition for builder image from busybox
🐋  Building image luet/cache-foo-bar-0.1-builder
🐋  Building image luet/cache-foo-bar-0.1-builder done
 Sending build context to Docker daemon  4.096kB
 ...

```

In order to consume the repository by a client, it is sufficient to create the repository:

```
$> luet create-repo
 For repository luet creating revision 1 and last update 1580640083...
```

Which generates a `repository.yaml` file containing the repository metadata needed by the client when consuming the packages.