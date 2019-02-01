==============================================================
Reference codebase for Universal-ctags
==============================================================

This is the reference codebase for measuring the performance of
Universal-ctags parsers.


How to run ctags for the code base
==============================================================

We assume you may have enough storage space on your PC.

1. Get the input code for a parser for the language you are
   interested in with following command line:

   .. code-block::console

	$ ./codebase clone <LANGUAGE>


   The following command lists The available language :


   .. code-block::console

	$ ./codebase list-languages
	#           LANGUAGE	CODE
			   C	linux
			 C++	qtbase rocksdb
			  Go	buildah kubernetes
			HTML	cockpit
		  JavaScript	cockpit
		    LdScript	linux
		  ObjectiveC	gnustep-libs-base

2. Run Universal-ctags for the cloned code with following command line:

   .. code-block::console

	$ ./codebase ctags <LANGUAGE> [<PROFILE>]

   You can run ctags with different option combination.
   We call such option combination PROFILE.
   The following command is for listing pre defined profiles:

   .. code-block::console

	$ ./codebase list-profiles

		PROFILE		DESCRIPTION
		maximum0	Enables all extras, fields, and kinds
		minimum0	Disables all fields and extras.

  Results are displayed to your terminal. Tee'ed output goes
  results/ directory.


How to add your code to code base
==============================================================

You have to write a .lcopy file and put it to lcopy.d directory.
See lcopy.d/linux.lcopy as an example.


How to add your profile to preset list
==============================================================

You have to write a .ctags file and put it to profile.d directory.
A line started from "# @" is used as a description for the profile.
You may wan to use `--options-maybe` to extend profile without
modifying existing .ctags files.


Let's optimize ourt parsers!
Masatake YAMATO <yamato@redhat.com>
