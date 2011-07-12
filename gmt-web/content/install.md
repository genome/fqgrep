## PREREQUISITES

`fqgrep` depends upon the following libraries:

* [TRE Regular Expression Library](http://laurikari.net/tre/) version 0.8.0.
* [zlib](http://www.zlib.net)

They will need to be available on your system before fqgrep can be
installed.

### TRE Regular Expression Library

On Ubuntu (or any other Debian-based Linux distribution):

<p class="terminal">sudo apt-get install libtre-dev libtre5</p>

On Mac OS X ( via [macports](http://www.macports.org/) ):

<p class="terminal">sudo port install tre</p>

For other installation alternatives please visit the [TRE download page](http://laurikari.net/tre/download/).

### zlib

On Ubuntu (or any other Debian-based Linux distribution):

<p class="terminal">sudo apt-get install zlib1g zlib1g-dev</p>

### Path::Class (optional)

Usage of the example trimmer, `fqgrep-trim.pl`, provided in the [scripts
subdirectory](https://github.com/genome/fqgrep/tree/master/scripts)
of this git repository, may require the installation of the [`Path::Class` perl
module](http://search.cpan.org/~kwilliams/Path-Class-0.24/lib/Path/Class.pm)
onto your system.

On Ubuntu (or any other Debian-based Linux distribution):

<p class="terminal">sudo apt-get install libpath-class-perl</p>

Otherwise, if on a UNIX-based system (including Mac OS X), please try
the following terminal command for "Path::Class" installation:

<p class="terminal">sudo cpan Path::Class</p>

## INSTALLATION

The installation process currently consists of a very simple Makefile.

Just do the following:

<p class="terminal">git clone git://github.com/genome/fqgrep.git;<br />
cd fqgrep;<br />
make;<br />
<br />
# Or, alternatively,<br />
# try 'make genome'   if at The Genome Institute at Washington University<br />
#                     or would like to create an executable with all the<br />
#                     relevant libraries statically compiled into it<br />
#<br />
# try 'make macports' if installing on Mac OS X and you installed the<br />
#                     TRE library via [macports](http://www.macports.org/)<br />
</p>

The `fqgrep` executable should be in the working directory.  Afterwards,
you can move the executable to wherever you wish.  Usually, this is the
directory `/usr/local/bin` .
