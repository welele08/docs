---
title: "Overview"
linkTitle: "Overview"
weight: 3
description: >
  See Luet in action.
---

# What

Luet brings the containers ecosystem to standard software package management and delivery. It is fully built around the container concept, and leverages the huge catalog already present in the wild. It lets you use Docker images from [Docker Hub](https://hub.docker.com/), or from private registries to build packages, and helps you to redistribute them.

Luet clients (here in after called "Luet native clients") can consume Luet repositories with 0-dependency installs. No dependency is required by the Package manager, giving you the full control on what you install or not in your system. While it can be used to generate *Linux from Scratch* distributions, it can also be used to build Docker images, or to simply build standalone packages that you might want to redistribute.

The syntax proposed aims to be [KISS](https://en.wikipedia.org/wiki/KISS_principle) - you define a set of steps that need to be run to build your image, and a set of constraints denoting the requirements or conflicts of your package.

# Why

There is no known package manager with 0-dependency that fully leverages the container ecosystem. This gap forces current package managers to depend on a specific system layout and the corresponding depencies. This can cause situations leading to a broken system. We want to fix that by giving you ways to define your system spanning from a layered system (just composed of "layers"), or a fully atomized one (a set of packages, that depends on each other and at the end compose up your system), without having a package manager that depends on what you ask to install.

# How

By defining a [YAML](https://en.wikipedia.org/wiki/YAML) specification format, Luet parses the [requirements](/docs/docs/concepts/constraints) to build [packages](/docs/docs/concepts/packages), so Luet clients can consume them. Since Luet has 0-dependency for installation of packages, it benefits from the nice effect of not depending on the package trees available for installation inside Luet repositories.

Below you can find links to tutorials on how to build packages, images and repositories.
