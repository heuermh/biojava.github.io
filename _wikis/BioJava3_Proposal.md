---
title: BioJava3 Proposal
---

BioJava 3 has been released
===========================

This page was used while starting the discussions for creating the new
version. BioJava 3.0 has been released on Dec. 28th 2010.

[BioJava3\_project](BioJava3_project "wikilink")

Executive Summary
-----------------

It is suggested that development stop on the existing
BioJava/BioJavaX/BioJava2 aggregation and start afresh as BioJava3.

General reasoning
-----------------

-   The existing code is disorganised, poorly commented, and hard to
    maintain due to the use of numerous different coding styles.
-   Existing documentation is poor and it would be hard to try and write
    any given the lack of code comments.
-   Unit testing is limited and hard to tack on to existing code.
-   The build scripts are out of date and the release process is hard.
-   There is demand for a number of smaller jars as opposed to one
    monolithic one.
-   We do not make use of any Java features since Java 4. Generics is
    the obvious one.
-   There is no support for changing file formats. It supports one
    version or another, but cannot handle both.
-   The only database support is for BioSQL, which uses Hibernate but
    not in a fully flexible manner (i.e. cannot connect to more than one
    db at a time).
-   It is very sequence-centric. Users have moved on. BioJava3 should
    embrace other datatypes. Most bioinformatics now deals with
    multi-dimensional feature vectors (data matrices). While one or more
    of these dimensions might be sequence there should be no need for
    everything to be tied to sequence.

Proposal
--------

-   Analyse how BioJava is being used by the community. See the
    [UsageAnalysis](UsageAnalysis "wikilink") page.
-   To start from scratch, creating a number of smaller jars as
    sub-projects within an umbrella BioJava3 project. Each jar would
    provide tools for a specific purpose. Additional jars would provide
    cross-purpose tools such as format converters or text-to-object
    interfaces. Possibly built using [Maven](http://maven.apache.org/)
    instead of [Ant](http://ant.apache.org/).
-   Although starting from scratch, much existing code could be reused
    or refactored to suit the new design.
-   We would take full advantage of [Java
    6](http://java.sun.com/javase/6/), including generics,
    (@)annotations, the built-in property change support. Everything
    would be a bean - absolutely everything.
-   We would aim to be fully [Java EE](http://java.sun.com/javaee/)
    compliant, with the majority of components fully reusable as a bean
    in any other application, just like
    [Spring](http://www.springframework.org/)'s components are.
-   We would write a [JUnit](http://junit.sourceforge.net/) test for
    every single class, writing the test first then the class
    afterwards. If other test frameworks are out there we could
    investigate these too - one suggestion is
    [TestNG](http://testng.org/doc/). We would also write documentation
    for every single class with additional full documentation for each
    separate jar.
-   We would adhere rigidly to a common coding style and heavily comment
    the code.
-   We should make it able to focus on any aspect the user requires and
    keep its efficiency, removing its dependency on everything being
    sequence-related.
-   SymbolLists and Alphabets to be rethought as these are the most
    common stumbling block.
-   Make methods parallel-aware and take advantage of this when
    possible, and provide a global variable to specify how much
    parallelisation can take place.

Data structure
--------------

-   RecordSource is an object which provides data. It can represent a
    file, a directory of files, a database, a web search engine, etc.
    etc. etc.. It has a RecordFormat which reads/writes Records to/from
    the RecordSource. It provides an iterator over Records which match a
    given RecordSearch.
-   A RecordFormat is version-specific to the format, as are the Record
    objects it produces.
-   RecordSearch defines search criteria to be applied to a RecordSource
    (or group thereof). It provides an iterator which returns all the
    combined Records from all RecordSources the RecordSearch was applied
    to. It uses RDF or something similar to map fields between different
    kinds of Records and the search parameters.
-   Record is a piece of data in any format, as a bean. It should be as
    lightweight as possible - lazyloading of all non-key data would be
    ideal. Each different kind of Record has an object structure
    suitably matched to the RecordFormat that produced it - e.g. Genbank
    Record objects should be structured internally in almost exactly the
    same way as the Genbank file. This allows minimal loss of
    information and maximum flexibility.
-   RecordConverters convert Record objects between different formats,
    e.g. Genbank Record to FASTA Record. They allow sensible defaults to
    be provided where one format does not supply enough info to satisfy
    the minimum requirements of another. Some kind of bean conversion
    system based on RDF would be suitable for this. One possible
    candidate would be [Dozer](http://dozer.sourceforge.net/).
-   A set of tools for converting flat data (e.g. sequence strings,
    taxononmy strings) into BioJava-like objects (e.g. SymbolLists,
    NCBITaxon). These BioJava-like objects could then be used for more
    advanced applications.
-   A set of tools for manipulating the BioJava-like objects.

Action plan
-----------

1.  Please modify this page and the [Talk
    page](Talk:BioJava3_Proposal "wikilink") as you see fit in order to
    flesh out details and/or make new points.
2.  Tentative Singapore meeting to get the ball rolling on the final
    design and initial coding front.

Previous work on the subject
----------------------------

1.  Michael Heuer's
    [proposal](http://www3.shore.net/~heuermh/static-alphabet-generics.tar.gz)
    for static generic symbols/symbol lists.
2.  Matthew Pocock's [BioJava2
    proposals](http://www.derkholm.net/svn/repos/bjv2).

Major problem areas
-------------------

1.  The singleton symbol model is hard to use and understand. It needs
    simplification.
2.  Strand is specified on feature and not on location. This is not
    biologically logical.
3.  Sequence and Feature objects are tightly bound - Features can't
    exist without also loading and assigning the appropriate Sequence
    object. This slows things down and uses memory.
4.  In general, most operations require a Sequence object underlying
    whatever object you are manipulating. At the time BioJava was
    designed and written, this was fine as most biologists were
    interested in sequence manipulation. Now they have moved on and are
    more interested in sequence meta-data such as features or protein
    structures or microarray experiments or phylogenetics. To enforce
    having to load the sequence for every feature in a region of
    interest before doing even basic analysis is wasteful of resources,
    and illogical. BioJava needs to lose the Sequence-centric view of
    the world.
5.  Interfaces that have already been deprecated in the 1.5 release need
    removing entirely. Many of them are heavily used within the existing
    code base, e.g. Sequence. To remove them would require a rewrite of
    almost the entire codebase anyway, and also a rewrite of most client
    code (e.g. to use RichSequence as the default replacement for
    Sequence, which would no longer exist).
6.  The code base doesn't take advantage of the possibility of threading
    for multiple CPU's. Dual core cpu's are now standard on everything.
    Quad cores are common on servers. If code is threaded the JVM can
    easily make use of these extra cores. Additionally many parts of the
    code base are currently not thread safe.
7.  Most of the code is not bean-like and therefore cannot easily be
    used in any of the modern Java EE frameworks such as Spring or
    Hibernate.
8.  Equals, compareTo and hashCode methods are inconsistent and often
    inaccurate, e.g. customised to suit a certain behaviour pattern
    (e.g. the BJX extensions assume that nulls are allowable for the
    purposes of Hibernate, whereas really they shouldn't be and
    Hibernate doesn't need them either). Changing these would change the
    behaviour of the object model particularly when it comes to
    collections and maps.
9.  Localisation causes mistranslation of strings from lower to upper
    case. For instance, in Turkish, the lower and upper case i/I do not
    match those in the English localisation. This causes protein
    sequences to be mistranslated or misrepresented. BioJava needs to be
    modified to take this into account.
10. BioSQL interaction is good but there are still issues - particularly
    to do with case conventions for naming things such as alphabets. A
    BioSQL mini-hackathon has been suggested as one way to nail down
    exactly how BioSQL should be used, right down to details like this,
    so that all projects may be able to fully interact without knowledge
    of which tool was used to write the data to BioSQL.
11. Gapped sequences and alignments need closer attention. Currently
    there are two ways - a SimpleSymbolList with '-' symbols, or a
    SimpleGappedSymbolList with proper block definitions and coordinate
    translation and access to the ungapped sequence. The MSF alignment
    parser uses the former which is counter-intuitive as programmers
    reading alignments would expect simple access to the ungapped
    sequence. There is no easy way to translate between them if you need
    the more advanced features such as coordinate translation from
    gapped to ungapped sequence. By allowing gap symbols directly in
    SimpleSymbolList, it is impossible programmatically to enforce
    whether a method accepts gapped or ungapped sequences.

Categories of Improvement
-------------------------

Initally suggested by Andreas this attempts to group the currently
recognized *issues* surrounding Biojava. See also
[UsageAnalysis](UsageAnalysis "wikilink").

### Category A

How to work with core concepts of BioJava:

-   How to get an Alphabet
-   How to make a Sequence Object from a String or make a Sequence
    Object back into a String

### Category B

Functionality; taking on concepts/practices from *Category A* and
applying them to a bioinformatics problem.

-   How to parse a Blast output
-   How to read sequences from a Fasta file
-   How to read a GenBank, SwissProt or EMBL file
-   How to generate a global or local alignment with the
    Needleman-Wunsch or the Smith-Waterman-algorithm
-   How to read a protein structure - PDB file
-   How to export a sequence to fasta
-   How to view a sequence in a gui
-   How to parse a Fasta database search output file

