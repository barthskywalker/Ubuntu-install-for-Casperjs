# Ubuntu-install-for-Casperjs
## CasperJS Background

The complete documentation for the current releases is **hosted on [docs.casperjs.org](http://docs.casperjs.org/).**

CasperJS is a navigation scripting & testing utility for [PhantomJS](http://www.phantomjs.org/) and [SlimerJS](http://slimerjs.org/) (still experimental). It eases the process of defining a full navigation scenario and provides useful high-level functions, methods & syntactic sugar for doing common tasks such as:

- defining & ordering [navigation steps](http://docs.casperjs.org/en/latest/quickstart.html)
- [filling forms](http://docs.casperjs.org/en/latest/modules/casper.html#fill)
- [clicking links](http://docs.casperjs.org/en/latest/modules/casper.html#click)
- [capturing screenshots](http://docs.casperjs.org/en/latest/modules/casper.html#captureselector) of a page (or an area)
- [making assertions on remote DOM](http://docs.casperjs.org/en/latest/modules/tester.html)
- [logging](http://docs.casperjs.org/en/latest/logging.html) & [events](http://docs.casperjs.org/en/latest/events-filters.html)
- [downloading](http://docs.casperjs.org/en/latest/modules/casper.html#download) resources, even binary ones
- catching errors and react accordingly
- writing [functional test suites](http://docs.casperjs.org/en/latest/testing.html), exporting results as JUnit XML (xUnit)

Browse the [sample examples repository](https://github.com/casperjs/casperjs/tree/master/samples). Don't hesitate to pull request for any cool example of yours as well!

**Read the [full documentation](http://docs.casperjs.org/) on casperjs documentation website.**

## CasperJS Example

The following code snippet is all the code nessecary to login to Facebook:

```javascript
//code to setup casper instance
var casper = require('casper').create({
  verbose: true,
  logLevel: "debug",
  waitTimeout: 7000,
  resourceTimeout: 10000,
  viewportSize: {
    width: 1024,
    height: 768
  },
  pageSettings: {
    javascriptEnabled: true,
    loadImages: true,
    loadPlugins: true
  }
});
//varibles required
var url = "https://www.facebook.com";
var utils = require('utils');
var x = require('casper').selectXPath;
//comamd line varibles
var user = casper.cli.args[0];
var pass = casper.cli.args[1];
// first step is to open the online resource
casper.start(url, function(response) {
  console.log(url + ' is opened');
});

// code to login to your facebook account
casper.then(function () {
  this.sendKeys(x('//*[@id="email"]'),user);
  this.sendKeys(x('//*[@id="pass"]'),pass);
  this.click(x('//*[@id="u_0_q"]'));
});

//run the script
casper.run(function() {
  this.then(function () {
    //take a screenshot of login
    this.capture('facebook_screenshot.png');
    this.exit();
  });
});
```

**Shell command to run CasperJS script**

```shell
casperjs --engine=slimerjs name_of_script.js user pass
```

**Shell command to run CasperJS script through browser on server**

```shell
xvfb -a casperjs --engine=slimerjs name_of_script.js user pass
```

## Install CasperJS on your system

If you are running Ubuntu :

- There is a Bash script in the python+bash directory [casper_installer.sh](https://git.heanet.ie/bokeefe/error_log/tree/master/python+bash)
- Download it and run it as superuser

### What the script does:

- Firstly it installs all the dependencies needed to run PhantomJS
- It then does a manual install of PhantomJS (the npm and repo versions I found are unstable).
- It then installs SlimerJS (you will be prompted as to whether or not this is a server install)
- Next it checks that Firefox is installed and if not installs it and sets the SlimerJs launcher.
- Finially it installs CasperJS
- It also additionally installs xvfb in order to make SlimerJS headless

### Possible additional steps depending on Firefox version:

- The current version of SlimerJS specifies Firefox 52.* as max version.
- However it is possible to change this by editing the appiclation.ini for SlimerJS

#### Command to edit appiclation.ini on local install

```shell
vi /usr/local/share/slimerjs-0.10.3/application.ini
```

#### Command to edit appiclation.ini on server install

```shell
vi ~/slimerjs-0.10.3/application.ini
```

#### The application.ini should look like this:

```shell
[App]
Vendor=Innophi
Name=SlimerJS
Version=0.10.3
BuildID=20170309
ID=slimerjs@slimerjs.org
Copyright=Copyright 2012-2017 Laurent Jouanneau & Innophi

[Gecko]
MinVersion=38.0.0
MaxVersion=53.*

[XRE]
EnableExtensionManager=1
```

#### Increase the `MaxVersion=53.*` if the version of Firefox installed is greater than that specified :

- you can check the version of Firefox by running this command

```shell
firefox -v
```

## At this point installation is complete !!!!!
