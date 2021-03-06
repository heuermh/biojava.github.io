---
title: BioJava:CookbookFrench:Alphabets:CrossProduct
---

Comment créer un CrossProductAlphabet tel qu'un Alphabet de codons?
-------------------------------------------------------------------

Les *CrossProductAlphabets* sont le résultat de la multiplication
d'autres *Alphabets*. Les *CrossProductAlphabets* sont utilisés pour
encapsuler 2 *Symbols* ou plus à l'intérieur d'un même *Symbol* par
"produit croisé". Par exemple, une opération de multiplication par 3 de
l*'Alphabet* ADN permettra d'encapsuler un codon en un seul *Symbol*.
Vous pourriez alors compter ces *Symbols* codons avec un *Count* ou les
utiliser dans une *Distribution*.

Les *CrossProductAlphabets* sont crées par nom (si les *Alphabets* qui
les constitutent sont enregistrés avec le *AlphabetManager*) ou en
créant une liste des *Alphabets* désirés et en fabriquant l*'Alphabet* à
partir de cette liste. Les deux approches sont utilisées dans l'exemple
ci-dessous.

<java> import java.util.\*; import org.biojava.bio.seq.\*; import
org.biojava.bio.symbol.\*;

public class CrossProduct {

` public static void main(String[] args) {`

`   // créer un CrossProductAlphabet à partir d'une liste`  
`   List l = Collections.nCopies(3, DNATools.getDNA());`  
`   Alphabet codon = AlphabetManager.getCrossProductAlphabet(l);`

`   // obtenir le même Alphabet par nom`  
`   Alphabet codon2 =`  
`       AlphabetManager.generateCrossProductAlphaFromName("(DNA x DNA x DNA)");`

`   // démontrer que les deux Alphabets sont identiques`  
`   System.out.println(codon == codon2);`  
` }`

} </java>
