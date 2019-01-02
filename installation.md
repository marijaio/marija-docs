---
layout: article
title: Installation
---

Installing Marija locally gives you more flexibility. You will be able to define
your own datasources.

## Step 1: Installation

There are 3 ways to install Marija:
1. Using Docker. **Recommended. The other options use outdated versions of Marija.**
2. Using Homebrew (macOS).
3. Installation from source.

### Option 1: Using Docker

To install Marija we will use Docker. Docker is a platform to run applications
withing containers. If you have never used Docker before, check out the
[guide on docker.com](https://docs.docker.com/get-started/).

When you have Docker installed, run the following command:
```
$ docker pull marija/marija
```

In your current directory, create a new file `config.toml`. In this file
we configure our datasources that will be searchable by Marija. Paste the
following contents into `config.toml`:

```
[datasource]

[datasource.blockchain]
type="blockchain"
```

Run the following command to start Marija:
```
$ docker run -d -p 8080:8080 -v $(pwd)/config.toml:/config/config.toml --name marija marija/marija
```

### Option 2: Using Homebrew (macOS)

> **Warning:** the Homebrew Marija is currently (2019-01-02) outdated. It will install an old version.

First make sure you have Homebrew installed. For more information see
[the Homebrew website](https://brew.sh/).

Run the following commands to install Marija:
```
$ brew tap dutchcoders/homebrew-marija
$ brew install marija
```

Find out the path to Marija with this command:
```
$ brew list marija
```

Paste the output of the previous command into your terminal to start Marija:
```
$ /usr/local/Cellar/marija/201612010907/bin/marija
```

**Todo: how do you configure the datasources with the Homebrew installation?**


### Option 3: Installation from source

> **Warning:** the Marija Golang package is currently (2019-01-02) outdated. It will install an old version.

Marija is written in Golang, so to build from source you will need a working Golang
environment.

If you do not have a working Golang environment setup first follow the
[Golang Installation Guide](https://golang.org/doc/install).

Run the following commands:
```
$ go get github.com/dutchcoders/marija
$ marija
```

**Todo: how do you configure the datasources with the Golang package?**

## Step 2: Checking if it works

Now Marija should be up and running on [localhost:8080](http://localhost:8080).
Visit it in your browser and try searching on a blockchain hash like
`7dc6951f1f8232d0e1e470430a82373c46bacf3622599ebd2a25faf979e204b1`.

## Next steps

We have installed Marija on our computer. Now it's time to configure our own
datasources, so we can search them with Marija.

[Learn how to add datasources.](/adding-datasources.html)