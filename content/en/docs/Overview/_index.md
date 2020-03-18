---
title: "Overview"
linkTitle: "Overview"
weight: 1
description: >
  See Luet in action.
---

# What

Luet brings the containers ecosystem to standard package management software and delivery. It is fully built around the container concept, and leverages the huge catalog already present in the wild. It lets you use Docker images from docker hub, or from private registry to build packages, and helps you to redistribute them.

Luet clients ( here in after called "Luet native clients" ) can consume Luet repositories with 0-dependency installs. No dependency is required by the Package manager, giving you the full control on what you install or not in your system. While it can be used to generate *Linux from Scratch* distributions, it can be used also to build Docker images, or simply build standalone packages that you might want to redistribute.

The syntax which is proposed it aims to be KISS - you define a set of steps that has to be run while you build your image, and a set of constraints that denotes the requirements, or conflicts of your package.

# Why

There is no known package manager that has zero dependency and that fully leverages the container ecosystem. This gap forces current package managers to depend on a specific system layout and by the depencies that it has to care about. This is somehow prone to situations that could lead to a broken system. We want to fix that by giving you ways to define your system that can span from a layered system (just composed of "layers"), or a fully atomized one (a set of packages, that depends on each other and at the end compose up your system), without having a Package manager that depends on what you ask to install.

# How

By defining a yaml specification format, Luet parses the [requirements](/docs/concepts/constraints) to build [packages](/docs/concepts/packages), so Luet clients can consume them.
As Luet has 0 dependency while being used for installation of packages, this have the nice effect that doesn't depend on the package tree that is available for installation inside a Luet repository.

Here below you can find links to tutorials on how to build packages, images and repositories.