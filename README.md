OSoDi (open source database & distribution = APP STORE)
=======================================================

summary / features
===================

* Create open source database containing source package descriptions of open
  source packages supporting most commonly used programming languages.
  Source package descriptions will contain strong cross language dependency
  information.

* Provide binary distributions (APP STORE). For unix like distributions this
  will be based on the [http://nixos.org](Nix package manager) or follow its
  main principles.

* Allow querying of the database

* Allow attaching information to packages such as vulnerabilities, package
  versions, and virtual packages in various ways. And such "attachments" may be
  signed and associated with accounts.
 
* Integrate with existing "software distributions" such as [http://brew.sh/](Homebrew) and 
  [http://nixos.org](Nix) allowing to serve cygwin, OSX, Linux systemwide or
  home directory installs.

* Try to be as reviewable and open to everybody as possible. Builds should be
  as deterministic as possible. This is important to build up and keep trust

* Publish all source so that other companies can verify builds results easily
  if they wish

background
==========
How many of the following packaging solutions do you know ? (arbitrary order)

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
* ? (C/C++)
* ? fortran
* ... (I'm sure I've been missing some important languages and tools)

How many of the following distributions do you know? (arbitrary order)

* Linux (nixos.org, Ubuntu, Fedora, Debian, Gentoo, Magela, Mint Linux, GoboLinux, Red Head)
* OSX Hemebrew
* Windows (cygwin, mingw)
* Android ?
* ...

You know there are many many more, see
[http://en.wikipedia.org/wiki/Linux_distribution](Wikipedia's list)

For example I'm not aware of any C/C++ package manager which can resolve dependencies on
its own (except gentoo/nixos distributions).

What do they all share? They all just want to provide a working set of packages
to serve users. The more effective those distributions are the better.

Developers usually want to get the packages they have to work on and start
coding - and they want support and have the dependencies "tagged" for instance.

Why learn about details of that many different packaging solutions and policies
which eventually still fail at tagging?

And even if you did you'd constantly be annoyed by gem/pip/... failing
because MySQL headers are not on your system and you'll have to retry
after adding those.

But the real problem to solve is cross language dependencies, for instance
* Inkscape has Python plugin support which requires additional libraries
* Vim plugins might require Vim with Ruby support and some compiled C code
* pythonocc depends on C libraries and Fortran.

You get the idea. I want to make installing *your setup* as painless as possible
on whatever system and languages you're working witr while being as trustworthy
and open to everyone as possible. I want to build a system for you I never had
I was missing since the beginning.

The solution part I:
====================
Introduce a database to store packages having at least those properties:
 * submission date (This is important to reproduce a setup from say from last year)
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
 * dependencies which depend on the flags
 ...

Store all those packages in a database and link a special dataset to each
package name providing information such as:
  * "recommended" version
  * differences (forks)
  * and similar

Allow 'virtual packages' which can be linked to different applications serving
similar purposes so that its easier for the user to understand which package to
try first.

Like Haskell allow creating "platforms". A platform is a set of commonly used
core packages.

part II: involving the community by supporting attaching information
====================================================================
* distributions can tell which versions they are using
* reviewers can mark packages to be reviewed which might help to
  tell companies which important open source is "underpowerd". It might help
  preventing bugs like the recent OpenSSL heartbleat bug
* users can comment
* attach continuous integration build results
* attach patches
* attach vulnerabilities

Result: You can run nice queries on your system, which packages
* are vulnerable
* have more recent (and reviewed!) versions
* ...

part III: The solver
====================
Yes - somehow there must be a way to actually turn such package database into
something usable: A compiled thing or compiled packages you can install.

Of course its hard to find the "best combination" of package versions you want
to use.

Before running any solver on the database you must know what you want:
* be bleeding edge
* be stable
* use what your neighbor nearby you is using so that you can ask in case of
  trouble?

Thus before asking a solver to find a set of packages to use you must block
some versions. A sane default would be:
- use versions the official maintainers of their packages recommend
- fall back to older versions unless there are known vulnerabilities
  if some packages cannot cope with the newer versions yet.

This project will implement different solvers strategies including SAT to find
the one serving you best.

part IV: building & distribution
================================
Let me first introduce you to [http://nixos.org](Nix package manager) briefly.
The main features are:
* atomic updates
* distributing builds on multiple machines
* allows to install per user/ system wide / as user
* most deterministic builds by making only build dependencies available in the
  build chroot
* already serves Darwin, Linux, Cygwin
* has a modular setup. Creating build recipes is separated from realizing the
  builds. There already exist 3 impementations: Nix, NiJS, Guix.

However at some point Nix will get to its limits: There are 50.000 Ruby
packages, countless perl packages, more and more Haskell packages.
You dont want to download 1.000.000 package descriptions in order to build 20
packages. Thus in the long it does make sense to move package descriptions
into the cloud.

Nix will be a primary target. Other distribution systems such as "Homebrew"
will be considered, too.

part V: future
==============
Once this is finished I'll work on declarative server management which allows
you to manage/transfer/backup/duplicate server services, networks *and state*.

My background
=============
I've been programming for about 15 years (Pascal, Delphi, some .net, JS, PHP,
Ruby, Python, Some C/C++, Haskell, Nix, VimL, Ocaml, Java, Scala, Haxe, ..) and
very often managing dependencies took more time than I wished it should.

I've been contributing to nixos.org which improves upon package management.
It introduces its own lazy language called "Nix" which builds up package build
recipes defining builds which then are built on one or more machines in parallel.

For that small "Nix" community two alternatives have been written:
  - Guix: a distribution focusing on GNU (still using the core of the Nix packaging system)
  - NiJS: Nix reimplemented in JS

This project tries to unify what all open source software distributions share:

The knowledge about which package combinations are known to work together so that
*YOU* can install and manage the software the way you want without missing
new/experimental software which you'll be able to compile in your home
directory for instance.

Thus you'll be able to say install a Python, Ruby, or Vim package along with
its C dependencies.

Steps to success (roadmap)
==============================
0) Think about which license exactly to use. We can use only 
    [https://www.bountysource.com/faq#what-types-of-projects-are-allowed-on-bountysource](open source licenses)
1) implement database & website
2) implement solver
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
Either by donating money (bountysource) or by helping implementing the features.

If you want to help just send me a [mailto:marco-oweber@gmx.de](mail) or
contact me on irc.freenode.net, nick MarcWeber.


CREDITS
=======
Of course most ideas are not new. Thus credits to everybody who is writing open
source or is working on/for any of the projects I mentioned.
I just want to unify this all.


link with:
http://stackoverflow.com/questions/7266097/are-there-any-efforts-to-create-a-package-manager-for-c
