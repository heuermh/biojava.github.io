---
title: BioJava:CookBook:PDB:aboutalign
---

About the Structure alignment algorithm
=======================================

The structure alignment algorithm contained in BioJava is based on a
variation of the PSC++ algorithm provided by Peter Lackner, Univ.
Salzburg (personal communication). The algorithm is calculating a
distance matrix based, rigid body protein structure superimposition. See
also a JavaWebStart example of how it works at <BioJava:Performance>.

What can it do?
---------------

calculate alignments of

`* whole structures, `  
`* single chains`  
`* any set of atoms`  
`* it provides alternative solutions`  
`* alternative solutions are clustered to groups of similar alignments`

How does it work?
-----------------

[View the source
code](http://code.open-bio.org/svnweb/index.cgi/biojava/view/biojava-live/trunk/src/org/biojava/bio/structure/align/StructurePairAligner.java)

`* It identifies short fragments in two protein structures that have similar intra-molecular distances. `  
`* The pairs of fragments are then compared and if possible, joined to longer fragments.`  
`* Finally the fragments are undergoing a refinement procedure in order to extend them to full size alignments.`

[Back to the CookBook](BioJava:CookBook:PDB:align "wikilink")
