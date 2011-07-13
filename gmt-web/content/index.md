## Introduction
* * *

`fqgrep` is a [grep](http://en.wikipedia.org/wiki/Grep)
(and [agrep](http://en.wikipedia.org/wiki/Agrep))
like tool optimized for matching sequence patterns in
[FASTA](http://en.wikipedia.org/wiki/Fasta_format) and
[FASTQ](http://en.wikipedia.org/wiki/FASTQ_format) files. With it, one
could:

* perform rudimentary exploratory data analysis on the read data
* identify & collect subset of reads within a larger grouping having 
  common sequence characteristics 
* design & construct custom adaptive trimmers, and/or demultiplexers
* enable other custom pre-processing of the read data before its 
  presentation to a more intensive analysis pipeline


## Features
* * *

The basic features of `fqgrep` are:

* search for a sequence pattern using a POSIX regular expression
* allow for specified mismatches in sequence search pattern
* fine tune both the mismatch weights and the total number of allowable 
  respective insertions, deletions, and substitutions in the sequence 
  search pattern
* highlight/color the matching substring within the overall read
* streaming ability (can pipe the output of one `fqgrep` command into 
  another `fqgrep` instance, or other command), for identifying particular 
  combinations of sequence patterns within a FASTQ/FASTA input file(s)
* generation of parsable detail match statistics reports on each read for 
  custom post-processing

See [Documentation](documentation.html) page for further details.
