---
title: BioJava:CookBook:Core:FastaReadWrite
---

How to Read a Fasta File with Biojava3
======================================

<java> import java.io.File; import java.io.FileInputStream; import
java.util.LinkedHashMap; import java.util.Map.Entry;

import org.biojava.nbio.core.sequence.ProteinSequence; import
org.biojava.nbio.core.sequence.compound.AminoAcidCompound; import
org.biojava.nbio.core.sequence.compound.AminoAcidCompoundSet; import
org.biojava.nbio.core.sequence.io.FastaReader; import
org.biojava.nbio.core.sequence.io.FastaReaderHelper; import
org.biojava.nbio.core.sequence.io.GenericFastaHeaderParser; import
org.biojava.nbio.core.sequence.io.ProteinSequenceCreator;

public class FastaOpen {

`   public static void main(String[] args) throws Exception{`  
`       /*`  
`        * Method 1: With the FastaReaderHelper`  
`        */`  
`       //Try with the FastaReaderHelper`  
`       LinkedHashMap`<String, ProteinSequence>` a = FastaReaderHelper.readFastaProteinSequence(new File(args[0]));`  
`       //FastaReaderHelper.readFastaDNASequence for DNA sequences`  
`       `  
`       for (  Entry`<String, ProteinSequence>` entry : a.entrySet() ) {`  
`           System.out.println( entry.getValue().getOriginalHeader() + "=" + entry.getValue().getSequenceAsString() );`  
`       }`  
`       `  
`       /*`  
`        * Method 2: With the FastaReader Object `  
`        */     `  
`       //Try reading with the FastaReader`  
`       FileInputStream inStream = new FileInputStream( args[0] );`  
`       FastaReader`<ProteinSequence,AminoAcidCompound>` fastaReader = `  
`           new FastaReader`<ProteinSequence,AminoAcidCompound>`(`  
`                   inStream, `  
`                   new GenericFastaHeaderParser`<ProteinSequence,AminoAcidCompound>`(), `  
`                   new ProteinSequenceCreator(AminoAcidCompoundSet.getAminoAcidCompoundSet()));`  
`       LinkedHashMap`<String, ProteinSequence>` b = fastaReader.process();`  
`       for (  Entry`<String, ProteinSequence>` entry : b.entrySet() ) {`  
`           System.out.println( entry.getValue().getOriginalHeader() + "=" + entry.getValue().getSequenceAsString() );`  
`       }`  
`   }`

}

</java>

How to Write a Fasta File with Biojava3
=======================================

Fasta files can be written with
[FastaWriterHelper](http://www.biojava.org/docs/api/org/biojava/nbio/core/sequence/io/FastaWriterHelper.html).
The description line is determined by
GenericFastaHeaderFormat.getHeader, which first attempts to write the
OriginalHeader from the sequence, and otherwise writes the accessionID.
If neither of these properties are defined, the description will be
blank and it will not be possible to read the file into a HashMap, as
with the FastaReader and FastaReaderHelper, above. These properties can
be set with
[Sequence.setOriginalHeader](http://www.biojava.org/docs/api/org/biojava/nbio/core/sequence/template/AbstractSequence.html#setOriginalHeader(java.lang.String))
and
[Sequence.setAccession](http://www.biojava.org/docs/api/org/biojava/nbio/core/sequence/template/AbstractSequence.html#setAccession(org.biojava.nbio/core.sequence.AccessionID)).
