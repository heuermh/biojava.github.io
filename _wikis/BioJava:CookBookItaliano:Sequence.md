---
title: BioJava:CookBookItaliano:Sequence
---

Come posso creare un oggetto Stringa a partire da una Sequenza e viceversa creare un oggetto Sequenza a partire da una Stringa?
-------------------------------------------------------------------------------------------------------------------------------

Molte volte ci imbattiamo in sequenze rappresentate da una Stringa di
caratteri come ad esempio "atgccgtggcatcgaggcatatagc". E' un metodo
conveniente per visualizzare e rappresentare in maniera sintetica un più
complesso polimero biologico. Biojava utilizza SymbolLists e Sequences
per rappresentare i polimeri biologici come Oggetti. Le Sequences
estendono le SymbolLists fornendo ulteriori metodi per memorizzare il
nome della sequenza e ogni tipo di caratteristica che potrebbe avere,
comunque basta pensare una Sequence come fosse una SymbolList.

Sia che si usi una Sequence che una SymbolList il polimero non verrà mai
memorizzato come una Stringa. Biojava differenza i residui di diversi
polimeri utilizzando oggetti di tipo Simobolo come provenienti da
Alfabeti diversi. In questa maniera è semplice dire se stiamo usando una
sequenza di DNA, di RNA o di qualcos'altro, inoltre è anche possibile
distinguere se un residuo 'A' appartiene a un DNA o ad un RNA. I
dettagli a riguardo di Symbols, SymbolLists e Alphabets sono trattati
qui. Questa è una parte cruciale perchè è la necessita di fornire al
programmatore un strada per effettuare una conversione fra un oggetto
Biojava a una Stringa semplice da leggere e da utilizzare, e viceversa.
Per fare questo Biojava usa dei Tokenizers che leggono una stringa di
testo e la interpretano per poi ottenere un oggetto BioJava Sequence o
SymbolList. Nel caso di DNA, RNA, e Protein è possibile fare questo
facendo una singola chiamata a un metodo. La chiamata è fatta su un
metodo statico dalle classi DNATools, RNATools or ProteinTools.

### Da String a SymbolList

<java> import org.biojava.bio.seq.\*; import org.biojava.bio.symbol.\*;

public class StringToSymbolList {

` public static void main(String[] args) {`  
`  `  
`   try {`  
`     //creo una DNA SymbolList a partire da una String`  
`     SymbolList dna = DNATools.createDNA("atcggtcggctta");`

`     //creo una RNA SymbolList a partire da una String`  
`     SymbolList rna = RNATools.createRNA("auugccuacauaggc");`

`     //creo una Protein SymbolList a partire da una String`  
`     SymbolList aa = ProteinTools.createProtein("AGFAVENDSA");`  
`   }`  
`   catch (IllegalSymbolException ex) {`  
`      //questa eccezione viene sollevata se viene utilizzato all'interno di una stringa`  
`      //un simbolo che non è previsto dallo IUB`  
`     ex.printStackTrace();`  
`   }`  
`  `  
` }`

} </java>

### Da String a Sequence

<java> import org.biojava.bio.seq.\*; import org.biojava.bio.symbol.\*;

public class StringToSequence {

` public static void main(String[] args) {`

`   try {`  
`     //creo una DNA sequence con nome dna_1`  
`     Sequence dna = DNATools.createDNASequence("atgctg", "dna_1");`

`     //creo una RNA sequence con nome  rna_1`  
`     Sequence rna = RNATools.createRNASequence("augcug", "rna_1");`

`     //creo una Protein sequence con nome  prot_1`  
`     Sequence prot = ProteinTools.createProteinSequence("AFHS", "prot_1");`  
`   }`  
`   catch (IllegalSymbolException ex) {`  
`     //viene sollevata una eccezione se non vengono utilizzati i Simboli previsti dallo IUB`  
`     ex.printStackTrace();`  
`   }`  
` }`

} </java>

### Da SymbolList a String

E' possibile chiamare il metodo seqString() sia sulla classe SymbolList
o su Sequence per ottenere una versione in forma di stringa.

<java> import org.biojava.bio.symbol.\*;

public class SymbolListToString {

` public static void main(String[] args) {`  
`   SymbolList sl = null;`  
`   //qui va il codice per istanziare una SymbolList`  
`  `  
`   //converto la SymbolList in una String`  
`   String s = sl.seqString();`  
` }`

} </java>
