# tre-agrep

## A fork of tre-agrep

This is a fork of `tre-agrep`, `https://github.com/laurikari/tre`

It has a few bug fixes and some enhancements.

## Why fork ?

1. `tre-agrep` does not seem to be actively maintained.

2. Some of the enhancements may not be for everyone.  But they work for me.

3. Especially, any changes in handling Unicode can be a source of controversy.

## What are the bug fixes?

### Fix #1 -- Unicode

`tre-agrep`, out of the box silently drops data if the input is not well-formed Unicode.
I have some sympathy for those who think we should all immediately convert everything to Unicode.
But silently dropping data does not seem to be the way to win over the unwashed masses.  To me, that is just a bug.

The printf family of functions can simply not print anything
if the format xxx is "%s" and the given string is not all valid UTF-8.
For that reason, I have replaced code that uses printf("%s", buf)
with fputs(buf, stdout).
fputs() may print out some Mojibake, but it does alway print something.

There is a school of thought that says that I get what I deserve
if I send invalid data to a program.  But, it is all too often to
go through daily life and accumulate some data from a combination
of web scraping from various sources and copy-and-paste and entering
notes by hand.  It can be hard to not have accented character or
a contraction with a Unicode apostrophe in an otherwise ASCII text file.
Many websites have Mojibake problems of their own.
Programming languages like Go, Python (2 and 3) behave in surprising
ways due to a mix of Unicode strings vs. paths vs. byte arrays, etc.
So, in this area, I need a Mojibake forgiving tool,
until things change.

### Fix #2

Also, one minor nit: the option '--delimiter' should require an argument.

### Fix #3

Also, `tre-agrep` would misbehave if the number of bytes remaining in the input buffer is less than the length of the delimiter.

One of the reasons for choosing `tre-agrep` in the first place is that the original `agrep` by Sun Wu and Udi Manber has a severe limit on the length of the record delimiter (maximum length of 8 bytes).

## What are the enhancements

### color

When the --color option is specified (and not --invert-match) all matches are shown in color, not just the first match.  This is consistent with GNU grep and with `ripgrep` and is what I believe most people would expect.

Note that --count shows the number of records that contain a match,
not the number of occurrences of matching text.  This is consistent with GNU grep and `ripgrep`.  I did not change it.

### indent

I added the option `--indent=NUM`.  With this option, Filenames are printed only once, without indentation, and all the rest of the information is shown indented.

This comes in really handy when the filenames are long paths.
Often, it is desirable to know what the filename is, but it need not be repeated as a prefix for all the match output.

I have implement the `indent=NUM` option for GNU grep, as well.

See `https://github.com/Guy-Shaw/grep-indent`


## Build

The file `agrep.c` is a drop-in replacement for the `agrep.c`
in https://github.com/laurikari/tre

You can build from sources following the direction from that github repository.
But, what I do on a Debian Linux based system (Ubuntu) is to download the
Debian source package using this recipe:

```
    apt-get install tre-agrep
    cd /path/to/source/build
    apt-get source tre-agrep
    replace agrep.c
    make
```

This gets whatever Debian modifications there are,
along with my changes.

Not recommended practice for much of anything,
but it works for me for for the simple drop-in replacement of one file.

I copy the binary to some other name besides `tre-agrep`,
so that name is reserved for the original.

## License

This is derived from `tre-agrep` and their license is good enough for me.

See https://github.com/laurikari/tre/blob/master/LICENSE

-- Guy Shaw

   gshaw@acm.org

   2010-Mar-01

