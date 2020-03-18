---
title: "Package"
linkTitle: "Package definition"
weight: 4
description: >
  How to define packages and relations between packages
---

A Package in Luet is denoted by a triple (`name`, `category` and `version`), here called *package form*. 

```yaml
name: "awesome"
version: "0.1"
category: "foo"
```

While `category` and `version` can be omitted, the name is required. Note that in `build.yaml` and in `definition.yaml` files, list of packages are represented by the same triple:

```yaml
requires:
- name: "awesome"
  version: "0.1"
  category: "foo"
```

They are nested in a list, allowing to specify as many as wanted:

```yaml
requires:
- name: "awesome"
  version: "0.1"
  category: "foo"
- name: "awesome2"
  version: "9999"
```

## Package building process

When a package is required to be built, Luet resolves the dependency trees and it orders the spec files to satisfy the given contraints.

Each package has a build context which corresponds where the spec files are found (`definition.yaml` and `build.yaml`), this means that in the container which is running the build process are accessible the sources refered to the build. *Note: you can use this mechanism to provide helper scripts or even static binaries to seed your images from scratch*

```
❯ tree distro/raspbian/buster
distro/raspbian/buster
├── build.sh
├── build.yaml
├── definition.yaml
└── finalize.yaml
```
In the example above, build.sh is accessible in build time. 

It can be invoked easily in build time by specifying in `build.yaml`:
```yaml
steps:
- sh build.sh
```

## Package provides

Packages can specify a list of `provides`. It is a list of packages in *package form* which indicates that the current definition *replaces* every occurrence of the packages in this list (both in *build* and *runtime*). This mechanism is particularly helpful for handling package moves or to have virtual packages. 

*Note: packages that in the `provides` list don't have to necessarly exist or either have a valid build definition.*

## Package types

By a combination of keywords in `build.yaml`, you end up with categories of packages that can be built:

- Seed packages
- Packages deltas
- Package layers
- Package with includes

Check the [Specfile concept](/docs/concepts/specfile) page for a full overview of the available keywords in the Luet specfile format.

### Seed packages

They denote the parent package (or root) that can be used by other packages to depend on. Normally seed packages includes just an image (preferably tagged) used as a base for other packages to depend on.

It is useful to pin to specific image versions, and to write down in a tree where packages are coming for. There can be as many seed packages as you like in a tree.

A seed package `build.yaml` example is the following:

```yaml
image: "alpine:3.1"
```

Every other package that depends on it will inherit the layers from it.

If you want to extract in separate packages (splitting) the content of the seed package, you can just create as many package as you wish depending on that one, and extract its content, for example:

**alpine/build.yaml**
```yaml
image: "alpine:3.1"
```

**alpine/definition.yaml**
```yaml
name: "alpine"
version: "3.1"
category: "seed"
```

**sh/build.yaml**
```yaml
# List of build-time dependencies
requires:
- name: "alpine"
  version: "3.1"
  category: "seed"
unpack: "true" # Tells luet to use the image content by unpacking it
includes: 
- /bin/sh
```

**sh/definition.yaml**
```yaml
name: "sh"
category: "utils"
version: "1.0"
```

In this example, there are two packages being specified:

- One is the `seed` package, which is the base image which we will use to extract later on packages. It has no installable content, and it is just virtual used during build phase.
- `sh` is the package which contains `/bin/sh`, extracted from the seed image and packaged. This can be consumed by Luet clients to install sh in their system.

### Packages delta

Luet by defaults will try to calculate the delta of the package that is meant to be built. It means that it tracks **incrementally** the changes in the packages, to ease the build definition. Let's see an example.

Given the root package:
**alpine/build.yaml**
```yaml
image: "alpine:3.1"
```

**alpine/definition.yaml**
```yaml
name: "alpine"
version: "3.1"
category: "seed"
```

We can generate whatsoever file, and include it in our package by defining this simple package:

**foo/build.yaml**
```yaml
# List of build-time dependencies
requires:
- name: "alpine"
  version: "3.1"
  category: "seed"
steps:
- echo "Awesome" > /foo
```

**foo/definition.yaml**
```yaml
name: "foo"
category: "utils"
version: "1.0"
```

By analyzing the difference between the two packages, Luet will automatically track and package `/foo` as part of the `foo` package.

To allow operations that must not be accounted in to the final package, you can use the `prelude` keyword:

**foo/build.yaml**
```yaml
# List of build-time dependencies
requires:
- name: "alpine"
  version: "3.1"
  category: "seed"
prelude:
- echo "Not packaged" > /invisible
steps:
- echo "Awesome" > /foo
```

**foo/definition.yaml**
```yaml
name: "foo"
category: "utils"
version: "1.0"
```

The list of commands inside the `prelude` that would produce artifacts, are not accounted to the final package. In this example, only `/foo` would be packaged (which output is equivalent to the example above).

This can be used for e.g. to fetch sources that must not be part of the package.

You can anytime apply restrictions, and use the `includes` keyword to specifically pin to the files you wish in your package.

### Package layers

Luet can be used to track entire layers, and make them installable by Luet clients. 

Given the examples above:

**alpine/build.yaml**
```yaml
image: "alpine:3.1"
```

**alpine/definition.yaml**
```yaml
name: "alpine"
version: "3.1"
category: "seed"
```

An installable package derived by the seed, with the actual full content of the layer con be composed as so:


**foo/build.yaml**
```yaml
# List of build-time dependencies
requires:
- name: "alpine"
  version: "3.1"
  category: "seed"
unpack: true # It advertize Luet to consume the package as is
```

**foo/definition.yaml**
```yaml
name: "foo"
category: "utils"
version: "1.0"
```

This can be combined with other keywords to manipulate the resulting package (layer), for example:



**foo/build.yaml**
```yaml
# List of build-time dependencies
requires:
- name: "alpine"
  version: "3.1"
  category: "seed"
unpack: true # It advertize Luet to consume the package as is
steps:
- apk update
- apk add git
- apk add ..
```

**foo/definition.yaml**
```yaml
name: "foo"
category: "utils"
version: "1.0"
```


### Package includes

The `includes` keyword can be combined ina ddition to extract portions from the package image.


**git/build.yaml**
```yaml
# List of build-time dependencies
requires:
- name: "alpine"
  version: "3.1"
  category: "seed"
unpack: true # It advertize Luet to consume the package as is
steps:
- apk update
- apk add git
includes:
- /usr/bin/git
```

**foo/definition.yaml**
```yaml
name: "git"
category: "utils"
version: "1.0"
```

As a reminder, the `includes` keywords accepts regex in the Golang format. Any criteria expressed with the regex which matches the file name (absolute path) will be part of the final package.
