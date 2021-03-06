---
title: BioJava:CookbookFrench:PDB:Header
---

Comment accéder aux informations contenues dans l'en-tête d'un fichier PDB?
---------------------------------------------------------------------------

Avec la version 1.6 de BioJava, il est maintenant possible de lire et
d'extraire les informations, souvent fort utiles , contenues dans
l'en-tête d'un fichier PDB; merci à Jules Jacobsen (EBI) pour le code
permettant ces opérations. Les informations sont contenues dans un objet
de type Compound, accessible via la méthode *getCompounds()* de la
classe Structure.

La recette suivante vous permet d'y accéder.

<java> public static void main(String[] args){

`       String pdbCode =  "1aoi";`

`       PDBFileReader pdbreader = new PDBFileReader();`  
`       pdbreader.setPath("/Path/To/PDBFiles/");`  
`       pdbreader.setParseSecStruc(true);`  
`       pdbreader.setAlignSeqRes(true);`  
`       pdbreader.setAutoFetch(true);`

`       try{`  
`           Structure struc = pdbreader.getStructureById(pdbCode);`  
`           Map`<String,Object>` m = struc.getHeader();`

`           Set`<String>` keys = m.keySet();`  
`           for (String key: keys) {`  
`               System.out.println(key +": " +  m.get(key));`  
`           }`

`           System.out.println("available compounds:");`  
`           List`<Compound>` compounds = struc.getCompounds();`  
`           for (Compound compound:compounds){`  
`               System.out.println(compound);`  
`           }`  
`           `

`       } catch (Exception e) {`  
`           e.printStackTrace();`  
`       }`  
`   }`

</java>

fournira la sortie suivante:

    title: COMPLEX BETWEEN NUCLEOSOME CORE PARTICLE (H3,H4,H2A,H2B) AND 146 BP LONG DNA FRAGMENT 
    technique: X-RAY DIFFRACTION 
    classification: DNA BINDING PROTEIN/DNA
    depDate: 03-JUL-97
    modDate: 01-APR-03
    idCode: 1AOI
    resolution: 2.8
    available compounds:
    Compound: 1 HISTONE H3 Chains: ChainId: A E Engineered: YES OrganismScientific: XENOPUS LAEVIS OrganismCommon: AFRICAN CLAWED FROG ExpressionSystem: ESCHERICHIA COLI Fragment: HISTONE H3 
    Compound: 2 HISTONE H4 Chains: ChainId: B F Engineered: YES OrganismScientific: XENOPUS LAEVIS OrganismCommon: AFRICAN CLAWED FROG ExpressionSystem: ESCHERICHIA COLI ExpressionSystemOtherDetails: SYNTHETIC GENE, OPTIMIZED CODON USAGE FOR Fragment: HISTONE H4 
    Compound: 3 HISTONE H2A Chains: ChainId: C G Engineered: YES OrganismScientific: XENOPUS LAEVIS OrganismCommon: AFRICAN CLAWED FROG ExpressionSystem: ESCHERICHIA COLI Fragment: HISTONE H2A 
    Compound: 4 HISTONE H2B Chains: ChainId: D H Engineered: YES Mutation: YES OrganismScientific: XENOPUS LAEVIS OrganismCommon: AFRICAN CLAWED FROG ExpressionSystem: ESCHERICHIA COLI Fragment: HISTONE H2B 
    Compound: 5 PALINDROMIC 146 BP DNA REPEAT 8/9 FROM HUMAN X- CHROMOSOME ALPHA SATELLITE DNA Chains: ChainId: I J Engineered: YES Synthetic: YES 
