# cl-tools: Legal history toolkit to work with CourtListener data

Last updated: October 31, 2018

## Overview

The `cl-tools` utilities were written by Eric
Nystrom (<http://ericnystrom.org>) to analyze full-text court
opinion data from <http://www.courtlistener.com>, for example by
examining key words in context (KWIC).

They were developed for a use case that foused exclusively on the
U.S. Supreme Court, and therefore incorporates a few output
fields that are specific to Supreme Court data. However, the
utilities should still function correctly if this information is
not present, e.g. if looking at opinions from other
jurisdictions.

The utilities are all licensed under the MIT License. In addition
to this README, most of the scripts are excessively commented. If
you find these useful, I'd be very interested to hear how you are
using them (via email, Github, Twitter, website).

## Tools

- `cl-opinion-kwic`: Search for a key word in opinion text, output it
  in context with additional info. (Perl)
- `cl-kwic-follower-map`: Using output from `cl-opinion-kwic`, maps
  the initial word of each phrase following the keyword to some kind
  of category, using an external TSV file of the words and their
  relations (gawk)
- `cl-kwic-follower-group`: Using output from `cl-kwic-follower-map`,
  groups those categorized keyword usages by case, and also notes
  percentages of each case's uses that fit into each detected category (gawk)
- `cl-weed-dupes`: Applies a *manually*-created duplicates list
  (2-column TSV duplicate, duplicate-of) to weed duplicates from
  `cl-opinion-kwic` output based on CL number (awk)

## Installation

Download and place somewhere in your $PATH, or call the scripts
with a complete path on the command line.

*Dependencies:* `cl-opinion-kwic` depends on Perl and the
 packages HTML::Strip, Text::Unidecode, LibJSON, Perl6::Slurp,
 and Getopt::Long. These can be downloaded from CPAN or are
 provided in Debian by the `libhtml-strip-perl`,
 `libtext-unidecode-perl`, `libjson-perl`, and
`libperl6-slurp-perl` packages. `cl-kwic-follower-map` and
 `cl-kwic-follower-group` depend on gawk; `cl-weed-dupes` depends on
 any type of awk.

## Usage

`cl-opinion-kwic -w TERM -l CLUSTER-LOCATION [-c NUMBER OF CONTEXT WORDS] [-s SEPARATOR] FILE.json`

## Output fields

By default, `cl-opinion-kwic` provides ten columns of output in a
TAB-separated format, with the first row of output containing a header
with column names. Information from columns 2-6 is extracted from CL
cluster files.

1. cl-opinion: The CourtListener opinion number for the case.
2. scdb: The Supreme Court Database reference number for the opinion
(if available)
3. title: Case title
4. cite: Best-guess case reporter citation
5. date: Date opinion was issued
6. vote: Majority-minority vote in the case
7. casematch: The sequential number of this keyword match within the
case text
8. before: phrase before the keyword (default 5 words, control with
    `-c` flag)
9. word: the keyword itself (set with `-w` flag)
10. after: phrase after the keyword 

## Caveats

There are likely bugs, please report them! These programs were
developed on a Debian Linux system. Suggestions to enhance portability
to other Unix-like systems would be welcomed. "Court Listener" is
undoubtedly a trademark of its owner, not me.
