---
title: BioJava:CookBook:PDB:ligands
---

<java>

`        //get a structure`  
`        Structure struct = structure;`  
`        //get the non-water HETATOM groups in the structure`  
`        List`<Group>` hets = struct.getHetGroups();`  
`           for (Group group : hets) {`  
`               System.out.println(group);`  
`               //for every Group in the list find all other groups in the structure within 4.00 Angstrom, not including waters`  
`               List`<Group>` fourAngstromShell = StructureTools.getGroupsWithinShell(struct, group, 4.00, false);`  
`               System.out.println("Groups within 4.00 Angstroms of " + group + ":");`  
`               for (Group fourAngstromgroup : fourAngstromShell) {`  
`                   System.out.println(fourAngstromgroup);`  
`                       `  
`               }`  
`               //find the inter-molecular bonds between a group and the surrounding groups `  
`               for (Bond bond : StructureTools.findBonds(group, fourAngstromShell)) {`  
`                       System.out.println(bond);`  
`               }`  
`           }`

</java>

*n.b.* StructureTools.findBonds() is currently under development and
will **not** give chemically correct answers. However, it will give very
quick and dirty approximations based on distances.
