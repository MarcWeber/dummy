![YPM](http://ypm.it-o-t-f.com/logo.svg "YPM Logo")

YPM (YOUR PACKAGE MANAGER)
==========================

goals:
* cross platform & cross language
* putting *YOU UNDER CONTROL*
* each package is "responsible" on its own thus it documents what in needs to
  be compiled & run.
* implement solver which gets the job done reasonably well.
* directly install from github / bitbucket if repositories ship recipes
* introduce global package database for search and attaching information
* be as simple as driving a car
* deterministic & reproducable & secure, thus chrooted builds if supported by OS
* for git/mercurial dependencies allow "latest/HEAD", but force devs to also specify a
  hash known to be working so that others can reproduce behavior.
* allow YOU to choose for which use cases you prefer bleeding edge over stable
  releases, whether to use which forks and similar
* allow impure substitutes - eg install Ruby gem mysql but use the system's
  mysql library - if its not there yet tell the user to install it before any
  build starts
* allow version lock files similar to what Ruby's bundler project provides:
  A file storing the exact dependencies so that they can be reproduced.

How sweet will it smell?
========================

This shell example shows how it'll look like installing a python project called A,
a Ruby project called B and so on:

    # Note how packages get simply prefixed by language file extension
    $ ypm i[nstall] py-A rb-B hs-C java-D js-E ml-F scala-G

      ypm: You haven't setup a configuration file yet.
      May I assume you want
        * to use latest versions unless there are known bugs or security issues
        * you trust the global package database
      [Y]/n
      .. thanks .. installing ..
      downloads: 200 MB of source
      downloads: 180 MB of prebuild binaries
      estimated execution time: 40 min
      ... build/download logs


Sample shell session installing from github like repositories:

    $ # assuming package has sane package description in its repository
    $ ypm install github:user/package


Starting to hack on an arbitrary project:

    $ # this will checkout sources, provide utilities, whatever you need to get
    $ # started hacking on a project:
    $ ypm hack-on "package-foo"


How to configure the solver?

    $ export YPM=SOLVER_CONFIG=file:PROFILE=~/ypm-profile:PACKAGE_POOL=A:PACKAGE_POOL=B

    where A,B can be the open source package database run by YPM project.

What does the solver do exactly? If a package says "I depend on package B",
it finds that package B, and if there are multiple versions it chooses the best
one. The meaning of "best" is your choice. It could be "bleeding edge" or
"stable" or you could be forcing 'recipe found at github/name/repo' depending
on what YOU prefer.

PACKAGE DATABASE
================
While you'll be able to get recipes directly from github repositories
it'll still make sense to have a database containing recipes of opensource
software so that you can search AND ATTACH INFORMATION.

PACKAGE RECIPES
===============
Package's constraints depend on context. Examples:
* java on java complier
* Ruby on platform (jruby, rvm, ruby) and operating system
* Ocaml: binary or bytecode
* ...

Thus recipes will abstract over those common choices and turn into "build
descriptions" once they get instantiated with proper data (such as compiler
version).
While doing this don't try to be perfect - this would be too CPU costly.
Instead try to maximize the value to the user. Thus be good enough to suport
flavour/targets such as:
* run test suite y/n
* build with / without documentation y/n
* ...

More details at the end of this document

PACKAGE DISAMBIGUATION
======================
Hot topic. All of you know that there are many forks on github of your beloved package FOO.
Thus introduce "meta information" which knowns about the 'official' fork. But
allow YOU to override this default choice.

Also allow "distributors" to tell which packages they support - so that you can
make a choice faster. Thus you'll be allowed to say "if in doubt use the
version Ubuntu uses if known".

LANGUAGES WHICH WILL BE SUPPORTED
=================================
* C
* C++
* lua
* Ruby
* PHP
* Haskell
* Perl
* Python
* FreePascal
* NodeJS
* Java
* Scala
* TCL
* Erlang
* Rust
* Ocaml
* D
* Fortran
* Elisp (Emacs extensions)
* Lisp
* VimL (Vim plugins)
(List is incomplete)

PLATFORMS WHICH WILL BE SUPPROTED
=================================
* linux
* OSX
* Darwin
* cygwin (thus Windows)
* FreeBSD/OpenBSD

HOW TO FOLLOW?
==============
See [ypm homepage](http://ypm.it-o-t-f.com) (More to to come)

ROADMAP (STEP TO SUCCESS)
=========================
0) Create a nice logo. Suggestions ?

1) For each language which could be supported (see above) create its own fund
raising page. Then start supporting the languages which get funded first.

2) describe the package recipes & instantiation, how dependencies could be
resolved and implement a prototype targeting [Homebrew- OSX](http://brew.sh/)
and [Nix packaging system](http://nixos.org) which runs on linux, darwin,
cygwin etc.

3) abstract over nix store location, so that binaries can be installed into
directories

4) Start YBF (your build farm) project

Who am I?
=========
I'm Marc Weber living in Germany (black forest).

I've been programming for 15 years using various languages. I've been always
annoyed by having to learn yet another packaging tool which was special in way
X just because I wanted to give a tool written in language Y a try.

I don't care about "dress codes" - I want to get jobs done. I only believe in
"beauty/theory" as long as it serves a purpose.

Everybody can join and get build recipes from - including big well
known distributions such as Ubuntu, Suse, Debian, Nixos, Gentoo (any other
listed or not listed on
[Wikipedia's list](http://en.wikipedia.org/wiki/Linux_distribution).


Why should you support this project/ which problems do get fixed compared to X?
===============================================================================

* accelerating open source software development. It'll be easy for users & testers
  to try new software and versions. Thus the feedback loop will become faster.
  Often users don't know how to program, but they still want to help by testing.

* managing the incredible amount of languages, packages, versions increasing
  every single day:
  The more packages exist the less sense does it make to download all
  package descriptions to your disk such as common linux distributions do it.
  Be lean, slick, simple: Only get on disk what you use, use the YPM database
  and google to find software you're interested in.

* developers will get started more quickly improving software because setting
  up the environment required to build a project will be trivial.
  Right now often you have to install software headers, pollute your system with
  unstable versions and keep track of those changes. YPM will provide a simple command
  ypm hack-on "package-foo" which will do everything you need to get started.

* By reducing package management to its minimal set of required features
  (package recipes, solvers, workers compiling binaries) distributors and
  developers will have least chance to get distracted.

CONTACT
=======
[mail](mailto:marco-oweber@gmx.de) or irc: irc.freenode.net, nick MarcWeber.

When I'm making process, what will I be working on as well?
===========================================================

* YBF (your build farm) Start a buildfarm for packages recipes. Because there are more packages
  then I'll have resources there will be "subscription options" for less
  commonly used software. This will ensure that resources are spend in an
  optimal way.

  Example: If you want fork of firefox you'll not pay for libjpeg, you'll only
  pay for building that fork - the more people join subscription - the less
  you'll pay. Yes, I do believe in the perfect market serving us all in the
  best way.

* Start a compiler which allows me to "code things once" - however this will
  take quite a while

* provide smart hosting which does not only handle the server code but also
  state in a simple way

FAQ
===
  Q: Will I be able to ensure everything found on github, sourceforge, bitbucket,
     any other location will install?
  A: No. In 2011 github alone was said to have more than 2.000.000 repositories.
    I will support the code and infrastructure so that maintainers can join.

  Q: Will I be able to ensure there are no bugs / security issues?
  A: No. But the database will allow to attach and query known issues.
     Putting you under control means also letting you choose what you consider to
     be a serious issue or a small problem you can live with.

  Q: Which language will be used to implement the first working prototype
  A: Haskell - unless you have strong reasons to use a different language

  Q: Why not use "gem" "pip" (any other existing packaging solution
  A: They are not cross language. Eg you still have to install mysql C library in
     order to build ruby's mysql library.

  Q: Will you serve end users or developers?
  A: Both. Of course I'll take care to allow generating "debuggable" binaries
    as well as generating tags, installing source along the binaries, allow to
    run test suites and much more.

  Q: How long will it take ?
  A: I guess you understand that there is a lot to consider. Thus it'll be very
    hard predict when all features will be implemented. I want 10 languages to be
    supported within 1 year.

  Q: Which languages will be supported first?
  A: Those who get most funding / whose community support is best.

  Q: If I spend money, will I get listed?
  A: Of course! I'll create a listing which will work like ads: The more you
    spend, the more likely your logo & homepage link will be shown. Thus if person
    A spend $2 and person B spends $8 B will be shown 4 times as often.
    This much fairer than introducing ordering.

  Q: license of the code?
  A: LGPLv2

  Q: What will happen if maintainers don't support YPM
  A: Then I or YPM contributors will write scripts turning release notes into
    recipes. Thus if there is intereset such projects will be supported as well.

How can I help?
===============
* Support the fundraiser. Its ok to spend not too much right now, but continue
  supporting the project later if you're satisfied by its progress.

* Tell me which packages/ use cases you want to be supported first serving as
  test case.

WATCH THIS PROJECT
==================
See mailinglist or to announcements at [ypm homepage](http://ypm.it-o-t-f.com)


principles I believe in
=======================
* KISS: keep it (as) stupid simple (as possible)
* code once & do it once (and right)
* use functional style if appropriate (no hidden state)
* Everybody should do what he can do best
* The perfect market being the best tool to serve everybody. Support markets by
  providing the information they need.
* reproducibility matters for servers & end users.
* Think twice when forcing new habits on users
* specialization matters. Nobody today can do everything.


DETAILS: PACKAGE RECIPES
========================

Package recipes will be written in its own simple language and look like
functions. Some arguments can be inferred by the solver, others will have to be
set by the user - such as build package foo with option bar.

A package recipe once instantiated can return different targets such as
binaries, documentation, tagged source and whatnot.

Example compiling PHP:

    # properties:
    name: 
    version: 
    source: {url = .. , hash = ..}
    license:

    # for search:
    provides-binaries: php

    # the function arguments, ! means must be passed
    # auto means can be inferred by solver
    {
      c_compiler,
      cross_compilation_options,

      auto supportZlib default true,
      auto supportSqlite default true,

      if supportZlib auto zlib
      if supportSqlite auto sqlite
    }

    # some code describing the build (eg shell scripts)

Getting this right alone will take 2 weeks or more.

DETAILS: PACKAGE DATABASE
=========================

Allow third parties to attach information:
* patches
* review status (could prevent OpenSSL heartbleat like bugs because companies know where to spend more resources on)
* issues
* alternatives
* list of pro/cons
* ....

Items such as "review status" or "issues" can be signed - thus its you choosing
whom to trust.
