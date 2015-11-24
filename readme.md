# IWYU

IWYU is a small utility that _helps_ you include the right C++ headers. It helps
you detect superfluous headers as well as potentially missing headers.

_IWYU requires PLY. Run `pip install --user ply` to install it_.

The tool is not particularly smart. The first issue is that IWYU doesn't
compile, parse or even preprocess the code. It merely relies on lexical
analysis to extract identifiers and their potential namespaces.

Secondly, it relies on manual input to learn where each symbol comes from. If
the tool encounters a symbol in a namespace you're interested in that it hasn't
seen before, it will ask you to input the correct header.

Despite these limitations the tool is fast and useful. The database is in plain
text and is intended to be edited. It contains all the symbols and their header
files, and has some tricks up it's sleeve:

1. Start a line with a question mark (?) to track a particular type of
   identifier. When tracking an identifier it means IWYU will stop ignoring a
   symbol and ask you to input it's header. Supports wildcards. Example:

        ? std::*

2. Start a line with an exclamation mark (!) to ignore a particular type of
   identifier. Supports wildcards and has preference over '?'. Example:

        ! std::experimental::*

3. Write an identifier on the left and a header file on the right separated by
   an equals sign (=) to assign a header. Supports wildcards. Exact matches have
   preference over wildcards. Examples:

        std::ios_base::* = <ios>
        assert = <cassert>

To run IWYU simply execute `python iwyu.py <FILE>...` and it will display a
minimal set of headers for each file. IWYU will not edit your source files.
