---
title: BioJava:CookbookFrench:Locations:Circular
---

Comment fonctionne les CircularLocations?
-----------------------------------------

Certains types de molécules d'ADN, comme les plasmides et les
chromosomes bactériens, sont circulaires. Les positions sur une molécule
circulaire sont donc spécifiées relativement à une origine fixée de
façon arbitraire.

Dans BioJava, les *SymbolLists* circulaires n'existent pas. Les
*Symbols* sous-jacents sont stockés ultimement comme un tableau de
pointeurs vers des *Symbols*. Un effet de cicularité peut être simuler
en utilisant un objet *CircularView* (qui implemente *SymbolListView*).

Dans une *SymbolList*, il est impossible d'accéder à un *Symbol* en
utilisant une *Location* qui se trouve à l'extérieur de la *SymbolList*.
Essayer d'obtenir le *Symbol* à la position 0 ou length+1 lancera une
exception de type *IndexOutOfBounds*. Dnas le cas d'une *CircularView*,
il est tout a fait possible de rechercher le *Symbol* à 0 ou -5 et de
s'attendre à obtenir ce *Symbol*. Parce que BioJava utilise un système
de coordonnées biologique, une *Sequence* se numérote de 1 à length.

Il n'y a pas de limite sur l'indexage d'une *CircularView* et une
convention particulière est utilisée pour la numérotation. Le *Symbol* à
l'index 1 est le premier *Symbol* de la *SymbolList* sous-jacente. Le
*Symbol* à l'index 0 est la base précédent immédiatement le *Symbol* 1
et, dans ce cas, est aussi la dernière base de la *SymbolList*
sous-jacente.

La classe *CircularLocation* s'occupe des objets *CircularLocations*. La
meilleure façon de créer des *CircularLocations* est de les contruire
avec la classe *LocationTools*. L'exmple ci-dessous montre comment
faire.

**Note:** La recette suivante ne fonctionne bien qu'avec des versions
récentes de BioJava, 1.3 et plus.

<java> import org.biojava.bio.seq.\*; import org.biojava.bio.symbol.\*;

public class SpecifyCircular {

` public static void main(String[] args) {`  
`   try {`  
`     Location[] locs = new Location[3];`  
`     //créer une CircularLocation spécifiant les positions 3-8 d'un 20mer`  
`     locs[0] = LocationTools.makeCircularLocation(3,8,20);`  
`     //créer une CircularLocation spécifiant les positions 0-4 d'un 20mer`  
`     locs[1] = LocationTools.makeCircularLocation(0,4,20);`  
`     //créer une CircularLocation spécifiant les positions 18-24 d'un 20mer`  
`     locs[2] = LocationTools.makeCircularLocation(18,24,20);`

`     for (int i = 0; i < locs.length; i++){`  
`       //imprimer la position`  
`       System.out.println("Location: "+locs[i].toString());`

`       //créer une SymbolList`  
`       SymbolList sl = DNATools.createDNA("gcagctaggcggaaggagct");`  
`       System.out.println("SymbolList: "+sl.seqString());`

`       //obtenir la SymbolList spécifiée par la CircularLocation`  
`       SymbolList sym = locs[i].symbols(sl);`  
`       System.out.println("Symbol specified by Location: "+sym.seqString());`  
`     }`  
`   }`  
`   catch (IllegalSymbolException ex) {`  
`     //si on utilise un Symbol illégal pour créer sl`  
`     ex.printStackTrace();`  
`   }`  
` }`

} </java>
