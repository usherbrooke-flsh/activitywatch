<img title="ActivityWatch" src="https://activitywatch.net/img/banner.png" align="center">

<p align="center">
  <b>Records what you do</b> so that you can <i>know how you've spent your time</i>.
  <br>
  All in a secure way where <i>you control the data</i>.
</p>

<p align="center">

  <a href="https://github.com/usherbrooke-flsh/activitywatch">
    <img title="Star on GitHub" src="https://img.shields.io/github/stars/usherbrooke-flsh/activitywatch.svg?style=social&label=Star">
  </a>

  <br>

  <b>
    <a href="https://activitywatch.net/">Official Website</a>
    — <a href="https://forum.activitywatch.net/">Official Forum</a>
    — <a href="https://docs.activitywatch.net">Official Documentation</a>
    — <a href="https://github.com/usherbrooke-flsh/activitywatch/releases">Releases</a>
  </b>
</p>

<p align="center">
  <a href="https://github.com/usherbrooke-flsh/activitywatch/actions?query=branch%3Amaster">
    <img title="Build Status GitHub" src="https://github.com/ActivityWatch/usherbrooke-flsh/activitywatch/Build/badge.svg?branch=master" />
  </a>
  <a href="https://docs.activitywatch.net">
    <img title="Documentation" src="https://readthedocs.org/projects/activitywatch/badge/?version=latest" />
  </a>
  <a href="https://github.com/usherbrooke-flsh/activitywatch/releases">
    <img title="Latest release" src="https://img.shields.io/github/release-pre/usherbrooke-flsh/activitywatch.svg">
  </a>
  <a href="https://github.com/usherbrooke-flsh/activitywatch/releases">
    <img title="Total downloads (GitHub Releases)" src="https://img.shields.io/github/downloads/usherbrooke-flsh/activitywatch/total.svg" />
  </a>
</p>

<!--
# TODO: Best practices badge that we should work towards, see issue #42.
[![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/873/badge)](https://bestpractices.coreinfrastructure.org/projects/873)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bhttps%3A%2F%2Fgithub.com%2FActivityWatch%2Factivitywatch.svg?type=shield)](https://app.fossa.io/projects/git%2Bhttps%3A%2F%2Fgithub.com%2FActivityWatch%2Factivitywatch?ref=badge_shield)
-->

<details>
 <summary>Table of Contents</summary>

 * [About](#about)
    * [Installation &amp; Usage](#installation--usage)
      * [How to build from source](#how-to-build-from-sources)
    * [Is this yet another time tracker?](#is-this-yet-another-time-tracker)
      * [Feature comparison](#feature-comparison)
 * [About this repository](#about-this-repository)
    * [Server](#server)
    * [Watchers](#watchers)
    * [Libraries](#libraries)
 * [Contributing](#contributing)
</details>

## About

The goal of ActivityWatch is simple: *Enable the collection of as much valuable lifedata as possible without compromising user privacy.*

We've worked towards this goal by creating a application for safe storage of the data on the users local machine and as well as a set of watchers which record data such as:

 - Currently active application and the title of its window
 - Currently active browser tab and it's title and URL
 - Keyboard and mouse activity, to detect if you are AFK ("away from keyboard") or not

It is up to you as user to collect as much as you want, or as little as you want (and we hope some of you will help write watchers so we can collect more).

## Installation & Usage

Downloads are available on our [releases page](https://github.com/usherbrooke-flsh/activitywatch).

For instructions on how to get started, please see [our guide in the documentation](https://docs.activitywatch.net/en/latest/getting-started.html).

### How to build from sources

**Requirements and code modification**
- Python 3.9
- [Openssl unsafelegacyrenegociation](https://stackoverflow.com/questions/71603314/ssl-error-unsafe-legacy-renegotiation-disabled)
- [aw-server-rust/aw-webui/vue.config.js](aw-server-rust/aw-webui/vue.config.js)
- [aw-server/aw-webui/vue.config.js](aw-server/aw-webui/vue.config.js)
```javascript
const crypto = require("crypto");
const crypto_orig_createHash = crypto.createHash;
crypto.createHash = algorithm => crypto_orig_createHash(algorithm == "md4" ? "sha256" : algorithm);
```
[Source of this example](https://weekendprojects.dev/posts/fixed-node-err_ossl_evp_unsupported/)

or use `export NODE_OPTIONS=--openssl-legacy-provider` when calling `vue serve` and `vue build`

#### Build

```bash
git clone --recursive git@github.com:usherbrooke-flsh/activitywatch.git
cd activitywatch
python3.9 -m venv venv
source ./venv/bin/activate

cd ./aw-server-rust/
cargo update
cd ./aw-models/
cargo update
cd ../../

cd aw-server/aw-webui/
npx browserslist@latest --update-db

make build
```

#### Update sub repositories

```bash
git submodule update --init --recursive
```

## Is this yet another time tracker?

Yes, but we found that most time trackers lack in one or more important features.

**Common dealbreakers:**

 - Not open source
 - The user does not own the data (common with non-open source options)
 - Lack of synchronization (and when available: it's centralized and the sync server knows everything)
 - Difficult to setup/use (most open source options tend to target programmers)
 - Low data resolution (low level of detail, does not store raw data, long intervals between entries)
 - Hard or impossible to extend (collecting more data is not as simple as it could be)

**To sum it up:**

 - Closed source solutions suffer from privacy issues and limited features.
 - Open source solutions aren't developed with end-users in mind and are usually not written to be easily extended (they lack a proper API). They also lack synchronization.

We have a plan to address all of these and we're well on our way. See the table below for our progress.


### Feature comparison

##### Basics

|               | User owns data     | GUI                | Sync                       | Open Source        |
| ------------- |:------------------:|:------------------:|:--------------------------:|:------------------:|
| ActivityWatch | :white_check_mark: | :white_check_mark: | [WIP][sync], decentralized | :white_check_mark: |
| [Selfspy]       | :white_check_mark: | :x:                | :x:                        | :white_check_mark: |
| [ulogme]        | :white_check_mark: | :white_check_mark: | :x:                        | :white_check_mark: |
| [RescueTime]    | :x:                | :white_check_mark: | Centralized                | :x:                |
| [WakaTime]      | :x:                | :white_check_mark: | Centralized                | Clients            |

[sync]: https://github.com/ActivityWatch/activitywatch/issues/35
[Selfspy]: https://github.com/selfspy/selfspy
[ulogme]: https://github.com/karpathy/ulogme
[RescueTime]: https://www.rescuetime.com/
[WakaTime]: https://wakatime.com/

##### Platforms
<!-- TODO: Replace Platform names with icons  -->

|               | Windows            | macOS              | Linux              | Android            | iOS                 |
| ------------- |:------------------:|:------------------:|:------------------:|:------------------:|:-------------------:|
| ActivityWatch | :white_check_mark: | :white_check_mark: | :white_check_mark: | [WIP][android]     |:x:                  |
| Selfspy       | :white_check_mark: | :white_check_mark: | :white_check_mark: | :x:                |:x:                  |
| ulogme        | :x:                | :white_check_mark: | :white_check_mark: | :x:                |:x:                  |
| RescueTime    | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |Limited functionality|

[android]: https://github.com/ActivityWatch/activitywatch/issues/6

##### Tracking

|               | App & Window Title | AFK                | Browser Extensions | Editor Plugins     | Extensible            |
| ------------- |:------------------:|:------------------:|:------------------:|:------------------:|:---------------------:|
| ActivityWatch | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark:    |
| Selfspy       | :white_check_mark: | :white_check_mark: | :x:                | :x:                | :x:                   |
| ulogme        | :white_check_mark: | :white_check_mark: | :x:                | :x:                | :x:                   |
| RescueTime    | :white_check_mark: | :white_check_mark: | :white_check_mark: | :x:                | :x:                   |
| WakaTime      | :x:                | :white_check_mark: | :white_check_mark: | :white_check_mark: | Only for text editors |

For a complete list of the things ActivityWatch can track, [see the page on *watchers* in the documentation](https://docs.activitywatch.net/en/latest/watchers.html).


## About this repository

This repo is a bundle of the core components and official modules of ActivityWatch (managed with `git submodule`). It's primary use is as a meta-package providing all the components in one repo; enabling easier packaging and installation. It is also where releases of the full suite are published (see [releases](https://github.com/ActivityWatch/activitywatch/releases)).

### Server

`aw-server` is the official implementation of the core service which the other ActivityWatch services interact with. It provides a REST API to a datastore and query engine. It also serves the web interface developed in the `aw-webui` project (which provides the frontend part of the webapp).

The REST API includes:

 - Access to a datastore suitable for timeseries/timeperiod-data
 - A query engine and language for such data

The webapp includes:

 - Data visualization & browser
 - Query explorer
 - Export functionality 

### Watchers

ActivityWatch comes pre-installed with two watchers, `aw-watcher-afk` which logs the presence/absence of user activity from keyboard and mouse input and `aw-watcher-window` which logs the currently active application and it's window title.

There are lots of other watchers for ActivityWatch which can track more types of activity such as `aw-watcher-web` which tracks time spent on websites, multiple editor watchers which tracks spent time coding and many more! [A full list of watchers can be found in our documentation here](https://docs.activitywatch.net/en/latest/watchers.html).

### Libraries

 - `aw-core` - core library, provides no runnable modules
 - `aw-client` - client library, useful when writing watchers

## Contributing

Want to help? Great! Check out the [CONTRIBUTING.md file](./CONTRIBUTING.md)!

## Questions and support

Have a question, suggestion, problem, or just want to say hi? Post on [the forum](https://forum.activitywatch.net/)!

