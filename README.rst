==============================================================
Reference codebase for Universal-ctags
==============================================================

This is the reference code base for measuring the performance of
Universal-ctags parsers.


How to run ctags for the code base
==============================================================

We assume you may have enough storage space on your PC.

1. Get the input code for a parser for the language you are
   interested in with following command line:

   .. code-block:: console

	$ ./codebase clone <LANGUAGE>


   The following command lists available languages:


   .. code-block:: console

	$ ./codebase list-languages
	#           LANGUAGE	CODE
			   C	linux
			 C++	qtbase rocksdb
			  Go	buildah kubernetes
			HTML	cockpit
		  JavaScript	cockpit
		    LdScript	linux
		  ObjectiveC	gnustep-libs-base

   An example preparing C source code:

   .. code-block:: console

	$ ./codebase clone C
	...

2. Run Universal-ctags for the cloned code with following command line:

   .. code-block:: console

	$ [CTAGS_EXE=${where your ctags executable is}] ./codebase ctags <LANGUAGE> [<PROFILE>]

   `codebase` refers *CTAGS_EXE* environment variable to run ctags. If
   CTAGS_EXE is not defined, `ctags` is used as the command name.

   You can run ctags with different option combination.
   We call such option combination *PROFILE*.
   The following command is for listing predefined profiles:

   .. code-block:: console

	$ ./codebase list-profiles

		PROFILE		DESCRIPTION
		maximum0	Enables all extras, fields, and kinds
		minimum0	Disables all fields and extras.


   Results are displayed to your terminal. Tee'ed output goes
   to a file under results/ directory.

   An example command line for running C parser:

   .. code-block:: console

	$ cd /home/yamato/hacking/ctags-github; ./autogen.sh; ./configure; make
	$ cd /home/yamato/hacking/codebase
	$ CTAGS_EXE=/home/yamato/hacking/ctags-github/ctags ./codebase ctags C
	version: 2c46f6d4
	features: +wildcards +regex +iconv +option-directory +xpath +json +interactive +sandbox +yaml +aspell +packcc
	log: results/2c46f6d4,C...................,..........,time......,default...,2019-03-18-12:47:20.log
	tagsoutput: /dev/null
	cmdline: + /home/yamato/var/ctags-github/ctags --quiet --options=NONE --sort=no --options=profile.d/maps --totals=yes --languages=C -o - -R code/linux code/php-src code/ruby
	27886 files, 19784832 lines (555082 kB) scanned in 15.1 seconds (36686 kB/s)
	1158391 tags added to tag file

	real	0m15.172s
	user	0m14.735s
	sys	0m0.399s
	+ set +x

   In the above output, "36686 kB/s" represents the speed of parsing of C parser.
   "1158391 tags" represents the number of tags captured by C parser.


How to add your code to code base
==============================================================

The code to be added to the code base must have a git repository.

You have to write a .lcopy file and put it to lcopy.d directory.
See lcopy.d/linux.lcopy as an example::

    REPO=https://github.com/torvalds/linux
    ALIGNMENT=v4.20
    LANGUAGES=C,LdScript,Asm,Kconfig,DTS

`codebase` commands loads a .lcopy file with source built-in command. In
a .lcopy file must define `REPO`, `ALIGNMENT`, and `LANGUAGES`.

`REPO` specifies a git repository.  `ALIGNMENT` is a tag put on the
git repository.  `ALIGNMENT` allows users of `codebase` to get the same
source tree. Specify a git tag or commit as `ALIGNMENT`. A git branch
is not acceptable. `LANGUAGES` is a comma separated language list.


How to add a new profile
==============================================================

Add a .ctags file under profile.d directory.
In the .ctags file, a line started from "# @" is used as a
description for the profile.

You may want to use `--options` to extend profile without
modifying existing .ctags files. If you want to extend `base.ctags`
in the profile.d directory, write your profile (`extended.ctags`)
as following::

  --options=base.ctags
  ...

Let's optimize our parsers!
Masatake YAMATO <yamato@redhat.com>
