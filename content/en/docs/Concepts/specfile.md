---
title: "Specfile"
linkTitle: "Specfile"
weight: 4
description: >
  Luet specfile syntax
---

## Specfiles

Luet has its own specfiles. They define the runtime and builtime requirements of a package.
There is an hard distinction between runtime and buildtime. A spec is composed at least by the runtime (`definition.yaml`) and the buildtime specification (`build.yaml`).

A Folder containing a `build.yaml` and a `definition.yaml` form a specfile. A Luet tree is merely a composition of specfile(s), but you can also have trees formed by only one.  There is no constriction on folder naming or hierarchy either.

You can look at them as an extension to Dockerfile(s), but it is reducing and expanding its syntax at the same time.

A build specification for our example (`awesome`) package looks like this:

*awesome/build.yaml*:
```yaml
steps:
- echo "Luet is awesome" > /awesome
requires:
- name: "echo"
  version: ">=1.0"
```

*awesome/definition.yaml*:
```yaml
name: "awesome"
version: "0.1"
```

As you can see in a glance, you can notice it depens on a package, `echo`! 

Let's see the build definition of `echo`:

*echo/build.yaml*:
```yaml
image: alpine
unpack: true
includes:
- /bin/echo
```

Where the definition is
*echo/definition.yaml*:
```yaml
name: "echo"
version: "1.3"
```

`echo` is packaged from the `alpine` image, which our `awesome` package depends on. Easy, no?

Now we can build `awesome`:

```bash
$> luet build --tree /tree/path /awesome-0.1
```

### Build specs

Build specs are defined in `build.yaml` files. They denote the build-time `dependencies` and `conflicts` along with a definition of the content of the package.

#### Steps

It is a list of commands to run into the container to install the package, and are actually translated as ```RUN``` keywords for ```Dockerfiles```. 
They represent the *steps* to install a package in the system. 

#### Prelude


