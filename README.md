SmartUnit is intended to be a place where unit test utility and helper classes can be collected to be shared.

SmartUnit artifacts, including sources and javadocs, are available on the [central Maven repository](http://search.maven.org/#search%7Cga%7C1%7Csmartunit).

Have a look at [our wiki](https://github.com/rlogiacco/SmartUnit/wiki) for more information.

[![TravisCI Build Status](https://travis-ci.org/rlogiacco/SmartUnit.svg?branch=master)](https://travis-ci.org/rlogiacco/SmartUnit)

<!-- toc -->

- [Release](#release)
  * [Release Keys](#release-keys)

<!-- tocstop -->

### Release

Before initiating a release it's strongly adviced to execute the full integration test suite which is normally not executed during the common *install* phase.
During the *release* phase the additional `ie`, `chrome` and `firefox` Maven profiles are enabled which execute browser specific tests: this means the releaser need to have a Windows box (due to Internet Explorer dependency) with the Selenium IE and Chrome drivers installed (Firefox does not need a driver).
The easiest way to provide the additional drivers path is to use system properties:
```
mvn integration-test -Pfirefox,chrome,ie -Dwebdriver.ie.driver=<path>\IEDriverServer.exe -Dwebdriver.chrome.driver=<path>\chromedriver.exe
```

Please note that the Selenium driver for Internet Explorer **requires** to set the protection mode to be the same on all zones, otherwise the browser will detach unexpectedly.

**Do not proceed to the release process unless the above command executes without errors**

To prepare and perform the release multiple _unix like_ commands needs to be on the path, the simplest way to have them all is to perform the release within the Git Bash shell.

The `gpg` command must be on the PATH in order to sign the artifacts so it's better to double check the environment configuration and your passphrase by running:
```
$> gpg --output test.sig --sign <anyfile>
```

Additionally `git`, `ssh` and `ssh-agent` executables must be on the PATH, the latter with your Github SSH key loaded:
```
$> eval$(ssh-agent)
$> ssh-add ~/.ssh/id_rsa
$> ssh -T git@github.com
```

#### Release Keys

Three keys are required for the release: a GPG one to sign the artifacts, an RSA one to commit release changes on Github and an RSA one to push the artifacts onto the Maven Central repository (hosted by Sonatype).

You can use the same RSA key for both Github and Sonatype to reduce the amount of passwords and configurations if you wish.
