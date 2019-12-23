---
title: "Getting Started"
linkTitle: "Getting Started"
weight: 2
description: >
  First steps with Luet
---


## Prerequisites


### Building Luet

- Golang installed in your system


## Installation


### Building Luet from source


```bash
$> git clone https://github.com/mudler/luet
$> cd luet
$> make build # or just go build
```

### From release

Just grab a release from [the release page on GitHub](https://github.com/mudler/luet/releases).


## Setup

To build packages **with** Luet you need to have installed in your system:

- [Docker](https://www.docker.com/) (or [Img](https://github.com/genuinetools/img)) 
- [container-diff](https://github.com/GoogleContainerTools/container-diff#installation)

## Try it out!

```bash
$> ./luet --version
```


## Installation notes for Linux Distributions

### Sabayon

```bash
## Install golang
$> equo install dev-lang/go

## install git
$> equo install dev-vcs/git

## install docker
$> equo install app-emulation/docker

# Get container-diff (you can also install the binary from their release page on github and skip this section)
$> sudo equo i enman
$> sudo enman add devel
$> sudo equo up
$> sudo equo install app-emulation/container-diff-0.15.0

## enable docker
$> systemctl start docker

## clone the luet git repository
$> git clone https://github.com/mudler/luet.git

## compile luet
$> cd luet && make build

## Test it
$> ./luet --version
$> ./luet --help
--> gets you help

##
## Create your first repository
##

## Create a root directory for your repository, it will contain all the package definitions ?
$> mkdir ~/my_repo

## As an example we are going to build the game freedoom, wich is in Gentoo Portage tree
$> mkdir freedom

$> cd freedom

## Go into the directory and create 2 yaml files
## build.yaml tells how to build it, and definitions.yaml tells what package def is
$> touch definition.yaml && touch build.yaml

## Edit definition.yaml
category: "games"
name: "freedoom"
version: "0.12.0"

## Edit build.yaml
image: sabayon/builder-amd64
prelude:
- equo install i freedoom --onlydeps # Because we don’t have all the deps already as specs
steps:
- emerge freedoom

## Now instruct luet to build the package
Inside ~/my_repo
$> luet build games/freedoom-0.12.0
```
