---
title: BioJava:CookbookFrench:Sequence:Transcribe
---

Comment transcrire une Sequence d'ADN en Sequence d'ARN?
--------------------------------------------------------

Avec BioJava, les *Sequences* et les *SymbolLists* d'ADN et d'ARN sont
faits à partir de différents *Alphabets*. Vous pouvez alors convertir
l'ADN en ARN en utilisant la méthode statique **transcribe()** de
*RNATools*.

<java> import org.biojava.bio.symbol.\*; import org.biojava.bio.seq.\*;

public class TranscribeDNAtoRNA {

` public static void main(String[] args) {`

`   try {`  
`     // créer une SymbolList d'ADN`  
`     SymbolList symL = DNATools.createDNA("atgccgaatcgtaa");`

`     // la transcrire en ARN`  
`     symL = RNATools.transcribe(symL);`

`     // pour montrer que ca fonctionne!`  
`     System.out.println(symL.seqString());`  
`   }`  
`   catch (IllegalSymbolException ex) {`  
`     // ce qui arrivera si vous essayer de faire une`  
`     // séquence d'ADN utilisant des caractères non-IUB`  
`     ex.printStackTrace();`  
`   }`  
`   catch (IllegalAlphabetException ex) {`  
`     // ce qui arrivera si vous essayer de`  
`     // transcrire une SymbolList non-ADN`  
`     ex.printStackTrace();`  
`   }`  
` }`

</java>
