Note: this project has been archived, as development has stalled, and it isn't being actively maintained, nor used.

SpiderNode: Node.js on SpiderMonkey
===
This project is a port of Node.js on top of SpiderMonkey, the JavaScript engine in Firefox. We're still in the very early stages of the port, and a lot of work remains to be done before Node works.

[![Build Status](https://travis-ci.org/mozilla/spidernode.svg?branch=master)](https://travis-ci.org/mozilla/spidernode)

### Goals
Right now we're focused on using this project in the [Positron](https://github.com/mozilla/positron) project.  This means that we will need to finish SpiderShim to the extent necessary for Node.js to work.  In the future, we may look into finishing implementing the features of the V8 API that Node.js does not use, in order to provide a V8 API shim layer out of the box in SpiderMonkey.  The SpiderShim code is being developed with that long term goal in mind.

### How it works
To enable building and running Node.js with SpiderMonkey, a V8 API shim (SpiderShim) is created on top of the SpiderMonkey API.  This is based on Microsoft's [node-chakracore](https://github.com/nodejs/node-chakracore), but it doesn't share much code with it besides the build system integration.

### Current status
This is a _work in progress_.  Node can now be successfully built on top of SpiderMonkey, and the very basics seem to work, but there are probably still a lot of issues to discover and fix.

We have implemented a fair portion of the V8 API.  More specifically [these tests](https://github.com/mozilla/spidernode/blob/master/deps/spidershim/test) are currently passing.  Many of those tests have been ported from the V8 API tests.

### How to build
Before building please make sure you have the prerequisites for building Node.js as documented [here](https://github.com/nodejs/node/blob/master/BUILDING.md).

Building on any OS other than Linux or OS X has not been tested.

Build Command:
```bash
$ ./configure [options]
...
$ make
...
$ ./node -e 'console.log("hello from " + process.jsEngine)'
hello from spidermonkey
```

Where `options` is zero or more of:
* `--engine`: The JavaScript engine to use.  The default engine is `spidermonkey`.
* `--debug`: Also build in debug mode.  The default build configuration is release.
* `--enable-gczeal`: Enable SpiderMonkey gc-zeal support.  This is useful for debugging GC rooting correctness issues.
* `--with-external-spidermonkey-release` and `--with-external-spidermonkey-debug`: Enable building against an out-of-tree SpiderMonkey. Expects a path to a built SpiderMonkey object directory (in release and debug modes, respectively)

If you build against an out-of-tree SpiderMonkey, you must include the SpiderMonkey library path in _LD_LIBRARY_PATH_ (_DYLD_LIBRARY_PATH_ on Mac) when running Node, i.e.:
```bash
$ LD_LIBRARY_PATH=path/to/obj-dir/dist/bin ./node -e 'console.log("hello from " + process.jsEngine)'
```

To run the API tests, do:

```bash
$ ./deps/spidershim/scripts/run-tests.py
```

### Repository structure
The repository is based on [node-chakracore](https://github.com/nodejs/node-chakracore).  The interesting bits can be found in the [deps/spidershim](https://github.com/mozilla/spidernode/tree/master/deps/spidershim) directory.
