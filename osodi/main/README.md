OSoDi (open source distribution)
================================
project by code-once team

I dream about *one simple place to search for open source software* (as user) and
libraries (as developer) and *getting it on disk easily* while knowing that
builds are reproducible. As project manager I want team members to be able to
reproduce their working environment as fast as possible.
This should be easier than driving your car.

*I want you to help me turning this dream into reality*.

Open Source software is scattered over many places such as sourceforge, github,
bitbucket, and many more. They all server special purposes and do their job
pretty well.
But I want to see *more than just code on github*:
* How I can support its development or its developers if I like the project
* does it build and on what platforms
* strengths / weaknesses ?
* how many users ?
* supported by which distributions ?
* are there alternatives ?
* can I cross compile it?
* which languages its written in?
* how long it'll take me to have it on disk (estimated download/build time)
* how to get notified about important updates
* are there reviewers?
* bugs/ security issues?
* which compilers is it known to work with?
* how many lines of code?
* the license(s)

While many distributions really try hard to serve users, its getting harder and
harder the more projects exist. Moreover they cannot get it right usually, because
users have different focus: Be stable or bleeding edge. 

Sometimes you want to be both: Bleeding edge for software editing, stable for
your emails application.
You already get it: The more ways to combine software, the more challenging
the task to put this all into one software distributions.

How to escape and succeed?
==========================
Stop thinking in terms of "distributors", starting thinking in way software
development happens: Developers know what they did, and they are interested in
getting feedback from users especially when none of the distributions are shipping
the latest features yet.

The *faster such a feedback loop, the happier will users and developers be*.

However developers have no time/interest in supporting the overwhelming amount of
distributions - from [http://brew.sh/](Homebrew- OSX) up to [](Cygwin - Linux for Windows).
To get an idea about how many Linux distributions exist see
[http://en.wikipedia.org/wiki/Linux_distribution](Wikipedia's list).

It just happens that people often keep running what they are used to.
Thus to serve developers and users you have to serve distributions,
thus *provide the information they need to build a software package from
source*. And this is what this project is about:

1) The backbone: A pool of known packages recipes
=================================================
A package recipe is a recipe which can be instantiated to a package
descriptions. Such package descriptions has build/run/test constraints to be
satisfied.

Package recipes will contain at least:
 * input params required to instantiate them (see below)
 * submission date (This is important to reproduce a setup from "last year")
 * package name
 * a package versions along with information how to to find out which version is newer/older
 * package description
 * where to get source (eg github)
 * license alternatives - maybe even per file
 * operating systems it can be used on
 * compilers it can be used with
 * special tags to track forks introducing special features.
 * cross compilation options
 * flags compilation/install options (eg with-feature-XY)
 * dependencies which depend on the flags and goal (development, testing, running, ...)
 * is it binary/source distributable in regions (patents?)
 * tags (for categorisation)
 * is it ok to install multiple versions (Examples: Gimp: yes; dbus systemwide: no)
 * a list of files which are known to differ in builds eg because they contain the compilation timestamp
 * a list of binaries (so that you can search for them)
 ...

Additionally for each "package" name (which may have many released versions and
github repositories/forks attached) provide a summary such as
  * "recommended" version
  * differences (forks)
  * .. whatever makes sense also

Like Haskell allow the creation of "platforms". A platform is a set of commonly used
core packages which can be referenced. This could be "rails-core-3.0" which itself
references many small packages. Rails is popular web framework for Ruby.

Ocaml for instance allows compiling the same sources using either ocamlopt or
ocamlc. Ruby packages can either be run by ruby, rubinius or jruby.
Thus package recipes must be like functions once fed with input they turn into
static package descriptions.

2) The solver
=============
The solver will have the task to find matching dependencies to build targets.

Because there are so many ways to combine packages there will be different
strategies satisfying your needs such as
  * strategy 1: be bleeding edge
  * strategy 2: versions recommended by maintainers

In all cases fallback to older/stable releases unless there are known
bugs/vulnerabilities.

SAT solvers will be an option, but I fear they'll be too slow. SAT solvers
have tho property that if there is a solution they'll find it. But due to the
endless options its likely to annoy the user due to taking a lot of RAM and
burning CPU. While Eclipse has it for this project evaluation speed will be
important more important.

3) binaries - THE APP STORE
===========================
Turning package recipes into package descriptions allows to build/install
packages easily. The follow backends will be supported:
* [http://brew.sh/](Homebrew)
* [http://nixos.org](Nix package manager)
* unpack & build in your "home" dir for developing/ auto tagging and similar
* Any other famous linux distro if they want support such as Debian, Ubuntu, ..
  It will be trivial to automate package generation.

Some of strong features of Nix:
* parallel building of dependencies distributed on multiple build machines. See Hydra
* atomic upgrades
* allow multiple versions of the same package installed
* multiple revisions
* cygwin/linux/darwin support
* its made up of modules separating build recipes from the build.
* one click installs
* garbage collection of packages no longer in use

Note that Nix is strong enough to create its own distribution, but can be used
as package management by users, too. 

Why not just use Nix? The frontend generating the recipes has been implemented
3 times by now: Nix (the language), NiJS (js implementation), Guix (gnu
operating system using nix but replacing the Nix language by Guile).
Thus even though Nix gets its job done perfectly it does make sense to
think about how to collaborate.

Of course building packages does require resources (cpu, disk, network) thus
tracking resources required to build a package is important. Tracking the
resources will be very important for companies who want to setup their own
servers. Telling the system which packages should be build first or which
should be deleted garbage collected will be configurable.

4) integrating with other services
==================================
* Allow attaching information to packages such as vulnerabilities, package
  versions, distro specific patches and more.
  Such "attachments" may be signed and associated with accounts so that it'll e
  you deciding to trust or not to trust

* github / bitbucket and similar => continuous integration

Which programming languages will be supported?
==============================================
OSoDi will be cross language/ cross platform.
This will allow you to install a Ruby/... library along with MySQL headers
without having to use "sudo" on your own.
Some implementations details will depend on the backend. I want to provide an
alternative installer for the following languages:

* RubyForge,gem,bundler (Ruby)
* cpan (Perl)
* pip and easyinstall (Python)
* ivy & maven (Java)
* simple build tool (Scala)
* pear (PHP)
* vim-pi, Vundle, VAM, NeoVundle, ... (VimL)
* Cabal (Haskell)
* Opam (Ocaml)
* el-get & elpa (Emacs)
* luarocks & luadist (lua)
* haxelib (Haxe)
* reltool (erlang)
* rust
* ? (C/C++)
* ? fortran
* ... (I'm sure I've been missing some important languages and tools)
* FreePascal (fpc)

For example I'm not aware of any C/C++ package manager which can resolve dependencies on
its own.

I know that I'll be unable to support many 100.000 packages on my own.
I will need support of package maintainers, no doubt.

How will languages be supported? Some packaging systems such as Haskell's Cabal
already have strong dependency information. It will be easy to import .cabal
files. For languages which lack such detailed information additional databases
will be created adding the missing information. Eg VAM has been doing this for
Vim plugins, same about el-get (package manager for Emacs).

Example: How will it look like?
===============================

      osodi -c ~/.config/solver-hints install package-1 package-2

and solver-hints contains many rules disambiguating solutions, eg
telling it to prefer ocamlopt, which java version to use as default and the
like

My background
=============
I've been programming for about 15 years (Pascal, Delphi, some .net, JS, PHP,
Ruby, Python, Some C/C++, Haskell, Nix, VimL, Ocaml, Java, Scala, Haxe, ..) and
very often managing dependencies took more time than I wished it should.

I've been contributing to nixos.org since 2007.

Steps to success (roadmap)
==========================
0) Think about which license exactly to use. Any of these can be used:
    [https://www.bountysource.com/faq#what-types-of-projects-are-allowed-on-bountysource](open source licenses)
1) implement specifications for package instantiation & website
2) implement solver
3) make sure the languages can use the solver
3) integrate with Nix
4) integrate with Homebrew (unless Nix works reasonably well on OSX)
5) integrate bountysource, github & similar websites
6) Implement basic support for languages you voted for most

Implementing "basic support" means:

* Import existing package descriptions into the database.
  If package descriptions are strong (Hackage/Haskell) case this will be
  possible

* Otherwise provide samples how to add new configuration files which will
  make it possible to build/install packages using OSoDi

Then
* For each language implement alternative packaging solution and libraries so
  that the packaging solution can be used from within that language.
  This will be a lot of work thus I'll create new fundraisers for each language
  to understand which language I should implement first.

* Implement binary & source package creation/distribution for OSX, linux, cygwin based
  on Nix or alternatives (such as Homebrew)

* A lot more could be implemented on top of this data base such as "code search
  engines" and so on.

* Advertise the solution on common open source websites such as sourceforge so
  that members of the open source community will join.

How can you help?
=================
by donating money (bountysource)

By starting your own "build farm" reproducing builds. The end goal should be
having an open source stack which yields the same output if you feed it with
the same input.

Tell me about projects (arbitrary language) you want to be supported first
which will serve as test cases.

You want to help in another way? See contact me

principles I believe in
=======================
* KISS
* do it once and do it right
* use functional style if appropriate (no hidden state)
* Everybody should do what he can do best
* The perfect market being the best tool to serve everybody. Support markets by
  providing the information they needs.
* reproducibility matters for servers and end users.
* Think twice when forcing new habits on users
* specialization matters - thus I want to provide tools which allow open source
  programmers to keep working on the projects they know

CREDITS
=======
Of course most ideas are not new. Thus credits to everybody who is writing open
source or is working on/for any of the projects I mentioned (and those I forgot)

contact me
==========
[mailto:marco-oweber@gmx.de](mail) or irc: irc.freenode.net, nick MarcWeber.

link with:
http://stackoverflow.com/questions/7266097/are-there-any-efforts-to-create-a-package-manager-for-c

