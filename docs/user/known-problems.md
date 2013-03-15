---
title: Known Problems
layout: en
permalink: known-problems/
---


### Production Status

Check it on [status.travis-ci.org](http://status.travis-ci.org)

### Long-Term Troubles affecting Travis CI Production

The list below presents well known problems, which are quite difficult to resolve within current Travis CI infrastructure (now based on [OpenVZ containerization](http://openvz.org/) and Ubuntu 12.04 64-bit).

* [Google Android SDK needs a 32bit-capable worker](https://github.com/travis-ci/travis-worker/issues/56).
  * Workaround very well described in this [blog post](http://rkistner.github.com/android/2013/02/05/android-builds-on-travis-ci/)
  * Planned to be solved by providing a genuine 32-bit linux worker (maybe dedicated for Android builds).
* [Google Chrome Browser does not work in an OpenVZ container](https://github.com/travis-ci/travis-ci/issues/938).
  * No fix planned yet.
  * It is thus recommended to execute other browsers for your tests (e.g. Firefox or Selenium WebDriver), or integrate with [SauceLabs](https://saucelabs.com/).

### Problem Report Guidelines

Feel free to report any problem, or submit pull request with bug fix, but please don't forget to first [read this guide](https://github.com/travis-ci/travis-web/blob/master/CONTRIBUTING.md) :)

TBD: the contributing guide should be enhanced, validated and merged in travis-ci/travis-ci repository.
