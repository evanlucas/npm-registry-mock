[![Build Status](https://travis-ci.org/npm/npm-registry-mock.png?branch=master)](https://travis-ci.org/npm/npm-registry-mock)
[![Dependency Status](https://gemnasium.com/npm/npm-registry-mock.png)](https://gemnasium.com/npm/npm-registry-mock)

#npm-registry-mock

##Mocked Packages

Currently mocked packages are:

`underscore` at 1.3.1, 1.3.3 and 1.5.1 while version 1.5.1 is the latest in this mocked registry.

`request` at 0.9.0, 0.9.5 and 2.27.0 while version 2.27.0 is the latest in this mocked registry.

`test-package-with-one-dep` at 0.0.0, with mocked dependency `test-package@0.0.0`.

`npm-test-peer-deps` at 0.0.0, with a peer dependency on `request@0.9.x` and a dependency on `underscore@1.3.1`.

##Usage

Installing underscore 1.3.1:

```javascript
var mr = require("npm-registry-mock")

mr(1331, function (s) {
  npm.load({registry: "http://localhost:1331"}, function () {
    npm.commands.install("/tmp", "underscore@1.3.1", function (err) {
      // assert npm behaves right...
      s.close() // shutdown server
    })
  })
})
```

Defining custom mock routes:

```javascript
var mr = require("npm-registry-mock")

var customMocks = {
  "get": {
    "/mypackage": [500, {"ente" : true}]
  }
}

mr({port: 1331, mocks: customMocks}, function (s) {
  npm.load({registry: "http://localhost:1331"}, function () {
    npm.commands.install("/tmp", "mypackage", function (err) {
      // assert npm behaves right with an 500 error as response...
      s.close() // shutdown server
    })
  })
})
```

##Adding a new fixture

Although ideally we stick with the packages already mocked when writing new tests, in some cases it can be necessary to recreate a certain pathological or unusual scenario in the mock registry. In that case you can run

```sh
$ ./add-fixture.sh my-weird-package 1.2.3
```

to add that package to the fixtures directory.

##Todo

 - add more use-cases
