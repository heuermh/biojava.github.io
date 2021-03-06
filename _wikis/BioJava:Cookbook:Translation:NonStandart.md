---
title: BioJava:Cookbook:Translation:NonStandart
---

How do I use a non standard translation table?
----------------------------------------------

The convenient translate() method in RNATools, used in the general
translation example, is only useful if you want to use the "Universal"
translation table. This is not so good if you want to use one of those
weird Mitochondrial translation tables. Fortunately this can be done in
BioJava. RNATools also has a static method getGeneticCode(String name)
that lets you get a TranslationTable by name.

The following TranslationTables are available:

-   FLATWORM\_MITOCHONDRIAL
-   YEAST\_MITOCHONDRIAL
-   ASCIDIAN\_MITOCHONDRIAL
-   EUPLOTID\_NUCLEAR
-   UNIVERSAL
-   INVERTEBRATE\_MITOCHONDRIAL
-   BLEPHARISMA\_MACRONUCLEAR
-   ALTERNATIVE\_YEAST\_NUCLEAR
-   BACTERIAL
-   VERTEBRATE\_MITOCHONDRIAL
-   CILIATE\_NUCLEAR
-   MOLD\_MITOCHONDRIAL
-   ECHINODERM\_MITOCHONDRIAL

These are also the valid names that can be used as an argument in the
static RNATools.getGeneticCode(String name) method. These names are also
available as static Strings in the TranslationTools class.

The following program shows the use of the Euplotid Nuclear translation
table (where UGA = Cys).

<java> import org.biojava.bio.seq.\*; import org.biojava.bio.symbol.\*;

public class AlternateTranslation {

` public static void main(String[] args) {`

`   //get the Euplotoid translation table`  
`   TranslationTable eup = RNATools.getGeneticCode(TranslationTable.EUPL_NUC);`

`   try {`  
`     //make a DNA sequence including the 'tga' codon`  
`     SymbolList seq = DNATools.createDNA("atgggcccatgaaaaggcttggagtaa");`

`     //transcribe to RNA`  
`     seq = RNATools.transcribe(seq);`

`     //view the RNA sequence as codons, this is done internally by RNATool.translate()`  
`     seq = SymbolListViews.windowedSymbolList(seq, 3);`

`     //translate`  
`     SymbolList protein = SymbolListViews.translate(seq, eup);`

`     //print out the protein`  
`     System.out.println(protein.seqString());`  
`   }`  
`   catch (Exception ex) {`  
`     ex.printStackTrace();`  
`   }`  
` }`

} </java>
