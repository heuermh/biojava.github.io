---
title: Google Summer of Code 2013
---

BioJava at GSoC Introduction
----------------------------

BioJava is applying to take part in this year's Google Summer of Code
again as part of the OBF - the Open Bioinformatics Foundation. See [this
year's GSoC page of the
OBF](http://www.open-bio.org/wiki/Google_Summer_of_Code).

If you want to propose a project, have a look at the <BioJava:Modules>
page, for areas which are currently under development. Also take a look
at the [Feature Requests](BioJava3_Feature_Requests "wikilink") page.
There are also some ideas from last year at
[Google\_Summer\_of\_Code\_2012](Google_Summer_of_Code_2012 "wikilink")

Please read the [GSoC page at the Open Bioinformatics
Foundation](http://www.open-bio.org/wiki/Google_Summer_of_Code) and the
[main Google Summer of Code page](http://code.google.com/soc) for more
details about the program.

### Mentor List

The following developers are possible Mentors for 2013:

-   Peter Troshin
-   Spencer Bliven
-   Michael Heuer
-   Andreas Prlic

### Project Proposals

------------------------------------------------------------------------

#### Port the BioJava 1 or 2 functionality to BioJava 3

As you might have noticed some functionality present in BioJava 1/2 is
missing from the BioJava 3. This is not because this functionality is
obsolete or not needed; this is because nobody had time to refactor it
to work in the BioJava 3.

So your challenge is to identify such functionality and write a proposal
where you specify

-   What functionality you are going to port
-   How you are going to do that (e.g. what needs changing)

This project is be suitable to a confident Java developer.

Please send your proposals to the BioJava dev mailing list early, so we
can discuss them in details.

P.S. Actually you are not limited to the old versions of BioJava, there
are plenty of small little known Java projects in the Bioinformatics
field which can be of interest to a wider user community. You can
integrate them as a whole or cannibalize them and extract something
useful on its own. You are more than welcome to optimize and improve the
code while porting it to BioJava as you see fit.

**Possible Mentors**

Peter Troshin 2nd mentor : not yet assigned

------------------------------------------------------------------------

#### Improve structural alignment datastructures to support topology-independent alignments

Rationale  

BioJava contains a number of algorithms for aligning protein structures.
In the most general case, an alignment consists of a mapping between
residues of two (or more) proteins. However, for historical and
performance reasons alignments are stored as linear, sorted arrays. This
makes it difficult to express cases where the order of aligned residues
differs between the two proteins. For instance, storing the following
alignment requires some creative work-arounds:

` 123456`  
` 456123`

Additionally, the class to store structural alignments (AFPChain)
contains a number of unneccessary, poorly documented, or
algorithm-specific parameters which should be removed or refactored.

Approach & Goals  

Your challenge is to propose and implement a data structure for storing
structure alignments which

-   Is flexible enough to store topology-independent alignments
-   Efficiently utilizes memory
-   Has good performance for common tasks

Difficulty and needed skills  

Moderate technical difficulty, but requires strong planning and abstract
thinking.

This project requires an understanding of basic data structures and
performance considerations. A successful proposal should consider not
only the new data structure, but also suggest a plan for integrating it
into existing methods, particularly in the biojava3-structure and
biojava3-structure-gui modules.

Possible Mentors  

Spencer Bliven 2nd mentor: not yet assigned

------------------------------------------------------------------------

#### Topology Diagrams of Protein Structures

**Rationale**

Topology diagrams are useful for visualising the arrangement and
connectivity of secondary structure elements of proteins. We are
currently not aware of an easy to use Java implementation of software
for drawing such diagrams.

**Approach & Goals**

The goal of this project would be to use the available tools from
biojava (load structures, define secondary structure) and implement a
layout algorithm that would arrange a representation of secondary
structure elements in a way so it can be easily used for various
visualisation libraries. Depending on the speed of progress a
visualisation layer could be added on top of this (e.g HTML5 vector
graphics, JPanel, etc.).

**Difficulty and needed skills**

This project requires some algorithmic knowledge for developing the
layout-algorithm for the secondary structure elements.

**Possible Mentors**

Andreas Prlic 2nd mentor: not yet assigned

------------------------------------------------------------------------

#### Sequence Variation

**Rationale**

Several similar file specifications exist for dealing with sequence
variation, including:

VCF (Variant Call Format) is a text file format used by the 1000 Genomes
project and others for representing variation against a reference
sequence.

<http://www.1000genomes.org/wiki/Analysis/Variant%20Call%20Format/vcf-variant-call-format-version-41>

The Genome Variation Format (GVF) is a text file format for describing
sequence variants at nucleotide resolution relative to a reference
genome. GVF is a type of GFF3 file with additional pragmas and
attributes specified.

<http://www.sequenceontology.org/resources/gvf_1.02.html>
<http://www.sequenceontology.org/resources/gff3.html>

Some support for these file specifications is already present in various
bioinformatics libraries (and in fact biojava3 already provides GFF3
support); it would be desireable to pull these together behind a set of
common APIs in biojava3.

**Approach & Goals**

-   Consider existing open source VCF and GVF implementations ([Genotype
    Analysis Toolkit, GATK](http://www.broadinstitute.org/gatk/),
    [VCFTools](http://vcftools.sourceforge.net/),
    [Picard](http://picard.sourceforge.net/),
    [GVF-Parser](https://github.com/srynobio/GVF-Parser), etc.)
-   Design APIs for common entities (Allele, Genotype, Haplotype, etc.)
-   Create adaptors to third party implementations or implement support
    directly in Biojava3

**Difficulty and needed skills**

Moderate difficulty.

Strength in API design, the ability to learn from existing codebases,
and experience with Java and other languages (i.e. Perl and Python) will
be necessary.

**Possible Mentors**

Michael Heuer 2nd mentor: not yet assigned

Previous Years
--------------

[Google Summer of Code 2012](Google Summer of Code 2012 "wikilink")

[Google Summer of Code 2011](Google Summer of Code 2011 "wikilink")

[Google Summer of Code 2010](Google Summer of Code 2010 "wikilink")
