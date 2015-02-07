## Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **v0.1.2** (2015-02-07):
  * Fix: sourcing no longer auto-installed/-removed on package installation - removed due to permission headaches; users must now run typex -i manually after installation.

* **v0.1.1** (2015-02-07):
  * Fix: --version reported incorrect version number.
  * Temp. fix: installation of sourcing with -i doesn't report error in case of failure so as not to prevent installation of the npm package altogether.

* **v0.1.0** (2015-02-07):
  * Initial release.
