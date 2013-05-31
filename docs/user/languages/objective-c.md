---
title: Building a Mac / iOS Project
layout: en
permalink: objective-c/
---

This guide will get you up and running with a Mac or iOS project on Travis CI.

## Prerequisites

- A Mac or iOS application written in Objective-C using Xcode. For RubyMotion
  projects, check out the [RubyMotion guide](/docs/user/rubymotion/).
- Unit and/or application tests for the project.

## Setting up your project

To get your project up and running on Travis CI, you need to set up a few things
on the project first. Travis CI uses a file called `.travis.yml` to store
settings and information on how to build your project, see the [build
configuration guide](/docs/user/build-configuration/) for more information about
that file. In order for us to recognize that your project is an Objective-C
project, add this to your `.travis.yml` file:

``` YAML
language: objective-c
```

This will make sure that your project is built on our OS X VMs and we set some
defaults for building your project.

## Dependency Management

Your project may have some dependencies that it needs to install before
building. If you're using Git submodules or CocoaPods for this, you don't have
to set anything up at all. If there's a `.gitmodules` file in your repository
we'll pull the Git submodules, and if there's a `Podfile` we'll run `pod
install`.

If you're using a different dependency management system, you can tell us how to
install dependencies using the `install` key in the `.travis.yml`. Note that
this will override CocoaPods if you have a `Podfile`, if you want both then you
need to either specify the `pod install` command or use the `before_install` key
instead.

``` YAML
install: make get-deps
```

Your project may also need some software to be installed on the machine it's
running on in order to work. For this purpose we have
[Homebrew](http://brew.sh/) installed. In order to install packages, add an
install script calling `brew install <names-of-packages>`. For instance, if you
need [ios-sim](https://github.com/phonegap/ios-sim) installed, add this to your
`.travis.yml` file.

``` YAML
install: brew install ios-sim
```

A short note on our configuration files: The files are based on the YAML file
format, and parse down to a dictionary-like structure (not unlike JSON if you're
familiar with that format). Just like NSDictionaries, each key has to be unique,
so if you want to specify multiple commands in the same stage, you have to make
it an array, like this:

``` YAML
install:
  - brew install ios-sim
  - make get-deps
```

If you have any questions about the configuration format, check out the [build
configuration guide](/docs/user/build-configuration/).


## Running the tests

Now for the most important part: actually building your project and running the
tests. If you already have a script to do this for you on the command line, then
this is very easy. Just like with install scripts, you specify it in the
`.travis.yml` file, but with the `script` key instead of the `install` key:

``` YAML
script: make test
```

If you don't have a script already, don't worry! You can use
[xctool](https://github.com/facebook/xctool), which is a replacement for the
`xcodebuild` tool. It adds some things, like a nicer output and returning a
different exit code for passing and failed builds (which is required for us to
correctly mark your build as passing or failing). To use xctool, add the
following to your `.travis.yml` file:

``` YAML
script: xctool -workspace MyApp.xcworkspace -scheme MyApp test
```

Replace `MyApp.xcworkspace` with the path to your workspace file, and `MyApp`
with the name of the scheme you want to build. If you're using Xcode projects
instead of workspaces, then you can replace `-workspace MyApp.xcworkspace` with
`-project MyApp.xcodeproj`. For more information on the options you can pass to
xctool, check out [their website](https://github.com/facebook/xctool).

## Examples

Here are some example projects running on Travis CI:

- [AFNetworking](https://github.com/afnetworking/afnetworking), an iOS and OS X
  networking framework ([.travis.yml](https://github.com/AFNetworking/AFNetworking/blob/master/.travis.yml)).
- [iOctocat](https://github.com/dennisreimann/ioctocat), a GitHub app for iOS
  ([.travis.yml](https://github.com/dennisreimann/ioctocat/blob/master/.travis.yml)).
