---
title: BioJava:CookbookFrench:Services:Qblast
---

Comment aligner une séquence en utilisant le service QBlast?
------------------------------------------------------------

Biojava possède maintenant dans le code de développement les outils de
base pour exécuter certaines tâches sur des serveurs distants et en
récupérer les résultats. Le premier exemple est la possibilité de faire
des analyses avec Blast en utilisant le service QBlast du NCBI. Quoique
n'étant pas strictement un service web dans le sens pur du terme, QBlast
utilise des requêtes HTTP spécialement formattées pour exécuter des
recherches Blast sur les serveurs du NCBI.

Les classe QBlast de BioJava implémentent une suite d'interfaces:
`RemotePairwiseAlignmentService`, `RemotePairwiseAlignmentProperties` et
`RemotePairwiseAlignmentOutputProperties`. Ces interfaces existent afin
de séparer la spécification des paramètres d'alignement, la soumission
des requetes et la récupération des résultats. Ceci permet à un
programme d'utiliser plus d'une série de paramétres pour aligner une
séquence et la récupération selon divers formats à un moment ultérieur.

Pour utiliser le service QBlast via BioJava, vous créer un objet de la
classe RemoteQBlastService (qui implémente
RemotePairwiseAlignmentService) qui sera responsable de la gestion de la
connection et des requêtes. Pour soumettre une requête avec une séquence
(que ce soit un objet de classe RichSequence, un chaine de caractères ou
un GID), vous devez accompagnez chaque séquence d'un objet de la classe
RemoteQBlastAlignmentProperties (qui implémente
RemotePairwiseAlignmentProperties) qui sert à spécificer le programme à
utiliser et la base de données de séquences à employer. Après la
soumission d'un ou plusieurs séquences, chacune avec un ou plusieurs
objets de la classe RemoteQBlastServiceProperties, vous récupérez les
résultats en utilisant l'identificateur de requête et un objet de la
classe RemoteQBlastOutputProperties pour les obtenir selon le format
souhaité. Le résultat est contenu dans un `InputStream` qui peut servir
autant à imprimer à l'écran qu'à écrire dans un fichier.

Les interfaces du package org.biojavax.bio.alignment devraient permettre
à n'importe qui de développer un service d'alignement, utilisant par
exemple FASTA ou Blast à l'EBI, qui eux utilisent des services web.

**AVERTISSEMENTS (en date de Juillet 2009):**

- Seulement les programmes de blastall sont fonctionnels. MegaBlast et
blastpgp sont des priorités.

- N'utilisez pas les threads pour envoyer une avalanche de requetes au
NCBI. Cela ne vous apporterais que des ennuis pouvant aller jusqu'à la
radiation de votre adresse IP par le NCBI.

<java> import java.io.BufferedReader; import java.io.FileReader; import
java.io.IOException; import java.io.InputStream; import
java.io.InputStreamReader; import java.util.ArrayList; import
java.util.Set;

import org.biojava.bio.BioException; import
org.biojavax.SimpleNamespace; import org.biojavax.bio.seq.RichSequence;
import org.biojavax.bio.seq.RichSequenceIterator;

import org.biojavax.bio.alignment.blast.RemoteQBlastService; import
org.biojavax.bio.alignment.blast.RemoteQBlastAlignmentProperties; import
org.biojavax.bio.alignment.blast.RemoteQBlastOutputProperties; import
org.biojavax.bio.alignment.blast.RemoteQBlastOutputFormat;

public class RemoteQBlastServiceTest {

`   /**`  
`    * Le programme prend en parametre le path vers un fichier de sequence`  
`    * `  
`    * Pour l'exemple, utilisons une sequence FASTA`  
`    * `  
`    */`  
`   public static void main(String[] args) {`

`       RemoteQBlastService rbw;`  
`       RemoteQBlastOutputProperties rof;`  
`       InputStream is;`  
`       ArrayList`<String>` rid = new ArrayList`<String>`();`  
`       String request = "";`

`       try {`  
`           rbw = new RemoteQBlastService();`  
`           SimpleNamespace ns = new SimpleNamespace("bj_blast");`  
`           RichSequenceIterator rs = RichSequence.IOTools.readFastaDNA(`  
`                   new BufferedReader(new FileReader(args[0])), ns);`

`           /*`  
`                        * On peut imaginer que nous utiliserions un meme ensemble de parametres`  
`                        * pour blaster une serie de sequences...`  
`                        *`  
`                        * Vous pourriez par exemple changer la pénalité pour l'ouverture/extension`  
`                        * des gaps (à venir...)`  
`            */`  
`           RemoteQBlastAlignmentProperties rqb = new RemoteQBlastAlignmentProperties();`  
`           rqb.setBlastProgram("blastn");`  
`           rqb.setBlastDatabase("nr");`

`           /*`  
`            * Envoyons la requete au service QBlast et conservons l'ID de la requete pour `  
`                        * pouvoir recupere les resultats plus tard.`  
`            * (en fait, dans quelques secondes :-))`  
`            *`  
`            * Utilisez une structure de données pour garder la trace de toutes les requetes`  
`                        * est une bonne pratique.`  
`            *`  
`            */`  
`           while (rs.hasNext()) {`

`               RichSequence rr = rs.nextRichSequence();`  
`               request = rbw.sendAlignmentRequest(rr, rqb);`  
`               rid.add(request);`  
`           }`

`           /*`  
`            * Vérifions que la requete a ete traitee. Si complete, obtenons l'alignement en `  
`                        * utilisant mes parametres de sortie de mon choix.`  
`            */`  
`           for (String aRid : rid) {`  
`               System.out.println("trying to get BLAST results for RID "`  
`                       + aRid);`  
`               boolean wasBlasted = false;`

`               while (!wasBlasted) {`  
`                   wasBlasted = rbw.isReady(aRid, System.currentTimeMillis());`  
`               }`

`               rof = new RemoteQBlastOutputProperties();`  
`               rof.setOutputFormat(RemoteQBlastOutputFormat.TEXT);`  
`               rof.setAlignmentOutputFormat(RemoteQBlastOutputFormat.PAIRWISE);`  
`               rof.setDescriptionNumber(10);`  
`               rof.setAlignmentNumber(10);`

`               /*`  
`                * Pour vous montrer que ca fonctionne.`  
`                * `  
`                */`  
`               Set`<String>` test = rof.getOutputOptions();`  
`               `  
`               for(String str : test){`  
`                   System.out.println(str);`  
`               }`  
`               `  
`               is = rbw.getAlignmentResults(request, rof);`

`               BufferedReader br = new BufferedReader(`  
`                       new InputStreamReader(is));`

`               String line = null;`

`               while ((line = br.readLine()) != null) {`  
`                   System.out.println(line);`  
`               }`  
`           }`  
`       }`  
`       /*`  
`        * Ce qui arrive si on ne peut lire la sequence`  
`        */`  
`       catch (IOException ioe) {`  
`           ioe.printStackTrace();`  
`       }`  
`       /*`  
`        * Ce qui arrive si la sequence n'est pas une sequence FASTA`  
`        */`  
`       catch (BioException bio) {`  
`           bio.printStackTrace();`  
`       }`  
`   }`

} </java>
