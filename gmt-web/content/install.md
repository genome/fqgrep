# PREREQUISITES

`fqgrep` depends upon the following libraries:

  * [TRE Regular Expression Library](http://laurikari.net/tre/) version 0.8.0.
  * [zlib](http://www.zlib.net)

They will need to be available on your system before fqgrep can be
installed.

## TRE Regular Expression Library

On Ubuntu (or any other Debian-based Linux distribution):

    sudo apt-get install libtre-dev libtre5

On Mac OS X ( via macports [http://www.macports.org/] ):

    sudo port install tre

For other installation alternatives please visit the [TRE download page](http://laurikari.net/tre/download/).

## zlib

On Ubuntu (or any other Debian-based Linux distribution):

    sudo apt-get install zlib1g zlib1g-dev

## Path::Class (optional)

Usage of the example trimmer, `fqgrep-trim.pl`, provided in the [scripts
subdirectory](https://github.com/genome/fqgrep/tree/master/scripts)
of this git repository, may require the installation of the [`Path::Class` perl
module](http://search.cpan.org/~kwilliams/Path-Class-0.24/lib/Path/Class.pm)
onto your system.

On Ubuntu (or any other Debian-based Linux distribution):

    sudo apt-get install libpath-class-perl

Otherwise, if on a UNIX-based system (including Mac OS X), please try
the following terminal command for "Path::Class" installation:

    sudo cpan Path::Class

# INSTALLATION

The installation process currently consists of a very simple Makefile.

Just do the following:

  git clone git://github.com/genome/fqgrep.git;
  cd fqgrep;
  make; # try 'make genome'   if at The Genome Institute at Washington University
        #                     or would like to create an executable with all the
        #                     relevant libraries statically compiled into it
        #
        # try 'make macports' if installing on Mac OS X and you installed the
        #                     TRE library via macports (http://www.macports.org/)

The `fqgrep` executable should be in the working directory.  Afterwards,
you can move the executable to wherever you wish.  Usually, this is the
directory `/usr/local/bin` .
