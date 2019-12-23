---
title: "Build requirements"
linkTitle: "Build requirements"
date: 2019-12-14
description: >
  What Luet needs to build your packages
---

Luet currently supports [Docker](https://www.docker.com/) and [Img](https://github.com/genuinetools/img). Both backend can be used, and they can be switched in runtime.

## Docker

Docker is the (less) experimental Luet engine supported. Be sure in your system to have Docker installed and the daemon running.
The user which is running `luet` commands needs the permission to execute the `docker` executable, and to connect to a `docker` daemon. 
The only feature needed by the daemon is the ability to build images, so it fully supports remote daemon as well. (They can be specified with the `DOCKER_HOST` environment variable, that is respected by `luet`)

## Img

Luet supports [Img](https://github.com/genuinetools/img). To use it, simply install it in your system, and while running `luet build`, you can switch the backend by providing it as a parameter: `luet build --backend img`. For small packages it is particularly powerful, as doesn't require any docker daemon running in the host.

## Container-diff

Luet requires [Container-diff](https://github.com/GoogleContainerTools/container-diff) installed in your system. It is used to construct your package if it is composed by image diffs. 

Check out the [installation](https://github.com/GoogleContainerTools/container-diff#installation) notes of container-diff, depending on your platform.