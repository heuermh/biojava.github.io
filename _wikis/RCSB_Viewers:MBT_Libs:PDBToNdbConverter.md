---
title: RCSB Viewers:MBT Libs:PDBToNdbConverter
---

Notes
-----

-   `     Probably the most confusing aspect of the loading/model creation mechanism.  See John Beaver's`  
    `     notes, below.`  
    `   `
-   `     Ids stored in the model are `<em>`Ndb`</em>` ids, not Pdb.  Pdb ids are looked up.`  
    `   `
-   `     One problem is the conversion methods are quite hard to use - they return a two-element array of`  
    `     objects which have to be tested for existence and cast.  I'm currently working on providing`  
    `     simplified versions.`  
    `   `

Explanation
-----------

*Ndb* ids primarily come from .cif/.xml files, Pdb ids from .pdb files.
The identification schemes are quite different.

Thus, the requirement to map from one to the other. The
PdbToNdbConverter performs this conversion.

-   On loading XML files, the chain and residue ids are extracted in
    both Ndb and Pdb namespaces.

<!-- -->

-   On loading PDB files, the Ndb ids are set to their corresponding Pdb
    ids, thus the mapping is essentially 1:1.

The loaders create the *PdbToNdbConverter* as the last step from the
lists of names extracted. It is handed off to the *StructureMap*, which
then uses it throughout the rest of the application.

Non-protein chains present their own issues -

` From John Beaver (edited):`

  
  
Pdb and Ndb deal with one of the major legacy problems of the PDB data.

<!-- -->

  
  
The old .pdb file format has been around for a very long time. It's
simple, and it's what most people who don't use the website use. It has
several technical limitations, but the data matches the original author
submission very closely.

<!-- -->

  
  
This is a problem. Very commonly, a small molecule or DNA strand will
have the same chain ID as a protein chain, for example. This can cause
problems when the viewer is deciding where to draw ribbons and bonds.

<!-- -->

  
  
The Ndb (whose name I took from one of the Xml tags in the PDB XML
format and which may or may not be proper terminology) is a separate
namespace for chain IDs and residue IDs. It is much more highly cleaned;
you'll almost never see a small molecule or DNA chain mixed with protein
in one chain. Also, PDB residue IDs can have letters in them; NDB
residue IDs are always integers.

<!-- -->

  
  
The Ndb namespace still has data cleanliness problems, but it seems much
better overall than the Pdb namespace.

For an example of what I mean, look at the following .xml snippet.Scroll
about halfway down the file, and you'll see something like...

`
&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:atom_site id="1249"&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:group_PDB&gt;ATOM&lt;/PDBx:group_PDB&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:type_symbol&gt;C&lt;/PDBx:type_symbol&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:label_atom_id&gt;CG&lt;/PDBx:label_atom_id&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:label_alt_id xsi:nil="true" /&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:label_comp_id&gt;ARG&lt;/PDBx:label_comp_id&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:label_asym_id&gt;A&lt;/PDBx:label_asym_id&gt;             (--&gt; NDB chain ID)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:label_entity_id&gt;1&lt;/PDBx:label_entity_id&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:label_seq_id&gt;165&lt;/PDBx:label_seq_id&gt;             (--&gt; NDB residue ID)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:Cartn_x&gt;15.583&lt;/PDBx:Cartn_x&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:Cartn_y&gt;0.027&lt;/PDBx:Cartn_y&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:Cartn_z&gt;-10.746&lt;/PDBx:Cartn_z&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:occupancy&gt;1.00&lt;/PDBx:occupancy&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:B_iso_or_equiv&gt;26.76&lt;/PDBx:B_iso_or_equiv&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:auth_seq_id&gt;165&lt;/PDBx:auth_seq_id&gt;               (--&gt; PDB residue ID)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:auth_comp_id&gt;ARG&lt;/PDBx:auth_comp_id&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:auth_asym_id&gt;E&lt;/PDBx:auth_asym_id&gt;               (--&gt; PDB chain ID)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:auth_atom_id&gt;CG&lt;/PDBx:auth_atom_id&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PDBx:pdbx_PDB_model_num&gt;1&lt;/PDBx:pdbx_PDB_model_num&gt;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&lt;/PDBx:atom_site&gt;
`

  
  
Here, label\_asym\_id is the NDB chain ID and auth\_asym\_id is the PDB
chain ID. Similarly, label\_seq\_id is the NDB residue ID and
auth\_seq\_id is the PDB residue ID.

<!-- -->

  
  
To make matters worse, Phil Bourne insisted that the community prefers
to see the PDB nomenclature. This is correct, since most of the
community uses the .pdb format. Whereas the NDB nomenclature is \*much\*
more amenable to use in the internal data structures, I had to make a
large dictionary to translate NDB to PDB


