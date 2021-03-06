cross-compilers
---------------

Dockerfiles for cross compiling in a Docker container.

Features
========

* different toolchains for cross compiling
* commands in the container are run as the calling user, so that any created files have the expected ownership (ie. not root)
* make variables (`CC`, `LD` etc) are set to point to the appropriate tools in the container
* cmake and ninja are precompiled available with a toolchain file for cmake
* current directory is mounted as the container's workdir, `/build`
* works with boot2docker on OSX and Docker for Mac beta (1.11.1-beta12)

Installation
============

This image is not intended to be run manually. Instead, there is a helper script which comes bundled with the image.

To install the helper script, run the image with no arguments, and redirect the output to a file::

        docker run CROSS_COMPILER_IMAGE_NAME > dockcross
        chmod +x dockcross
        mv dockcross ~/bin/

Usage
=====

For the impatient, here's a one-liner to compile a hello world for armv7::

    docker run thewtex/cross-compiler-linux-armv7 > dockcross && chmod +x dockcross && ./dockcross gcc test/C/hello.c -o hello_arm

Note how invoking any toolchain command (make, gcc, etc...) is just a matter of prepending **dockcross** in the commandline:

`dockcross [command] [args...]`

Dockcross will thus execute the given command-line inside the container, along with all arguments passed after the command.

Alternatively, if the command matches one of the dockcross built-in commands (see below), that will be executed locally.


Built-in commands
=================

- `dockcross -- [command] [args...]`: Forces a command to run inside the container (in case of a name clash with a built-in command), use `--` before the command.
- `dockcross update-image`: Fetch the latest version of the docker image.
- `dockcross update-script`: Update the installed dockcross script with the one bundled in the image.
- `dockcross update`: Update both the docker image, and the dockcross script.

Configuration
=============

The following command-line options and environment variables are used. In all cases, the command-line option overrides the environment variable.

### DOCKCROSS_CONFIG / --config <path-to-config-file>

This file is sourced if it exists.

Default: `~/.dockcross`

### DOCKCROSS_IMAGE / --image <docker-image-name>

The docker image to run.

Default: thewtex/cross-compiler-linux-armv7

### DOCKCROSS_ARGS / --args <docker-run-args>

Extra arguments to pass to the `docker run` command.

Examples
========

1. **dockcross make**: Build the Makefile in the current directory.
2. **dockcross cmake -Bbuild -H. -GNinja***: Run CMake with a build directory "build" for the CMakeLists.txt in the current directory and generate `ninja` files.
3. **dockcross ninja -Cbuild**: Run ninja in the generated build directory.
4. **dockcross bash -c 'find . -name \*.o | sort > objects.txt'**

Note that commands are executed verbatim. If you require any shell processing for environment variable expansion or redirection, please use `bash -c 'command args...'`.

---

Credits go to `sdt/docker-raspberry-pi-cross-compiler <https://github.com/sdt/docker-raspberry-pi-cross-compiler>`_, who invented the base of this **dockcross** script.


CI status
---------

.. image:: https://circleci.com/gh/thewtex/cross-compilers/tree/master.svg?style=svg
  :target: https://circleci.com/gh/thewtex/cross-compilers/tree/master


.. |base-images| image:: https://badge.imagelayers.io/thewtex/cross-compiler-base:latest.svg
  :target: https://imagelayers.io/?images=thewtex/cross-compiler-base:latest

thewtex/cross-compiler-base
  |base-images| Base image for other toolchain images. From Debian Jessie with GCC,
  make, autotools, CMake, Ninja, Git, and Python.


.. |android-arm-images| image:: https://badge.imagelayers.io/thewtex/cross-compiler-android-arm:latest.svg
  :target: https://imagelayers.io/?images=thewtex/cross-compiler-android-arm:latest

thewtex/cross-compiler-android-arm
  |android-arm-images| The Android NDK standalone toolchain for the arm
  architecture.


.. |browser-asmjs-images| image:: https://badge.imagelayers.io/thewtex/cross-compiler-browser-asmjs:latest.svg
  :target: https://imagelayers.io/?images=thewtex/cross-compiler-browser-asmjs:latest

thewtex/cross-compiler-browser-asmjs
  |browser-asmjs-images| The Emscripten JavaScript cross compiler.


.. |linux-armv6-images| image:: https://badge.imagelayers.io/thewtex/cross-compiler-linux-armv6:latest.svg
  :target: https://imagelayers.io/?images=thewtex/cross-compiler-linux-armv6:latest

thewtex/cross-compiler-linux-armv6
  |linux-armv6-images| Linux ARMv6 cross compiler toolchain for the Raspberry
  Pi, etc.


.. |linux-armv7-images| image:: https://badge.imagelayers.io/thewtex/cross-compiler-linux-armv7:latest.svg
  :target: https://imagelayers.io/?images=thewtex/cross-compiler-linux-armv7:latest

thewtex/cross-compiler-linux-armv7
  |linux-armv7-images| Generic Linux armv7 cross compiler toolchain.


.. |linux-ppc64le-images| image:: https://badge.imagelayers.io/thewtex/cross-compiler-linux-ppc64le:latest.svg
  :target: https://imagelayers.io/?images=thewtex/cross-compiler-linux-ppc64le:latest

thewtex/cross-compiler-linux-ppc64le
  |linux-ppc64le-images| Linux PowerPC 64 little endian cross compiler
  toolchain for the POWER8, etc.


.. |linux-x64-images| image:: https://badge.imagelayers.io/thewtex/cross-compiler-linux-x64:latest.svg
  :target: https://imagelayers.io/?images=thewtex/cross-compiler-linux-x64:latest

thewtex/cross-compiler-linux-x64
  |linux-x64-images| Linux x86_64 / amd64 compiler. Since the Docker image is
  natively x86_64, this is not actually a cross compiler.


.. |linux-x86-images| image:: https://badge.imagelayers.io/thewtex/cross-compiler-linux-x86:latest.svg
  :target: https://imagelayers.io/?images=thewtex/cross-compiler-linux-x86:latest

thewtex/cross-compiler-linux-x86
  |linux-x86-images| Linux i686 cross compiler.


.. |windows-x64-images| image:: https://badge.imagelayers.io/thewtex/cross-compiler-windows-x64:latest.svg
  :target: https://imagelayers.io/?images=thewtex/cross-compiler-windows-x64:latest

thewtex/cross-compiler-windows-x64
  |windows-x64-images| 64-bit Windows cross-compiler based on MXE/MinGW-w64.


.. |windows-x86-images| image:: https://badge.imagelayers.io/thewtex/cross-compiler-windows-x86:latest.svg
  :target: https://imagelayers.io/?images=thewtex/cross-compiler-windows-x86:latest

thewtex/cross-compiler-windows-x86
  |windows-x86-images| 32-bit Windows cross-compiler based on MXE/MinGW-w64.
