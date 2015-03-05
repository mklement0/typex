## Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **v0.1.7** (2015-03-04):
  * Fix: Variables `$u` and `$i` that happen to be defined in the current shell no longer interfere with the sourced function.

* **v0.1.6** (2015-02-11):
  * Temp. fix: `typex -i` now properly reports failure on platforms that don't have the actual `ed` line editor, such as Debian.

* **v0.1.5** (2015-02-11):
  * Fix: --version no longer mistakenly exits the current shell when typex is invoked as a sourced function.
  * Dev: bash-presence test improved.
  * Dev: Makefile improvements.

* **v0.1.4** (2015-02-11):
  * Fix: Filenames that start with '-' are now handled correctly.
  * Dev: bash-presence test added.
  * Dev: Makefile improvements.

* **v0.1.3** (2015-02-09):
  * Doc: read-me and package description improved.

* **v0.1.2** (2015-02-07):
  * Fix: sourcing no longer auto-installed/-removed on package installation - removed due to permission headaches; users must now run typex -i manually after installation.

* **v0.1.1** (2015-02-07):
  * Fix: --version reported incorrect version number.
  * Temp. fix: installation of sourcing with -i doesn't report error in case of failure so as not to prevent installation of the npm package altogether.

* **v0.1.0** (2015-02-07):
  * Initial release.
