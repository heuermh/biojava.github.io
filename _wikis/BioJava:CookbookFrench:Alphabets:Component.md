---
title: BioJava:CookbookFrench:Alphabets:Component
---

Comment décomposer les Symbols d'un CrossProductAlphabet pour obtenir les Symbols qui les constituent?
------------------------------------------------------------------------------------------------------

Les *CrossProductAlphabets* sont utilisés pour représenter des groupes
de *Symbols* comme un *Symbol* unique. C'est une façon très pratique de
traiter, par exemple, les codons comme des *Symbols* individuels.
Cependant, il est parfois nécessaire de reconvertir ces *Symbols* pour
obtenir à nouveau les *Symbols* qui les constituent. La recette qui suit
montre comment cela peut être fait.

Les *Symbols* d'un *CrossProductAlphabet* sont des implémentations de
l'interface *AtomicSymbol*. Le préfixe 'Atomic' suggère qu'un *Symbol*
ne peut être diviser alors comment faire pour subdiviser des *Symbols*
indivisibles en leur éléments constituants? La définition complète d'un
*AtomicSymbol* est qu'il ne peut être diviser en un *Symbol* plus simple
qui fait parti du même *Alphabet*. Les composantes qui ont construit un
*AtomicSymbol* d'un *CrossProductAlphabet* ne font pas partie de cet
*Alphabet*, par conséquent la définition d' "Atomic" reste. Un codon
provient d'un *Alphabet* (ADN x ADN x ADN) alors que les composantes
d'un *Symbol* codon font parties de l'Alphabet ADN.

Cette situation contraste avec la définition d'un *BasisSymbol*. Un
*BasisSymbol* peut très bien être diviser en composantes qui font partie
du même *Alphabet*. de cette façon, un *BasisSymbol* peut être ambigüe.
Pour une discussion sur les *BasisSymbols*, cliquer ici.

<java> package biojava\_in\_anger;

import java.util.\*; import org.biojava.bio.seq.\*; import
org.biojava.bio.symbol.\*;

public class BreakingComponents {

`   public static void main(String[] args) {`  
`       `  
`       // créer l'Alphabet "codon"`  
`       List l = Collections.nCopies(3, DNATools.getDNA());`  
`       Alphabet alpha = AlphabetManager.getCrossProductAlphabet(l);`  
`       `  
`             // obtenir le premier Symbol de cet Alphabet`  
`       Iterator iter = ((FiniteAlphabet)alpha).iterator();`  
`       AtomicSymbol codon = (AtomicSymbol)iter.next();`  
`       System.out.print(codon.getName()+" is made of: ");`  
`       `  
`       // décomposer pour obtenir une liste des composantes`  
`       List symbols = codon.getSymbols();`  
`       for(int i = 0; i < symbols.size(); i++){`  
`           if(i != 0)`  
`               System.out.print(", ");`  
`           Symbol sym = (Symbol)symbols.get(i);`  
`           System.out.print(sym.getName());`  
`       }`  
`   }`

} </java>
