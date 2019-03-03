<a href="https://raw.githubusercontent.com/hackerb9/ugrep/master/README.md.d/screenshot.png">
<img title="ugrep screenshot" alt-text="Example of ugrep vine" align="right" src="README.md.d/screenshot.png" width="50%">
</a>

# ☙ ugrep ❧

_Find unicode characters based on their names_

ugrep is essentially [grep](https://www.gnu.org/software/grep/) for
the Unicode table. It prints out the resulting unicode characters
literally, so you can easily cut-and-paste. Ugrep is useful for
looking up Emojis 😤, finding obscure symbols ⚸⅗ℏ℞☧☭, or beautiful
glyphs to decorate your text. 🙶❡✯🟔❢🙷

You can also use it for the reverse operation to lookup a single
character you've pasted into the terminal.

## Usage

Usage: **ugrep** _regex_

Where _regex_ is a regular expression. If you don't know [regular
expressions](https://docs.python.org/3/howto/regex.html), don't worry.
Just use plain strings.

### Examples:

Note: output from all examples has been excerpted. (You'd be amazed
how many heart emojis Unicode has. 😜)

* Plain text search is simple:

	    $ ugrep heart
	    ☙	U+2619	REVERSED ROTATED FLORAL HEART BULLET
	    ❣	U+2763	HEAVY HEART EXCLAMATION MARK ORNAMENT
	    ❤	U+2764	HEAVY BLACK HEART
	    ⋮	[ ... truncated for brevity ... ]
	    💞	U+1F49E REVOLVING HEARTS
	    💟	U+1F49F HEART DECORATION
	    😍	U+1F60D SMILING FACE WITH HEART-SHAPED EYES
	    😻	U+1F63B	SMILING CAT FACE WITH HEART-SHAPED EYES

* Arguments on the command line have an implicit wildcard between them:

	    $ ugrep right.*gle
	    $ ugrep right gle       # Equivalent
	    »	U+00BB	RIGHT-POINTING DOUBLE ANGLE QUOTATION MARK
	    ’	U+2019	RIGHT SINGLE QUOTATION MARK
	    ∟	U+221F	RIGHT ANGLE
	    ⊿	U+22BF	RIGHT TRIANGLE

* You can use regular expressions for fancier searches: 

	    $ ugrep "\bR\b"         # The letter R used as a word
	    R	U+0052  LATIN CAPITAL LETTER R
	    Ŗ	U+0156  LATIN CAPITAL LETTER R WITH CEDILLA
	    ℛ	U+211B  SCRIPT CAPITAL R (Script r)
	    ℜ	U+211C  BLACK-LETTER CAPITAL R (Black-letter r)
	    ℝ	U+211D  DOUBLE-STRUCK CAPITAL R (Double-struck r)

	    $ ugrep ^x		    #  Regex anchors ^ and $ work
	    ⊻	U+22BB	XOR
	    ⌧	U+2327	X IN A RECTANGLE BOX (clear key)

* Aliases (alternate names) are also searched:

	    $ ugrep backslash
	    \	U+005C	REVERSE SOLIDUS (backslash)

* Browse through all unicode characters:

	    $ ugrep . | less
	    ⋮
	    ⚳	U+26B3	CERES
	    ⚴	U+26B4	PALLAS
	    ⚵	U+26B5	JUNO
	    ⚶	U+26B6	VESTA
	    ⚷	U+26B7	CHIRON
	    ⚸	U+26B8	BLACK MOON LILITH
	    ⚹	U+26B9	SEXTILE
	    ⚺	U+26BA	SEMISEXTILE
	    ⚻	U+26BB	QUINCUNX
	    ⋮

	Sometimes it's useful to page through the unicode table and
	see what characters are defined in a region. (Tip: search for
	a code point in `less` by pressing `/U\+A60F`).

### Fun things to try:

To see some useful and lovely glyphs, try this:

    ugrep face 
    ugrep alchemical 
    ugrep ornament
    ugrep bullet
    ugrep '(vine|bud)'
    ugrep vai
    ugrep heavy
    ugrep drawing

## Prerequisite: UnicodeData.txt

You must have a copy of
[UnicodeData.txt](https://unicode.org/Public/UNIDATA/UnicodeData.txt)
installed.

**Easiest**: On Ubuntu and Debian GNU/Linux, simply `apt install unicode-data`.

**Still easy**: Or, you can download it by hand from
[unicode.org](https://unicode.org/Public/UNIDATA/UnicodeData.txt)
and place it in `~/.local/share/unicode/UnicodeData.txt`

**Not hard**: Or, if you wish the file to be accessible to all users on
your machine, place it in `/usr/local/share/unicode/UnicodeData.txt`.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 

## Installation

It's just a shell script. Download it to `/usr/local/bin` or `~/bin`
and make it executable.

    cd /usr/local/bin
    wget https://github.com/hackerb9/ugrep/blob/master/ugrep
    chmod 755 ugrep

## Boring Implementation notes

This is a rewrite of b9's AWK ugrep into Python. While AWK makes more
sense for what this program does (comparing fields based on regexps),
a rewrite was necessary because GNU awk, while plenty powerful, uses
`\y` for word edges instead of the standard `\b`. Gawk does this for
backwards compatibility with historic AWK, but lacks a way to disable
it for new scripts.

Switching to Python did have the benefit of allowing more powerful
Perlesque regexes (not that anyone has requested that).

### Why not use unicodedata module?

I do not use Python's `unicodedata` module because it is woefully
insufficient. It allows one to search by character name only by
specifying it fully and exactly: `unicodedata.lookup("ROTATED HEAVY
BLACK HEART BULLET")`.

## Future Work

### Maybe use NamesList.txt

It looks like
[`NamesList.txt`](https://unicode.org/Public/UNIDATA/NamesList.txt)
might be useful to also parse as it allows multiple aliases for a
character. For example (from `grep -B1 [=%] NamesList.txt`):

    0023    NUMBER SIGN
            = pound sign, hash, crosshatch, octothorpe

    002E    FULL STOP
            = period, dot, decimal point
    --
    002F    SOLIDUS
            = slash, virgule

    1F70A   ALCHEMICAL SYMBOL FOR VINEGAR
            = crucible; acid; distill; atrament; vitriol; red
              sulfur; borax; wine; alkali salt; mercurius vivus,
              quick silver

I'm not sure how useful this will be (who is going to look up the
number sign by searching on "octothorpe"), but it'd be nice to be able
to at least show them as aliases.

Also, NamesList.txt has a fascinating "cross reference" feature:

    0021    EXCLAMATION MARK
            = factorial
            = bang
            x (inverted exclamation mark - 00A1)
            x (latin letter retroflex click - 01C3)
            x (double exclamation mark - 203C)
            x (interrobang - 203D)
            x (heavy exclamation mark ornament - 2762)

How would one find the interrobang (‽) without such a cross reference?

Note that the NamesList.txt file actually starts with a warning *not*
to parse it as it says it is generated mechanically from
UnicodeData.txt plus "manually created annotations". However, those
annotations are what is interesting about the file (the aliases and
cross references) and there appears to be no other official source of
that data.
