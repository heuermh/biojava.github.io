---
title: BioJava:CookBook:PDB:SCOP
---

Parsing SCOP with BioJava
=========================

The BioJava SCOP parser can

-   automatically download the SCOP release files (if they are not at a
    specified local directory)
-   parse the SCOP files
-   provides an API to access any level of the SCOP classification.

If you are running out of memory while running any of the examples,
increase memory by adding the VM argument:

`-Xmx500M`

Print all SCOP domains for a protein structure
----------------------------------------------

<java> public void printDomainsForPDB(){

`     String cacheLocation = "/tmp/";`  
`     String pdbId = "4HHB";`  
`     `  
`     // download SCOP if required and load into memory`  
`     ScopInstallation scop = new ScopInstallation(cacheLocation);`

`     List`<ScopDomain>` domains = scop.getDomainsForPDB(pdbId);`

`     System.out.println(domains);`

} </java>

prints:

    [d4hhba_    4hhb    A:  a.1.1.2 15251   cl=46456,cf=46457,sf=46458,fa=46463,dm=46486,sp=46487,px=15251, 
    d4hhbc_ 4hhb    C:  a.1.1.2 15252   cl=46456,cf=46457,sf=46458,fa=46463,dm=46486,sp=46487,px=15252, 
    d4hhbb_ 4hhb    B:  a.1.1.2 15428   cl=46456,cf=46457,sf=46458,fa=46463,dm=46500,sp=46501,px=15428, 
    d4hhbd_ 4hhb    D:  a.1.1.2 15429   cl=46456,cf=46457,sf=46458,fa=46463,dm=46500,sp=46501,px=15429]

Traverse the SCOP hierarchy
---------------------------

This examples loads a domain and traverses through its hierarchy in
SCOP.

<java> private void traverseHierarchy()

`  {`  
`     String cacheLocation = "/tmp/";`  
`     String pdbId = "4HHB";`  
`     // download SCOP if required and load into memory`  
`     ScopInstallation scop = new ScopInstallation(cacheLocation);`  
`     `  
`     List`<ScopDomain>` domains = scop.getDomainsForPDB(pdbId);`  
`     `  
`     // show the hierachy for the first domain:`  
`     `  
`     ScopNode node = scop.getScopNode(domains.get(0).getSunid());`  
`     `  
`     while (node != null){`  
`        `  
`        System.out.println("This node: sunid:" + node.getSunid() );`  
`        System.out.println(scop.getScopDescriptionBySunid(node.getSunid()));`  
`        node = scop.getScopNode(node.getParentSunid());`  
`     }`  
`     `

} </java>

Produces this output:

    parsed 110800 scop sunid domains.
    parsed 143429 scop sunid nodes.
    This node: sunid:15251
    parsed 143428 scop sunid descriptions.
    15251   px  a.1.1.2 d4hhba_ 4hhb A:
    This node: sunid:46487
    46487   sp  a.1.1.2 -   Human (Homo sapiens) [TaxId: 9606]
    This node: sunid:46486
    46486   dm  a.1.1.2 -   Hemoglobin, alpha-chain
    This node: sunid:46463
    46463   fa  a.1.1.2 -   Globins
    This node: sunid:46458
    46458   sf  a.1.1   -   Globin-like
    This node: sunid:46457
    46457   cf  a.1 -   Globin-like
    This node: sunid:46456
    46456   cl  a   -   All alpha proteins
    This node: sunid:0
    null

Print various SCOP categories
-----------------------------

<java> public void getCategories(){

`     String cacheLocation = "/tmp/";`  
`     `  
`     // download SCOP if required and load into memory`  
`     ScopInstallation scop = new ScopInstallation(cacheLocation);`  
`     List`<ScopDescription>` superfams = scop.getByCategory(ScopCategory.Superfamily);`

`     System.out.println("Total nr. of superfamilies:" + superfams.size());`  
`     `  
`     List`<ScopDescription>` folds = scop.getByCategory(ScopCategory.Fold);`  
`     System.out.println("Total nr. of folds:" + folds.size());  `  
`}`

</java>

prints

    Total nr. of superfamilies:2223
    Total nr. of folds:1393

Load a SCOP superfamily and align the first domain against all others
---------------------------------------------------------------------

This example loads a superfamily from SCOP and aligns the first domain
in this family against all others.

<java>

`public void alignSuperfamily(){`  
`     `  
`     String cacheLocation = "/tmp/";`  
`    `  
`     // download SCOP if required and load into memory`  
`     ScopInstallation scop = new ScopInstallation(cacheLocation);`  
`     List`<ScopDescription>` superfams = scop.getByCategory(ScopCategory.Superfamily);`

`     System.out.println("Total nr. of superfamilies:" + superfams.size());`

`     // configure where to load PDB files from and `  
`     // what information to load`  
`     AtomCache cache = new AtomCache(cacheLocation, true);      `  
`     FileParsingParameters fileparams = new FileParsingParameters() ;`  
`     fileparams.setAlignSeqRes(false);`  
`     fileparams.setLoadChemCompInfo(true);`  
`     fileparams.setParseSecStruc(false);`  
`     cache.setFileParsingParams(fileparams);`  
`     `  
`     // get the first superfamily`  
`     ScopDescription superfam1 = superfams.get(0);`  
`     System.out.println("First superfamily: " + superfam1);`  
`     `

`     // ScopNodes allow to traverse the SCOP hierarchy      `  
`     ScopNode node = scop.getScopNode(superfam1.getSunID());`  
`     System.out.println("scopNode for first superfamily:" + node);`  
`     `  
`     List`<ScopDomain>` doms4superfam1 = scop.getScopDomainsBySunid(superfam1.getSunID());`  
`     ScopDomain dom1 = doms4superfam1.get(0);`  
`     `  
`     // align the first domain against all others members of this superfamily`  
`     for ( int i = 1 ; i < doms4superfam1.size() ; i ++){`

`        ScopDomain dom2 = doms4superfam1.get(i);`  
`       `  
`        try {`  
`           Structure s1 = cache.getStructureForDomain(dom1);`  
`           Structure s2 = cache.getStructureForDomain(dom2);`  
`           `  
`           Atom[] ca1 = StructureTools.getAtomCAArray(s1);`  
`           Atom[] ca2 = StructureTools.getAtomCAArray(s2);`  
`           StructureAlignment ce = StructureAlignmentFactory.getAlgorithm(CeMain.algorithmName);`  
`           AFPChain afpChain = ce.align(ca1, ca2);`  
`           `  
`           //System.out.println(afpChain.toCE(ca1, ca2));`  
`           `  
`           //StructureAlignmentDisplay.display(afpChain, ca1, ca2);`  
`           `  
`           System.out.println(dom1.getScopId() + " vs. " + dom2.getScopId()+ " :" + afpChain.getProbability());`  
`           `  
`        } catch (Exception e){`  
`           e.printStackTrace();`  
`        }`  
`     }`

} </java>
