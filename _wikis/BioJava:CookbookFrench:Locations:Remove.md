---
title: BioJava:CookbookFrench:Locations:Remove
---

Comment supprimer un *Feature* d'une *Sequence*?
------------------------------------------------

Lors du traitement d'un objet *Sequence*, il est possible que vous
vouliez supprimer certains *Features*. L'exemple suivant, gracieusement
offert par Keith James, montre comment faire pour supprimer tous les
*Features* rencontrant un critère donné. Dans cet exemple, tous les
*Features* sur le brin codant sont effacés.

<java> import java.io.\*; import java.util.\*; import
org.biojava.bio.\*;

import org.biojava.bio.seq.\*; import org.biojava.bio.seq.io.\*;

public class RemoveFeatures {

`   public static void main(String [] argv) throws Exception`  
`   {`  
`       //lire un fichier EMBL`  
`       BufferedReader br = new BufferedReader(new FileReader(argv[0]));`

`       SequenceIterator seqI = SeqIOTools.readEmbl(br);`  
`       `  
`       while (seqI.hasNext())`  
`       {`  
`           Sequence seq = seqI.nextSequence();`  
`           //obtenir tous les Features sur le brin codant`  
`           FeatureHolder fh =`  
`               seq.filter(new FeatureFilter.StrandFilter(StrandedFeature.POSITIVE));`  
`           //parcourir les Features`  
`           for (Iterator i = fh.features(); i.hasNext();)`  
`           {`  
`               //et les supprimer `  
`               seq.removeFeature((Feature) i.next());`  
`           }`  
`           //pour finir, écrire la séquence éditée`  
`           SeqIOTools.writeEmbl(System.out, seq);`  
`       }`  
`   }`

} </java>
