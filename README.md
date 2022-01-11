# Demonstrate install using --tags

Meson 0.60.0 added a new feature when installing to limit files based
on 'tags'. This is going to be really nice for building packages for
Linux distributions, but needs a bit of refinement. This package can
be used to demonstrate how the current version works and some changes
that could be made to make it more useful.

Shared libraries have version numbers in their filenames. For example,
the 'foo' library with version '2.4.6' gets installed as:

 1. libfoo.so.2.4.6
 2. libfoo.so.2 -> libfoo.so.2.4.6
 3. libfoo.so -> libfoo.so.2

The first name is the full version of the library. The second is what
applications use when loading the library at runtime. The third name
is used by the linker when building other applications or libraries
that reference the foo library.

You may also have version 1.2.3 of the foo library installed, and that
would normally install:

 1. libfoo.so.1.2.3
 2. libfoo.so.1 -> libfoo.so.1.2.3
 3. libfoo.so -> libfoo.so.1

The last filename conflicts with the same one from version 2.4.6,
preventing us from installing both packages at the same time.

To make this work, meson should *not* install the bare '.so' name
along with other runtime files.

## Building the demo

Build the demo and install in a DESTDIR for demo purposes:

	$ meson build
	$ cd build
	$ ninja
	$ DESTDIR=$HOME/tmp meson install --tags runtime

