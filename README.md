[![npm version](https://img.shields.io/npm/v/typex.svg)](https://npmjs.com/package/typex) [![license](https://img.shields.io/npm/l/typex.svg)](https://github.com/mklement0/typex/blob/master/LICENSE.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

- [typex &mdash; Introduction](#typex-&mdash-introduction)
  - [Examples](#examples)
  - [Installation](#installation)
    - [From the npm registry](#from-the-npm-registry)
    - [Manual installation](#manual-installation)
  - [Usage](#usage)
  - [License](#license)
    - [Acknowledgements](#acknowledgements)
    - [npm dependencies](#npm-dependencies)
  - [Changelog](#changelog)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# typex &mdash; Introduction

`typex` is an extended version of the `type` Unix shell builtin that incorporates features from the `which` and `file` utilities:  
By default, it provides information about the current shell.
Given a name, it indicates how the current shell would interpret that name as a command, and provides information about each command form found.
Given a filesystem path, it provides information such as file type and the canonical path.

`typex` extends `type` as follows:

* _All_ defined forms of a given command name are (always) reported, which includes shell keywords, aliases, shell functions, builtins, and executables.
* Defined forms are listed in descending order of precedence - the first form listed is the effective one.
* If an executable is a symlink, the entire chain of symlinks up to and including the ultimate target is printed, using _absolute_ paths.
* The type of executable is printed, including the specific shell/interpreter used to run executable scripts.
* Version information, if available, is printed for executables.
* Optionally, you can get information about any filesystem object, including its canoncial path and symlink chain, if applicable.

Note: `typex` is designed for interactive use, to provide humans with concise, salient information about a command or filesystem object.
As such, its output is geared toward readability by humans, and should not be relied on for programmatic parsing.

See the [Usage](#usage) chapter for details.

For supported platforms and shells, see the [Installation](#installation) chapter.

## Examples

```shell
# Print information about the current shell.
$ typex
BINARY:     /bin/bash  [GNU bash, version 3.2.57(1)-release (x86_64-apple-darwin14)]

# Print information about command forms named 'printf' in order of precedence.
$ typex printf
BUILTIN:    printf
BINARY:     /usr/bin/printf  [printf (GNU coreutils) 8.21]

# Print information about binary 'nawk'; note the chain of symlinks,
# and its use of *absolute* paths, as well as the version information.
$ typex nawk
BINARY:     /usr/bin/nawk@ -> /etc/alternatives/nawk@ -> /usr/bin/gawk  [GNU Awk 4.0.1]

# Print information about executable script 'npm';
# note the interpreter information ('js script').
$ typex npm
SCRIPT:     /usr/bin/npm@ -> /usr/lib/node_modules/npm/bin/npm-cli.js  (js script)  [1.4.28]

# Print information about an alias:
$ alias lsx='ls -FAhl'
$ typex lsx
ALIAS:      lsx='ls -FAhl'
  BINARY:     /bin/ls  [ls (GNU coreutils) 8.21]

# Print the canonical path of a directory:
$ typex /etc/apache2/
DIRECTORY:  /etc/apache2 -> /private/etc/apache2

# Print extended information about special file /dev/null
$ typex -v /dev/null
FILE:       /dev/null  (character special )
	crw-rw-rw- 1 root root 1, 3 Feb  1 19:43

```

## Installation

**Supported platforms**

* When installing from the **npm registry**: **Linux** and **OS X**
* When installing **manually**: any **Unix-like** platform with **Bash** that uses either **BSD or GNU utilities**. 

**Supported shells**

For this utility to detect the current shell's aliases, keywords, (non-exported) shell functions, and builtins, it must be *sourced*, which is supported in the following shells:

* **bash**, **ksh**, and **zsh**

`typex` can still be useful in unsupported shells when invoked standalone (in which case it runs in `bash`), albeit in limited form:

* only executable *files* and filesystem objects will be detected - no other command forms.
* a warning to that effect is printed; you can suppress it with `-p`

### From the npm registry

With [Node.js](http://nodejs.org/) or [io.js](https://iojs.org/) installed, install [the package](https://www.npmjs.com/package/typex) as follows:

    [sudo] npm install typex -g

**Note**:

* Whether you need `sudo` depends on how you installed Node.js / io.js and whether you've [changed permissions later](https://docs.npmjs.com/getting-started/fixing-npm-permissions); if you get an `EACCES` error, try again with `sudo`.
* The `-g` ensures [_global_ installation](https://docs.npmjs.com/getting-started/installing-npm-packages-globally) and is needed to put `typex` in your system's `$PATH`.

**IMPORTANT**: After installation, run `typex -i` to add sourcing of `typex` to the per-user initialization file of all supported shells that are present (e.g., `~/.bashrc`).
To remove these sourcing commands later, run `typex -u`.

### Manual installation

* Download [this `bash` script](https://raw.githubusercontent.com/mklement0/typex/stable/bin/typex) as `typex`.
* Make it executable with `chmod +x typex`.
* Move it or symlink it to a folder in your `$PATH`, such as `/usr/local/bin` (OS X) or `/usr/bin` (Linux).

**IMPORTANT**: After installation, run `typex -i` to add sourcing of `typex` to the per-user initialization file of all supported shells that are present (e.g., `~/.bashrc`).
To remove these sourcing commands later, run `typex -u`.

## Usage

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```nohighlight
$ typex --help

SYNOPSIS
  typex [-p] [-V] [-v] [name ...]
  typex -i|-u

DESCRIPTION
  An extended version of the `type` utility that incorporates elements
  of the `which` and `file` utilities.
  
  Determines if NAME refers to one or more of the following command forms: 
    alias, shell keyword, shell function, builtin, or
    executable file in the PATH
  Provides more information than `type -a`, notably executable
  type, symlink chains, and version information.

  Alternatively, NAME may be a *filesystem path* rather than a mere (file)name,
  in which case information about that path is reported.

  Not specying any NAME prints information about the current shell.

  NOTE: For this utility to detect the current shell's aliases, keywords,
        (non-exported) shell functions, and builtins, it must be 
        *sourced*, which is supported in the following shells:
           bash, ksh, zsh
        
        Sourcing this utility without arguments in your shell's per-user
        initialization file will define a function of the same name that
        will be available throughout the session.
         - typex -i installs this sourcing for all supported shells,
         - typex -u uninstalls it.

        Ad-hoc and in scripts, you can source and invoke at the same time;
        e.g.:
          . typex read

        If you instead invoke this utility directly:
         - only executable *files* will be detected - no other command forms;
         - a warning to that effect is printed; suppress it with -p.

  -p
    Only looks for (executable) *files* (ignores aliases, keywords, functions,
    and builtins). This option applies implicity if NAME is a filesystem path
    rather than a mere command name.

  -V
    Suppresses the attempt to obtain version information from executable
    files. Helpful if you know that the target file doesn't support --version,
    or you're wary of what invocation with --version might do.

  -v
    Verbose mode: prints additional information - see below.

  Note that, unless you specified a filesystem path, ALL defined forms of a 
  given command name are reported, in order of precedence (highest first).
    bash, zsh: alias > shell keyword > shell function > builtin > executable
    ksh:       shell keyword > alias > shell function > builtin > executable

  All output starts with a headword identifying the command form / path type,
  as in the description below.
  Output for each command form is single-line, except for the optional
  information added by -v; the latter is indented with 2 spaces.
  The exit code is 0 if all NAMEs were found, the number of non-existent
  NAMES otherwise.

  ALIAS:
    Prints the alias's definition. If the first token of the alias' 
    definition is itself a command, information about that command is
    printed as well, in indented form, potentially recursively.
    No attempt is made to detect additional commands inside the alias
    definition.
    -v is never applied to the recursive calls.

  KEYWORD:
    A shell-language keyword, such as `while`; the name is printed.

  FUNCTION:
    Prints the function name, followed by '()'; e.g., 'foo()'.
    In bash, if the function is also exported, ' # -x' is appended.

    With -v specified:
      The function definition (its source code) is printed, too.

  BUILTIN:
    Prints the builtin's name.

  BINARY / SCRIPT / DIRECTORY / FILE:
    Prints files' full path, and, in the case of a symlink, the *entire
    symlink chain to the ultimate target file*, using full paths.

    If the file is not itself a symlink, but has symlink components
    (directories) in its *path*, the input path is printed as well
    as the canonical path with all symlinks resolved.

    If the file is an executable script, interpreter information is
    appended in parentheses; e.g., '(bash script)'.
    If the file is non-executable, file-type information obtained with
    `file` is appended in parentheses; e.g., '(ASCII text)'.
    If the file is a special file such as /dev/null, its type is also
    appended in parentheses; e.g., '(character special)'.
    
    Unless -V was specified, an attempt is made to obtain version information
    through invocation of the executable file with --version.
    If -V was specified, or if no version information was found, the
    last-modified timestamp of the (ultimate target) file is used.
    Whatever information is available is appended in square brackets; e.g.,
    '[ls (GNU coreutils) 8.21]' or '[2014-09-09]'

    With -v specified:
      File statistics (permissions, owner, ...) are printed, using `ls -l`.
      For symlinks, this happens for every file in the symlink chain.
      Additional information printed for executables:
       - binaries: file-type information obtained with `file`.
       - scripts: the shebang line.
  
COMPATIBILITY
  Platforms supported in principle:
    OSX, Linux, BSD (with bash installed)
  Among those, sourcing the script (recommmended) works in the following
  shells:
    bash, ksh, zsh

EXAMPLES
    # Print info about the current shell.
  $ typex 
  BINARY:     /bin/bash  [GNU bash, version 3.2.57(1)-release (x86_64-apple-darwin14)]
    # Print info about utility 'awk' and its symlink chain:
  $ typex awk  
  BINARY:     /usr/bin/awk@ -> /etc/alternatives/awk -> /usr/bin/gawk [GNU Awk 4.0.1]
    # Print information about command forms named 'printf' in order of 
    # precedence:
  $ typex printf
  BUILTIN:    printf
  BINARY:     /usr/bin/printf  [printf (GNU coreutils) 8.21]
    # Print information about script 'npm':
  $ typex -p npm
  SCRIPT:     /usr/local/bin/npm  (node script)  [2.1.7]
    # Print information about executables 'nawk' and 'mawk':
  $ typex -p nawk mawk
  BINARY:     /usr/bin/nawk@ -> /etc/alternatives/nawk@ -> /usr/bin/gawk  [GNU Awk 4.0.1]
  BINARY:     /usr/bin/mawk  [2014-03-24]
    # Define an alias and print information about it.
  $ alias lsx='ls -FAhl'; typex lsx
  ALIAS:      lsx='ls -FAhl'
    BINARY:     /bin/ls  [ls (GNU coreutils) 8.21]
```

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## License

Copyright (c) 2015 Michael Klement <mklement0@gmail.com> (http://same2u.net), released under the [MIT license](https://spdx.org/licenses/MIT#licenseText).

### Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have optional suffixes denoting the type of dependency; the _absence_ of a suffix denotes a required _run-time_ dependency: `(D)` denotes a _development-time-only_ dependency, `(O)` an _optional_ dependency, and `(P)` a _peer_ dependency.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

### npm dependencies

* [doctoc (D)](https://github.com/thlorenz/doctoc)
* [json (D)](https://github.com/trentm/json)
* [replace (D)](https://github.com/harthur/replace)
* [semver (D)](https://github.com/npm/node-semver#readme)
* [shall (D)](https://github.com/mklement0/shall)
* [urchin (D)](https://github.com/tlevine/urchin)

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **[v0.3.3](https://github.com/mklement0/typex/compare/v0.3.2...v0.3.3)** (2015-09-15):
  * [dev] Makefile improvements; various other behind-the-scenes tweaks.

* **[v0.3.2](https://github.com/mklement0/typex/compare/v0.3.1...v0.3.2)** (2015-09-03):
  * [fix] Broken (dangling) symlinks are now properly detected and reported.

* **[v0.3.1](https://github.com/mklement0/typex/compare/v0.3.0...v0.3.1)** (2015-06-26):
  * [dev] Makefile updated; to-do file added; typo in source-code comment fixed.
  * [doc] Minor read-me revision.

* **[v0.3.0](https://github.com/mklement0/typex/compare/v0.2.1...v0.3.0)** (2015-06-10):
  * [behavior change, enhancement] When reporting on an alias, information about the underlying command is now output with indentation, so as to make it clear that the information is subordinate to the alias information.
  * [fix] Reporting on an alias' underlying command no longer causes additional command forms of the same name to be ignored.
  * [fix] Obtaining a file's last-modified timestamp no longer breaks with filenames with embedded spaces.
  * [doc] Read-me improvements; version badge switched to shields.io; license badge added.

* **v0.2.1** (2015-05-30):
  * [doc] [npm registry badge](https://badge.fury.io) added

* **v0.2.0** (2015-05-24):
  * [enhancement] Now by default prints information about the _current shell_ (if no names are given).
  * [enhancement] Specifying `.` now also prints the current directory's canonical path.
  * [enhancement] Given a filesystem path, now also displays its canonical path with all symlinks resolved, if different.
  * [enhancement] A warning is now issued if a bareword can't be found as a command, but a file or dir. of the same name exists in the current dir.
  * [enhancement] For aliases, their definitions are now printed recursively, if applicable, and, if the ultimate definition's first token is a command, its information is printed as well.
  * [fix] `typex perl` now correctly reports Perl's version.
  * [fix] Paths with embedded space are now handled correctly.

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
