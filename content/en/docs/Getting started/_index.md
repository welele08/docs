---
title: "Getting Started"
linkTitle: "Getting Started"
weight: 2
description: >
  First steps with Luet
---


## Prerequisites

No dependencies. Only to build packages [Docker](https://www.docker.com/) (or [Img](https://github.com/genuinetools/img)) is required.

Luet currently supports [Docker](https://www.docker.com/) and [Img](https://github.com/genuinetools/img) as backends to build packages. Both of them can be used and switched in runtime.

### Docker

Docker is the (less) experimental Luet engine supported. Be sure to have Docker installed and the daemon running. The user running `luet` commands needs the corresponding permissions to run the `docker` executable, and to connect to a `docker` daemon. The only feature needed by the daemon is the ability to build images, so it fully supports remote daemon as well (this can be specified with the `DOCKER_HOST` environment variable, that is respected by `luet`)

### Img

Luet supports [Img](https://github.com/genuinetools/img). To use it, simply install it in your system, and while running `luet build`, you can switch the backend by providing it as a parameter: `luet build --backend img`. For small packages it is particularly powerful, as it doesn't require any docker daemon running in the host.


## Get Luet  

### From release

Just grab a release from [the release page on GitHub](https://github.com/mudler/luet/releases). The binaries are statically compiled.

```bash
wget https://github.com/mudler/luet/releases/download/0.8.3/luet-0.8.3-linux-amd64 -o luet
```

### Building Luet from source

Requirements:

- [Golang](https://golang.org/) installed in your system.
- make


```bash
$> git clone https://github.com/mudler/luet
$> cd luet
$> make build # or just go build
```

## Install it as a system package

In the following section we will see how to install luet with luet itself. We will use a transient luet version that we are going to throw away right after we install it in the system.


```bash
# Get a luet release. It will be used to install luet in your system
wget https://github.com/mudler/luet/releases/download/0.8.3/luet-0.8.3-linux-amd64 -O luet
chmod +x luet

# Creates the luet configuration file and add the luet-index repository.
# The luet-index repository contains a collection of repositories which are 
# installable and tracked in your system as standard packages.
cat > .luet.yaml <<EOF
repositories:
- name: "mocaccino-repository-index"
  description: "MocaccinoOS Repository index"
  type: "http"
  enable: true
  cached: true
  priority: 1
  urls:
  - "https://mocaccino.keybase.pub/repository-index"
EOF

# Install the official luet repository to get always the latest luet version
./luet install repository/luet

# Install luet (with luet) in your system
./luet install system/luet

# Remove the temporary luet used for bootstrapping
rm -rf luet

# Copy over the config file to your system
mkdir -p /etc/luet
mv .luet.yaml /etc/luet/luet.yaml
```


## Installation notes for Linux Distributions

### Sabayon

Install golang to build from source:
```bash
## Install golang
$> equo install dev-lang/go
```

Then install git and Docker:
```bash
## install git
$> equo install dev-vcs/git

## install docker
$> equo install app-emulation/docker

## enable docker
$> systemctl start docker
```

Build Luet from source:
```bash

## clone the luet git repository
$> git clone https://github.com/mudler/luet.git

## compile luet
$> cd luet && make build

## Test it
$> ./luet --version
$> ./luet --help

```