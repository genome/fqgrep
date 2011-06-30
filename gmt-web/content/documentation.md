# NAME

`fqgrep` - a tool for searching sequences in FASTQ/FASTA files

# SYNOPSIS

    fqgrep [options] -p <pattern> <fastq_or_fasta_files>
            -h                  This help message
            -V                  Program and version information
            -p <STRING>         Pattern of interest to grep [REQUIRED]
            -v                  Invert match - show only sequences that
                                DO NOT match the pattern
            -a                  Show all records irregardless of match status
                                Useful in conjunction with the -r option;
                                when one would like to do further post-processing
                                of the match data
            -c                  Highlight matching string with color
            -f                  Output matches in FASTA format
            -r                  Output matches in detailed stats report format
            -b <STRING>         Delimiter string to separate columns
                                in detailed stats report [Default: '\t']
            -m <INT>            Total number of mismatches to at most allow for
                                in search pattern [Default: 0]
            -s <INT>            Max threshold of substitution mismatches to allow
                                for in search pattern [Default: unlimited]
            -i <INT>            Max threshold of insertion mismatches to allow
                                for in search pattern [Default: unlimited]
            -d <INT>            Max threshold of deletion mismatches to allow for
                                in search pattern [Default: unlimited]
            -S <INT>            Cost of base substitutions in obtaining
                                approximate match [Default: 1]
            -I <INT>            Cost of base insertions in obtaining
                                approximate match [Default: 1]
            -D <INT>            Cost of base deletions in obtaining
                                approximate match [Default: 1]
            -e                  Force tre regexp engine usage
            -C                  Display only a total count of matches
                                (per input FASTQ/FASTA file)
            -o <out_file>       Desired output file.
                                If not specified, defaults to stdout

# Software Details

## Pattern Matching Algorithms Used

When simply searching for exact sequence patterns, or in other words,
when zero mismatches on a pattern are specified (the default case), the
[Boyer-Moore](http://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_string_se
arch_algorithm) approach (as used similarly by `grep`) is taken.

`fqgrep` does not use an alignment strategy for approximate sequence
searching. For approximate pattern matching, the [TRE regular expression
library](http://laurikari.net/tre/) is being used.

The TRE library uses a variant of [Bitap
algorithm](http://en.wikipedia.org/wiki/Bitap_algorithm) for approximate
string matches. Below is an excerpt from the README from the TRE
software distribution describing the notions behind the bitap algorithm:

<blockquote>
    Approximate pattern matching allows matches to be approximate, that
   is, allows the matches to be close to the searched pattern under
   some measure of closeness. TRE uses the edit-distance measure (also
   known as the <a href="http://en.wikipedia.org/wiki/Levenshtein_distance">Levenshtein distance</a> ) where characters can be
   inserted, deleted, or substituted in the searched text in order to
   get an exact match. Each insertion, deletion, or substitution adds
   the distance, or cost, of the match. TRE can report the matches
   which have a cost lower than some given threshold value. TRE can
   also be used to search for matches with the lowest cost.
</blockquote>

These above mentioned insertion, deletion, and substitution cost
parameters can be adjusted by the `-m`, `-s`, `-i`, `-d`, `-S`, `-I`,
`-D` options to `fqgrep`. The exact costs for each match can be observed
via the detailed stats report (the `-r` option to `fqgrep`).

The TRE regular expression library is the default regular expression
engine used in [R](http://www.r-project.org); due to this, the results
of `fqgrep` maybe similar to the results produced by software from the
[Bioconductor Project](http://www.bioconductor.org).

## Detailed Match Statistics Report (`-r` option)  Notes

The `-r` option to `fqgrep` produces a detailed match report of each
record analyzed in the input FASTQ/FASTA file. Each field of the output
format is delimited with tabs by default. The delimiter string can be
altered by the `-b` option.

Below are the columns displayed in the statistics report:

    0   read name
    1   total mismatches
    2   # insertions
    3   # deletions
    4   # substitutions
    5   start position
    6   end position
    7   match string
    8   sequence
    9   quality

## Controlling Match Outputs

By default, the output of fqgrep only displays FASTQ/FASTA records that
have matched a pattern. Using the `-v` option, will invert the output,
in other words, show records that DO NOT match a given pattern.

The `-a` option will show all the records in a FASTQ/FASTA file (both
that MATCH and DO NOT MATCH a given input pattern). This is useful in
conjunction with the `-r` option to perform custom post-processing on
read data. See the 'A simple adaptive trimmer' section below for an
example.

When a sequence match is not present for a given record (via the `-v` or
`-a` options). The "match string" column just contains the string '\*'.

## Other Pattern Matching Notes

* [Only POSIX styled regular expression can be used with
fqgrep](http://en.wikipedia.org/wiki/Regular_expression#POSIX).
* When searching for an exact pattern (0 mismatches) via a regular
expression, please use the '-e' option. This forces the use of the TRE
regular expression engine.

## Reading from Standard Input (STDIN)

If a dash ('-') is used as the input file, it will read the FASTQ/FASTA
input from standard input (`stdin`). This is useful when piping the
output of one `fqgrep` command (or any other program that produces
FASTQ/FASTA formatted output) into another `fqgrep` command. Supplying
anything other than a FASTQ/FASTA formatted input in this manner may
give unexpected behavior to `fqgrep`. See the examples below for
demonstrative usage.

# Usage & Examples

## Basic Usage

Search for the sequence pattern 'GATTACA' anywhere in the input reads
file, `input.fq`, a FASTQ file.

_Note that an input can be a FASTA file as well. GZipped input FASTA
(input.fa.gz) and FASTQ (input.fq.gz) files are acceptable as well._

    fqgrep -p 'GATTACA' input.fq

## Outputting in FASTA format (-f option)

    fqgrep -f -p 'GATTACA' input.fq

## Highlighting matches with color (-c option)

    fqgrep -c -f -p 'GATTACA' input.fq

## Allowing pattern mismatches (-m option)

Allow up one mismatch (insertion, deletion, or substitutions) in the pattern.

    fqgrep -c -f -p 'GATTACA' -m 1 input.fq

## Usage of regular expressions

At the beginning of each read, allow for a variable 0 to 3 bases (A, C, G, T, or N) followed by the pattern 'GATTACA'.  Plus, within the 'GATTACA' string, allow for at most a couple mismatches.

    fqgrep -c -f -p '^[AGCTN]{0,3}GATTACA' -m 2 input.fq

## View a Stats Report of the Matches (-r option)

View a detailed statistics report of just the matches in the above command:

    fqgrep -c -r -p '^[AGCTN]{0,3}GATTACA' -m 2 input.fq

## Reading from a pipe or STDIN

Usage of a single dash, <tt>'-'</tt>, as a file name allows for reading a FASTQ or FASTA file from STDIN.

    cat input.fq | fqgrep -p 'GATTACA' -

_Please note that when reading from standard input, `fqgrep` is
expecting an input of a FASTQ or FASTQ format. Not a detailed statistics
report format. See the 'A simple adaptive trimmer' section below for an
example observation._

## Rudimentary Set Operations

### Set Inversion (-v option)

Show all the reads (in FASTQ format) that DO NOT exactly start with 'GATTACA':

    fqgrep -v -e -p '^GATTACA' input.fq

### Set Intersection

Show all the reads (in FASTQ format) that DO NOT start with 'GATTACA'
AND end with a polyA sequence (8 or more As) with a couple of
mismatches:

    fqgrep -v -e -p '^GATTACA' input.fq | fqgrep -m 2 -p 'A{8,}$' -

## Example post-processing with `fqgrep`

### A simple adaptive trimmer

The following command produces a cheap 5' and 3' end trimmer (assuming
all the reads in the FASTQ file are oriented in a 5' to 3' direction).

    fqgrep -m 1 -r -p '^[AGCTN]{0,3}GATTACA' input.fq | trimmer --5-prime | fqgrep -e -r -p 'AAAAAA$' - | trimmer --3-prime    
    
The first command: 

    fqgrep -a -m 1 -r -p '^[AGCTN]{0,3}GATTACA'  input.fq

says to identify all the reads in the input file that may have 0-3
random bases (A, G, C, T or N) followed by the sequence `GATTACA`
allowing for a single mismatch (`-m` option) and output all the fastq
records (irregardless if there there is a match or not, `-a` option) in
the tab-delimited statistics report format (`-r` option) . This output
is taken as input into the next piped command:

    trimmer --5-prime
 
[`trimmer`](https://github.com/genome/fqgrep/blob/master/scripts/trim
mer) is an example perl script that produces FASTQ formatted output from
its supplied `fqgrep` statistics report input. For each input record,
`trimmer` will trim out the identified match string from the 5' end, if
there was a match. If there was no match, it does nothing and simply
re-prints out the original fastq record. `trimmer`'s FASTQ output serves
as the input to the next piped `fqgrep` command:

     fqgrep -a -e -r -p 'AAAAAA$' - 

The above statement says to find all the reads supplied from standard
input that have the exact (0 mismatches) polyA sequence `AAAAAA` on its
3' end. As a regular expression is being supplied for the `-p` search
pattern, the `-e` option of fqgrep is used to ensure the usage of the
regular expression engine in the pattern search. The output of the above
is inputted into the following subsequent trimming command:

    trimmer --3-prime

The above statement performs a similar function as the prior `trimmer`
command, but trims from the 3' end instead.

The above
[`trimmer`](https://github.com/genome/fqgrep/blob/master/scripts/trim
mer) script and another example script
[`fqgrep-trim.pl`](https://github.com/genome/fqgrep/blob/master/scrip
ts/fqgrep-trim.pl) are supplied in the fqgrep package. They are both
simplistic trimmers. The `fqgrep-trim.pl` script performs similiarly to
the `trimmer` script, but provides a differing output format and user
interface. See the documentation inside the `fqgrep-trim.pl` script for
more details.

One could imagine creating more complex trimmers based upon the quality
score of the read, the experimental design generating the reads, the
sequencing technology being used and other notions. This is left for the
user to further customize for the particular scientific problem at hand.

# Useful References

* [TRE Regex Syntax Manual](http://laurikari.net/tre/documentation/regex-syntax/)

# AUTHOR

`fqgrep` was written by Indraniel Das <idas@wustl.edu>. 

# DISCLAIMER

This software is provided "as is" without warranty of any kind.

# COPYRIGHT

`fqgrep` is free software, distributed under the terms of the [GNU GPL v3 or later](http://gnu.org/licenses/gpl.html).
