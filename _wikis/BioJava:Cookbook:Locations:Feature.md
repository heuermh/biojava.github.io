---
title: BioJava:Cookbook:Locations:Feature
---

How can I make a Feature?
-------------------------

In BioJava Features are a bit like an Annotation with a Location. There
are various types of Features that all implement the Feature interface.
All Feature implementations contain an inner class called 'Template'.
The Template class specifies the minimum information needed to create a
Feature. A feature is realized when the feature template is passed as an
argument to the createFeature method of an implementation of the
FeatureHolder interface.

Conveniently Sequence is a sub interface of FeatureHolder so it can hold
features. Note that a SymbolList cannot hold Features. Interestingly the
Feature interface is also a sub interface of FeatureHolder. Because of
this a Feature can hold sub features in a nested hierarchy. This allows
a 'gene' feature to hold 'exon' features and 'exon' features to hold
'snp' features etc. There is a built in safety check that will prevent a
feature holding itself.

Feature templates can be created de novo or copied from an existing
Feature. The following example shows both options.

<java> import org.biojava.bio.\*; import org.biojava.bio.seq.\*; import
org.biojava.bio.symbol.\*; import org.biojava.utils.\*;

public class MakeAFeature {

` public static void main(String[] args) {`  
`   //get the feature template for a StrandedFeature`  
`   StrandedFeature.Template templ = new StrandedFeature.Template();`

`   //fill in the template`  
`   templ.annotation = Annotation.EMPTY_ANNOTATION;`  
`   templ.location = new RangeLocation(3,6);`  
`   templ.source = "my feature";`  
`   templ.strand = StrandedFeature.POSITIVE;`  
`   templ.type = "interesting motif";`

`   try {`  
`     //the sequence the feature will go on`  
`     Sequence seq = DNATools.createDNASequence("atgcgcttaag","seq1");`  
`     System.out.println(seq.getName()+" contains "+seq.countFeatures()+" features");`

`     System.out.println("adding new feature...");`

`     //realize the feature on the Sequence and get a pointer to it so we can make another`  
`     Feature f = seq.createFeature(templ);`  
`     System.out.println(seq.getName()+" contains "+seq.countFeatures()+" features");`

`     //make an identical template to that used to make f`  
`     templ = (StrandedFeature.Template)f.makeTemplate();`  
`     //give it a different location and type`  
`     templ.location = new PointLocation(4);`  
`     templ.type = "point mutation";`

`     System.out.println("adding nested feature...");`  
`     //realize the new feature as a nested feature of f`  
`     f.createFeature(templ);`

`     //notice how the countFeatures() method only counts top level features`  
`     System.out.println(seq.getName()+" contains "+seq.countFeatures()+" features");`  
`     System.out.println(f.getSource()+" contains "+seq.countFeatures()+" features");`  
`   }`  
`   catch (Exception ex) {`  
`     ex.printStackTrace();`  
`   }`  
` }`

} </java>
