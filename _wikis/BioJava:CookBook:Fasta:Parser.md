---
title: BioJava:CookBook:Fasta:Parser
---

How Do I Parse a FASTA Search Result?
-------------------------------------

The procedure for parsing FASTA results is very similar to the procedure
for parsing BLAST results. The code below is essentially the same as the
blast parser except for the use of a FastaSearchSAXParser instead of a
BlastLikeSAXParser.

It is important to note that the Fasta parser classes provided with
biojava will only ever work with output produced with the -m 10 option.
This is a nice machine readable output that is more easily parsed.
Please consult your Fasta documentation for more information.

Below are two code examples. The first is a parser that binds everything
to biojava ssbind objects. The second is the equivalent of the BlastEcho
program which prints parsing events to STDOUT. This is useful for
designing your own parser if you are only interested in small parts of
the output and you don't want your JVM memory to be consumed by lots of
objects

### FastaParser.java

<java> /\*

`* FastaParser.java`  
`*`  
`* Created on July 13, 2005, 10:15 AM`  
`*`  
`* `  
`*/`

import java.io.FileInputStream; import java.io.IOException; import
java.io.InputStream; import java.util.ArrayList; import
java.util.Iterator; import java.util.List; import
org.biojava.bio.Annotation; import
org.biojava.bio.program.sax.FastaSearchSAXParser; import
org.biojava.bio.program.ssbind.BlastLikeSearchBuilder; import
org.biojava.bio.program.ssbind.SeqSimilarityAdapter; import
org.biojava.bio.search.SearchContentHandler; import
org.biojava.bio.search.SeqSimilaritySearchHit; import
org.biojava.bio.search.SeqSimilaritySearchResult; import
org.biojava.bio.seq.db.DummySequenceDB; import
org.biojava.bio.seq.db.DummySequenceDBInstallation; import
org.xml.sax.InputSource; import org.xml.sax.SAXException;

public class FastaParser {

` /**`  
`  * args[0] is assumed to be the name of a Fasta output file`  
`  */`  
` public static void main(String[] args) {`  
`   try {`  
`     //get the Fasta input as a Stream`  
`     InputStream is = new FileInputStream(args[0]);`

`     //make a FastaSearchSAXParser`  
`     FastaSearchSAXParser parser = new FastaSearchSAXParser();`

`     //make the SAX event adapter that will pass events to a Handler.`  
`     SeqSimilarityAdapter adapter = new SeqSimilarityAdapter();`

`     //set the parsers SAX event adapter`  
`     parser.setContentHandler(adapter);`

`     //The list to hold the SeqSimilaritySearchResults`  
`     List results = new ArrayList();`

`     //create the SearchContentHandler that will build SeqSimilaritySearchResults`  
`     //in the results List`  
`     SearchContentHandler builder = new BlastLikeSearchBuilder(results,`  
`         new DummySequenceDB("queries"), new DummySequenceDBInstallation());`

`     //register builder with adapter`  
`     adapter.setSearchContentHandler(builder);`

`     //parse the file, after this the result List will be populated with`  
`     //SeqSimilaritySearchResults`  
`     parser.parse(new InputSource(is));`

`     //output some blast details`  
`     for (Iterator i = results.iterator(); i.hasNext(); ) {`  
`       SeqSimilaritySearchResult result =`  
`           (SeqSimilaritySearchResult)i.next();`

`       Annotation anno = result.getAnnotation();`

`       for (Iterator j = anno.keys().iterator(); j.hasNext(); ) {`  
`         Object key = j.next();`  
`         Object property = anno.getProperty(key);`  
`         System.out.println(key+" : "+property);`  
`       }`  
`       System.out.println("Hits: ");`

`       //list the hits`  
`       for (Iterator k = result.getHits().iterator(); k.hasNext(); ) {`  
`         SeqSimilaritySearchHit hit =`  
`             (SeqSimilaritySearchHit)k.next();`  
`         System.out.print("\tmatch: "+hit.getSubjectID());`  
`         System.out.println("\te score: "+hit.getEValue());`  
`       }`

`       System.out.println("\n");`  
`     }`

`   }`  
`   catch (SAXException ex) {`  
`     //XML problem`  
`     ex.printStackTrace();`  
`   }catch (IOException ex) {`  
`     //IO problem, possibly file not found`  
`     ex.printStackTrace();`  
`   }`  
` }`

} </java>

### FastaEcho.java

<java> import java.io.FileInputStream; import java.io.IOException;
import org.biojava.bio.program.sax.FastaSearchSAXParser; import
org.biojava.bio.program.ssbind.SeqSimilarityAdapter; import
org.biojava.bio.search.SearchContentAdapter; import
org.biojava.bio.search.SearchContentHandler; import
org.xml.sax.ContentHandler; import org.xml.sax.InputSource; import
org.xml.sax.SAXException;

/\*\*

`* `

Echos events from a FastaSearchSAXParser

`*/ `

public class FastaEcho {

` public FastaEcho() { `  
` } `

` private void echo (InputSource source) throws IOException, SAXException{ `  
`   //make a FastaSearchSAXParser `  
`   FastaSearchSAXParser parser = new FastaSearchSAXParser(); `  
`   `  
`   ContentHandler handler = new SeqSimilarityAdapter();`  
`   `  
`   //use our custom SearchContentHandler (see below)`  
`   SearchContentHandler scHandler = new EchoSCHandler(); `  
`   ((SeqSimilarityAdapter)handler).setSearchContentHandler(scHandler); `

`   parser.setContentHandler(handler); `  
`   parser.parse(source); `  
` } `

` /**`  
`  * Customs Search Content Handler. Intercepts all events and logs`  
`  * them to STDOUT`  
`  */`  
` private class EchoSCHandler extends SearchContentAdapter{ `  
`   public void startHit(){ `  
`     System.out.println("startHit()"); `  
`   } `  
`   public void endHit(){ `  
`     System.out.println("endHit()"); `  
`   } `  
`   public void startSubHit(){ `  
`     System.out.println("startSubHit()"); `  
`   } `  
`   public void endSubHit(){ `  
`     System.out.println("endSubHit()"); `  
`   } `  
`   public void startSearch(){ `  
`     System.out.println("startSearch"); `  
`   } `  
`   public void endSearch(){ `  
`     System.out.println("endSearch"); `  
`   } `  
`   public void addHitProperty(Object key, Object val){ `  
`     System.out.println("\tHitProp:\t"+key+": "+val); `  
`   } `  
`   public void addSearchProperty(Object key, Object val){ `  
`     System.out.println("\tSearchProp:\t"+key+": "+val); `  
`   } `  
`   public void addSubHitProperty(Object key, Object val){ `  
`     System.out.println("\tSubHitProp:\t"+key+": "+val); `  
`   }`  
`   public void setQueryID(String queryID) {`  
`     System.out.println("\tQueryID:\t "+queryID);`  
`   }`  
`   public void setDatabaseID(String databaseID) {`  
`     System.out.println("\tDatabaseID: "+databaseID);`  
`   }`  
` } `

` public static void main(String[] args) throws Exception{ `  
`   InputSource is = new InputSource(new FileInputStream("fasta_3.3t08.out")); `  
`   FastaEcho fastaEcho = new FastaEcho(); `  
`   fastaEcho.echo(is); `  
` } `

} </java>
