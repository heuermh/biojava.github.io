---
title: BioJava:CookbookFrench:Sequence:SubSequence
---

Comment obtenir une portion d'une Sequence?
-------------------------------------------

Il est fréquent que nous voulions n'obtenir qu'une portion d'une
séquence, disons les 10 premiers résidus ou bien une section entre deux
positions. Vous pourriez également ne vouloir imprimer qu'une portion
sur un *OutputStream* comme STDOUT. Alors comment faire?

BioJava utilise un système biologique de coordonnées pour identifier la
position des résidus: la première position est la position 1 et la
dernière position de l'index est égale à la longueur de la séquence.
Noter bien la différence avec la numérotation d'un objet *String* qui
démarre à 0 et va jusqu'à (longueur-1). Si vous tentez d'accéder à une
position à l'extérieur de la région (1,longueur), vous obtiendrez une
erreur de type *IndexOutOfBoundsException*.

### Obtenir une portion de Sequence

<java>

`   SymbolList symL = null;`

`   // votre code générant une SymbolList`

`   // obtenir le premier Symbol`  
`   Symbol sym = symL.symbolAt(1);`

`   // obtenir les 3 premiers résidus`  
`   SymbolList symL2 = symL.subList(1,3);`

`   // obtenir les 3 derniers résidus`  
`   SymbolList symL3 = symL.subList(symL.length() - 3, symL.length());`

</java>

### Imprimer une partie d'une Sequence

<java>

`   // imprimer les 3 derniers résidus d'une SymbolList ou Sequence`  
`   String s = symL.subStr(symL.length() - 3, symL.length());`  
`   System.out.println(s);`

</java>

### Code complet

<java> import org.biojava.bio.seq.\*; import org.biojava.bio.symbol.\*;

public class SubSequencing {

` public static void main(String[] args) {`  
`   SymbolList symL = null;`

`   // créer une SymbolList d'ARN`  
`   try {`  
`     symL = RNATools.createRNA("auggcaccguccagauu");`  
`   }`  
`   catch (IllegalSymbolException ex) {`  
`     ex.printStackTrace();`  
`   }`

`   // obtenir le premier résidu`  
`   Symbol sym = symL.symbolAt(1);`

`   // obtenir les 3 premiers résidus`  
`   SymbolList symL2 = symL.subList(1,3);`

`   // obtenir les 3 derniers résidus`  
`   SymbolList symL3 = symL.subList(symL.length() - 3, symL.length());`

`   // imprimer les 3 derniers résidus`  
`   String s = symL.subStr(symL.length() - 3, symL.length());`  
`   System.out.println(s);`  
` }`

} </java>
