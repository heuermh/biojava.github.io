---
title: BioJava:Cookbook:Sequence:Edit
---

How can I Edit a Sequence?
--------------------------

Sometimes you will want to modify the order of Symbols in a SymbolList
or Sequence. For example you may wish to delete some bases, insert some
bases or overwrite some bases in a DNA Sequence. BioJava SymbolLists
have a method called edit(Edit e) that takes an Edit object and performs
that edit on the SymbolList. The Edit object takes arguments that
specify where the edit should begin, how many residues will be changed
and a SymbolList that will replace the residues.

It is worth noting that many BioJava implementations of Sequence and
SymbolList do not allow edit operations as this may invalidate
underlying Features or Annotations. The best strategy is to make a copy
of the Symbols in the Sequence or SymbolList and operate on those.

<java> import org.biojava.bio.seq.\*; import org.biojava.bio.symbol.\*;

public class EditExamples {

` public static void main(String[] args) throws Exception{`  
`   //you can't actually edit a sequence`  
`   Sequence seq = DNATools.createDNASequence("atggct", "seq");`

`   //so you need to get a copy of the Symbols in it`  
`   //using a "copy constructor"`  
`   SimpleSymbolList syms = new SimpleSymbolList(seq);`

`   //add to the end, while overwriting 0 symbols, "cc"`  
`   Edit e = new Edit(seq.length()+1, 0, DNATools.createDNA("cc"));`  
`   //apply the edit`  
`   syms.edit(e);`  
`   //should now be atggctcc`  
`   System.out.println(syms.seqString());`

`   //insert at the start, while overwriting 0 Symbols "tt"`  
`   e = new Edit(1, 0, DNATools.createDNA("tt"));`  
`   syms.edit(e);`  
`   //should now be ttatggctcc`  
`   System.out.println(syms.seqString());`

`   //insert at position 4, overwriting 0 symbols "aca"`  
`   e = new Edit(4, 0, DNATools.createDNA("aca"));`  
`   syms.edit(e);`  
`   //should now be ttaacatggctcc`  
`   System.out.println(syms.seqString());`

`   //overwrite at position 2, 3 bases with "ggg"`  
`   e = new Edit(2, 3, DNATools.createDNA("ggg"));`  
`   syms.edit(e);`  
`   //should now be tgggcatggctcc`  
`   System.out.println(syms.seqString());`

`   //delete from the start 5 bases (overwrite 5 bases with nothing)`  
`   e = new Edit(1, 5, SymbolList.EMPTY_LIST);`  
`   syms.edit(e);`  
`   //should now be atggctcc`  
`   System.out.println(syms.seqString());`

`   //now a more complex example`

`   //overwrite positions two and three with aa and then insert tt`  
`   e = new Edit(2, 2, DNATools.createDNA("aatt"));`  
`   syms.edit(e);`  
`   //should now be aaattgctcc`  
`   System.out.println(syms.seqString());`  
` }`

} </java>
