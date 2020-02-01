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

Packages can specify a list of `provides`. It is a list of packages in *package form* which indicates that the current definition *replaces* every occurrence of the packages in this list in the same context (*build* or *runtime*). This mechanism is particularly helpful for handling package moves or to have virtual packages. 