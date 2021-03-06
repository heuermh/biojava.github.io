---
title: BioJava:Tutorial:Blast2HTML
---

by **[Cambridge Antibody
Technology](mailto:bioinformatics@CambridgeAntibody.com)**

Introduction
------------

This tutorial covers the use of the Blast-like parsing framework to
generate HTML representations of the Blast-like XML.

Here are some examples of the type of output you can generate.

-   [Blastp](http://www.biojava.org/tutorials/blastlikeParsingCookBook/blastp.html)
-   [Blastn](http://www.biojava.org/tutorials/blastlikeParsingCookBook/blastp.html)

Prerequisites are:

-   an upto date copy of biojava
-   the programs in the demos directory

Running the demos
-----------------

To generate for yourself the above example HTML files, change directory
to the demos directory of biojava. The following commands will generate
the HTML to standard out:

    java eventbasedparsing.Blast2HTML nucleic files/ncbiblast/blastn.out
    java eventbasedparsing.Blast2HTML protein files/ncbiblast/blastp.out

You can choose an output file (instead of redirecting standard out) by
adding a third argument to the command:

    java eventbasedparsing.Blast2HTML protein files/ncbiblast/blastp.out blastp.html

Customising the Output
----------------------

The `HTMLRenderer` constructor takes several parameters which allow
customisation of the HTML.

-   **Style sheet**: You can change the definition of the styles in the
    style sheet.
-   **Alignment width**: The alignment width simply specifies the number
    of bases/residues per alignment block.
-   **URLGeneratorFactory**: Returns a `List` of
    `DatabaseURLGenerators`. These are used to convert database ID's to
    URL's and links. You can create your own. See
    NcbiDatabaseURLGenerator for an example.
-   **AlignmentMarker**: Delegates most of it's operations to the
    `ColourCommand` and `AlignmentStyler`.
    -   `ColourCommand`: Controls whether a pair of characters in the
        alignment are styled or not.
    -   `AlignmentStyler`: Decides what style to apply to any given pair
        of characters.

E.g. To markup mismatches in red you would have a `ColourCommand` that
decides only mismatches are coloured, and then an `AlignmentStyler` that
colours any characters passed to it as red.

There are a couple of implementations of `AlignmentStyler`:
`SimpleAlignmentStyler` and `BlastMatrixAlignmentStyler` - see the
Javadocs for details.

Of course you can also use custom handlers to only pass on a subset of
the output.

<Category:Tutorial>
