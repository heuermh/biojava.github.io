---
title: BioJava:CookbookFrench:Translation:SixFrame
---

Comment faire pour traduire une Sequence dans ses six cadres de lectures?
-------------------------------------------------------------------------

Cette tache est probablement une des plus communes de la
bio-informatique et une des questions les plus souvent posées sur la
liste de courriels.

La traduction des six cadres de lecture est efficace pour identifier des
grands ORFs pouvant contenir des régions codantes, du moins dans les
espèces n'ayant pas d'introns. Une traduction dans les six cadres se
fait simplement en prenant des sous-séquences de la séquence d'intérêt
pour en faire la complémentation inverse et la traduction. Le seul
détail important est comment faire pour sélectionner les sous-séquences
pour qu'elles soient également divisibles par trois.

L'exemple suivant montre un simpe programme qui traduira les six cadres
de lecture de toutes les séquences contenues dans un fichier pour en
imprimer les résultats sur la console en format FASTA.

<java> import java.io.BufferedReader; import java.io.FileReader;

import org.biojava.bio.Annotation; import org.biojava.bio.seq.DNATools;
import org.biojava.bio.seq.RNATools; import
org.biojava.bio.seq.Sequence; import
org.biojava.bio.seq.SequenceIterator; import
org.biojava.bio.seq.SequenceTools; import
org.biojava.bio.seq.io.SeqIOTools; import
org.biojava.bio.symbol.SymbolList;

/\*\*

`* Programme pour traduire les six cadres de lecture`  
`* d'une séquence de nucléotides `  
`*/`

public class Hex {

` /**`  
`  * Méthode appellée pour info sur l'utilisation`  
`  * Le programme se termine après son appel.`  
`  */`  
` public static void help() {`  
`   System.out.println(`  
`       "usage: java Hex `<file>` `<format eg fasta>` `<dna|rna>`");`  
`   System.exit( -1);`  
` }`

` public static void main(String[] args) throws Exception{`  
`   if (args.length != 3) {`  
`     help();`  
`   }`

`   BufferedReader br = null;`  
`   // format du fichier  (par ex.: fasta)`  
`   String format = args[1];`  
`   // type de séquence  (par ex.: dna)`  
`   String alpha = args[2];`

`   try {`  
`     br = new BufferedReader(new FileReader(args[0]));`

`     SequenceIterator seqi =`  
`         (SequenceIterator)SeqIOTools.fileToBiojava(format, alpha, br);`

`    // pour chaque séquence`  
`    while(seqi.hasNext()){`  
`       Sequence seq = seqi.nextSequence();`

`       // pour chaque cadre`  
`       for (int i = 0; i < 3; i++) {`  
`         SymbolList prot;`  
`         Sequence trans;`

`        // prenez le cadre de lecture`  
`         SymbolList syms = seq.subList(`  
`               i+1,`  
`               seq.length() - (seq.length() - i)%3);`

`         // si la séquence est d'ADN, transcription en ARN`  
`         if(syms.getAlphabet() == DNATools.getDNA()){`  
`           syms = RNATools.transcribe(syms);`  
`         }`

`        // sortir la traduction des cadres avant sur STDOUT`  
`         prot = RNATools.translate(syms);`  
`         trans = SequenceTools.createSequence(prot, "",`  
`                                              seq.getName()+`  
`                                              "TranslationFrame: +"+i,`  
`                                              Annotation.EMPTY_ANNOTATION);`  
`         SeqIOTools.writeFasta(System.out, trans);`

`        // sortir la traduction des cadres inverses sur STDOUT`  
`         syms = RNATools.reverseComplement(syms);`  
`         prot = RNATools.translate(syms);`  
`         trans = SequenceTools.createSequence(prot, "",`  
`                                              seq.getName() +`  
`                                              " TranslationFrame: -" + i,`  
`                                              Annotation.EMPTY_ANNOTATION);`  
`         SeqIOTools.writeFasta(System.out, trans);`  
`       }`  
`     }`  
`   }`  
`   finally {`  
`     // pour finir`  
`     if(br != null){`  
`       br.close();`  
`     }`  
`   }`  
` }`

} </java>
