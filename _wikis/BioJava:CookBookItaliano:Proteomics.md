---
title: BioJava:CookBookItaliano:Proteomics
---

Come posso calcolare la massa e il pI di un aminoacido?
-------------------------------------------------------

Se si sta lavorando ad un progetto sulla proteomica è importante
conoscere quale è approssimativamente la massa e il pI di ogni gene
putativo. Biojava contiene 2 classi (MassCalc e IsoelectricPointCalc)
che fanno parte del package proteomics che calcolano questi valori.

Il programma seguente mostra un primo utilizzo di queste classi. Questo
semplice esempio utilizza entrambe le classi MassCalc e
IsoelectricPointCalc con le proprietà di default, ma entrambe hanno
delle proprietà particolare che sarebbe meglio approfondire utilizzando
le API docs.

<java> import java.io.BufferedReader; import java.io.FileOutputStream;
import java.io.FileReader; import java.io.PrintWriter;

import org.biojava.bio.BioException; import
org.biojava.bio.proteomics.IsoelectricPointCalc; import
org.biojava.bio.proteomics.MassCalc; import
org.biojava.bio.seq.ProteinTools; import org.biojava.bio.seq.RNATools;
import org.biojava.bio.seq.Sequence; import
org.biojava.bio.seq.SequenceIterator; import
org.biojava.bio.seq.io.SeqIOTools; import org.biojava.bio.symbol.Edit;
import org.biojava.bio.symbol.IllegalAlphabetException; import
org.biojava.bio.symbol.IllegalSymbolException; import
org.biojava.bio.symbol.SimpleSymbolList; import
org.biojava.bio.symbol.SymbolList; import
org.biojava.bio.symbol.SymbolPropertyTable;

/\*\*

`* Calcola la massa e il punto isoelettrico di una collezione di sequenze.`  
`*/`

public class CalcMass {

` /**`  
`  * Chiama questo metodo per sapere come utilizzare questa classe, il programma termina dopo l'esecuzioned i questo metodo.`  
`  */`  
` public static void help(){`  
`   System.out.println(`  
`       "usage: java calcMass `<file>` `<format>` `<DNA|RNA|PROTEIN>` `<out file>`");`  
`   System.exit( -1);`

` }`

` public CalcMass() {`  
` }`

` /**`  
`  * Calcola la massa dell'aminoacido in Daltons. Utilizzando la media della massa degli isotopi`  
`  * @param protein the peptide`  
`  * @throws IllegalSymbolException if ``protein`` is not a protein`  
`  * @return the mass`  
`  */`  
` public double mass(SymbolList protein)throws IllegalSymbolException{`  
`   double mass = 0.0;`  
`   MassCalc mc = new MassCalc(SymbolPropertyTable.AVG_MASS, true);`  
`   mass = mc.getMass(protein);`  
`   return mass;`  
` }`

` /**`  
`  * Calcola il punto isoelettrico assumento un libero NH e COOH`  
`  * @param protein the peptide`  
`  * @throws IllegalAlphabetException if ``protein`` is not a peptide`  
`  * @throws BioException`  
`  * @return double the PI`  
`  */`  
` public double pI(SymbolList protein)`  
`     throws IllegalAlphabetException, BioException{`

`   double pI = 0.0;`  
`   IsoelectricPointCalc ic = new IsoelectricPointCalc();`  
`   pI = ic.getPI(protein, true, true);`  
`   return pI;`  
` }`

` public static void main(String[] args) throws Exception{`  
`   if(args.length != 4)`  
`     help();`

`   BufferedReader br = null;`  
`   PrintWriter out = null;`  
`   try{`  
`     //leggo la sequenza`  
`     br = new BufferedReader(new FileReader(args[0]));`  
`     SequenceIterator seqi =`  
`         (SequenceIterator)SeqIOTools.fileToBiojava(args[1], args[2], br);`

`     out = new PrintWriter(new FileOutputStream(args[3]));`

`     //scrivo l'header`  
`     out.println("name, mass, pI, size, sequence");`

`     CalcMass calcMass = new CalcMass();`

`     while (seqi.hasNext()) {`  
`       SymbolList syms = seqi.nextSequence();`  
`       String name = null;`

`       //prendo il nome corretto dell'aminoacido`  
`       if(args[1].equalsIgnoreCase("fasta")){`  
`         name = ((Sequence) syms).getAnnotation().`  
`             getProperty("description_line").toString();`  
`       }else{`  
`         name = ((Sequence)syms).getName();`  
`       }`  
`       out.print(name+",");`

`       //Se non è una proteina abbiamo bisogno di tradurla`  
`       if(syms.getAlphabet() != ProteinTools.getAlphabet() &&`  
`          syms.getAlphabet() != ProteinTools.getTAlphabet()){`  
`         if(syms.getAlphabet() != RNATools.getRNA()){`  
`           syms = RNATools.transcribe(syms);`  
`         }`

`         //Se non è divisibile per 3 bisogna troncarla`  
`         if(syms.length() % 3 != 0){`  
`           syms = syms.subList(1, syms.length() - (syms.length() %3));`  
`         }`

`         syms = RNATools.translate(syms);`

`        /*`  
`         * La traduzione di GTG o TTG attualmente è la Metionina se`  
`         * se è il codone di start(tutte le sequenze partono con f-Met). Altrimenti`  
`         * dobbiamo modificare la sequenza.`  
`         */      `  
`         if(syms.symbolAt(1) != ProteinTools.met()){`  
`           `  
`           //SimpleSymbolLists sono editabili altri no`  
`           syms = new SimpleSymbolList(syms);`  
`           Edit e = new Edit(1, syms.getAlphabet(), ProteinTools.met());`  
`           syms.edit(e);`  
`         }`  
`       }`  
`       //Se la sequenza finisce con una * (terminazione) abbiamo bisogno di rimuovere l'*`  
`       if (syms.symbolAt(syms.length()) == ProteinTools.ter()) {`  
`         syms = syms.subList(1, syms.length()-1);`  
`       }`

`       //effettuo i calcoli`  
`       double mass = calcMass.mass(syms);`  
`       double pI = calcMass.pI(syms);`

`       //stampo i risultati per questa proteina`  
`       out.println(mass+","+pI+","+syms.length()+","+syms.seqString());`  
`     }`  
`   }`  
`   finally{ //tidy up`  
`     if(br != null){`  
`       br.close();`  
`     }`  
`     if(out != null){`  
`       out.flush();`  
`       out.close();`  
`     }`  
`   }`  
` }`

} </java>
