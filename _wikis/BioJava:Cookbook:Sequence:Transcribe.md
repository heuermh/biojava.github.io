---
title: BioJava:Cookbook:Sequence:Transcribe
---

How do I Transcribe a DNA Sequence to an RNA Sequence?
------------------------------------------------------

In BioJava DNA and RNA Sequences and SymbolLists are made using
different Alphabets you can convert from DNA to RNA using the static
method transcribe() in RNATools.

<java> import org.biojava.bio.symbol.\*; import org.biojava.bio.seq.\*;

public class TranscribeDNAtoRNA {

`  public static void main(String[] args) {`

`     try {`  
`      //make a DNA SymbolList`  
`      SymbolList symL = DNATools.createDNA("atgccgaatcgtaa");`

`      //transcribe it to RNA (after BioJava 1.4 this method is deprecated)`  
`      symL = RNATools.transcribe(symL);`

`      //(after BioJava 1.4 use this method instead)`  
`      symL = DNATools.toRNA(symL);`  
`      `  
`      //just to prove it worked`  
`      System.out.println(symL.seqString());`  
`     }`  
`     catch (IllegalSymbolException ex) {`  
`       //this will happen if you try and make the DNA seq using non IUB symbols`  
`        ex.printStackTrace();`  
`     }catch (IllegalAlphabetException ex) {`  
`      //this will happen if you try and transcribe a non DNA SymbolList`  
`        ex.printStackTrace();`  
`     }`  
`  }`

} </java>
