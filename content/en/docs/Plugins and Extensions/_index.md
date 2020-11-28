
---
title: "Plugins and Extensions"
linkTitle: "Plugins and Extensions"
weight: 6
description: >
  How can be Luet expanded?
---

Luet can be extended in 2 ways, by extensions and plugins.

## Extentions
Extensions expand Luet featureset horizontally, so for example, “luet geniso” will allow you to build an iso using luet, without this needing to be part of the luet core.

## Plugins

Plugins instead are expanding Luet vertically by hooking into internal events. Plugins and Extensions can be written in any language, bash included! Luet uses [go-pluggable](https://github.com/mudler/go-pluggable) so it can dispatch events to external binaries.