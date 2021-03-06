---
title: BioJava:CookBookItaliano:Translation:Single
---

Come posso tradurre un singolo codone in un singolo aminoacido?
---------------------------------------------------------------

L'esempio di traduzione precedente mostra come utilizzare RNATools per
tradurre una SymbolList RNA in una SymbolList di Proteine. Possiamo
capire meglio il funzionamento del metodo translate() se proviamo a
tradurre un singolo codone in un singolo aminoacido.

Vediamo come:

<java> import org.biojava.bio.seq.\*; import org.biojava.bio.symbol.\*;

public class SingleTranslationDemo {

` public static void main(String[] args) {`

`   //creo un alfabeto composto da codoni che sarà di un elemento`  
`   Alphabet a = AlphabetManager.alphabetForName("(RNA x RNA x RNA)");`

`   //prendo la "Tabella Standard del Codice Genetico"`  
`   TranslationTable table = RNATools.getGeneticCode(TranslationTable.UNIVERSAL);`

`   try {`  
`     //creo un codone`  
`     SymbolList codon = RNATools.createRNA("UUG");`

`     //ottengo la rappresentazione di questo codone come Simbolo`  
`     Symbol sym = a.getSymbol(codon.toList());`

`     //lo traduco in aminoacido`  
`     Symbol aminoAcid = table.translate(sym);`

`     /*`  
`      * Questo passo non è richiesto per la traduzione ma prova solamente che`  
`      * il Simbolo proveniente dall'alfabeto corretto. Altrimenti viene sollevata una eccezione.`  
`      */`  
`     ProteinTools.getTAlphabet().validate(aminoAcid);`

`     //mi aspetto che sia Leucina`  
`     System.out.println(aminoAcid.getName());`  
`   }`  
`   catch (IllegalSymbolException ex) {`  
`     ex.printStackTrace();`  
`   }`  
` }`

} </java>

NB: Questo è soltanto uno dei metodi per effettuare questa traduzione
