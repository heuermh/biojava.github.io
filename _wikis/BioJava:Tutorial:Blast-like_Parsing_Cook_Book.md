---
title: BioJava:Tutorial:Blast-like Parsing Cook Book
---

by **[Cambridge Antibody
Technology](mailto:bioinformatics@CambridgeAntibody.com)**

This section of the BioJava tutorial covers making use of the output
from software used for sequence similarity/homology based searches of
biological databases. The material is presented in a Cook Book fashion
giving practical examples that should be enough to get you going. If you
want to make use of the output from the following programs by using
BioJava, this is a useful tutorial to work through:

-   NCBI Blast (blastn, blastx, blastp, tblastn, tblastx)
-   WU-Blast (blastn, blastx, blastp, tblastn, tblastx)
-   or HMMER

*NB Please check the JavaDocs of `BlastLikeSAXParser` to see the extent
of support for output from the various applications.*

The section of BioJava you will be making use of in the tutorial is the
SAX2-compliant event-based parsing framework. After following this
tutorial, you will you be able to not only to deal with output from the
above pieces for bioinformatics software, but also get started with
working with other types of data, such as three-dimensional
macromolecular structures which are also supported by the framework.

What you need to know about the parsing framework
-------------------------------------------------

The framework has been designed in such a way that you don't need to
understand the details of how it works in order to use it. This is
achieved by providing facade classes that are simple to use. For parsing
Blast-like output, the facade class you need to use is
`org.biojava.bio.program.sax.BlastLikeSAXParser`. You pass streams of
data to this class, and the framework will do the rest. As the name
suggests, this class is actually a SAX parser, and implements the
`org.xml.sax.XMLReader` interface. You are thus able to treat the output
data as thought it is in an XML format.

The framework performs the magic of emitting SAX2 events from non-XML
format data. Thus you don't have to do any parsing yourself. Rather you
will simply be writing XML Content Handlers. The recipes for XML Content
Handlers presented here will point you in the direction of populating
your own (or BioJava) objects with bioinformatics data.

It is also worth noting, that the SAX events that the framework emits
are consistent with a scenario where all the pieces of bioinformatics
software above, actually produced identically formatted data.

Benefits of using the framework
-------------------------------

-   Allows you to focus on the objects you want to create, and forget
    about writing complex parsing code
-   Allows you to make use of the output from more pieces of software.
    Because of the "concept-based" approach to the representation of
    data, many of the Content Handler classes you write can be re-used
    with the output of several different programs.

Recipes
-------

The recipes are simple examples designed to get you up and running
populating objects in the way you want. For each example recipe, two
classes are provided:

-   An XML Content Handler (this is the class that does the work of
    populating objects with data)
-   A sample application class that takes blast-like program output and
    and sets up for parsing using the Content Handler class.

*NB You will find the complete source code for all the classes described
here the demos section of biojava, in the eventbasedparsing package.*

After Example 1, the only classes that are described are the XML Content
Hander classes, because the application classes are essentially
identical for all examples.

To help you get going, in addition to the source code for the examples,
there are also several example examples of raw ouput from NCBI-blast,
WU-blast, and HMMER the "files" directory of the demos section of
biojava.

### Example 1

For all the hits from a search as detailed in the summary section of the
output, prepare a list of Hit Ids. This is an example of a re-useable
Content Handler. The same piece of code works equally well with the
output from multiple flavours of NCBI Blast, WU-Blast, and HMMER.

#### Step A - Create an application that sets up the parser and does the parsing

The full source is in `eventbasedparsing.TutorialEx1`. Because there is
no difference between what you do here, and what you would do to parse
XML files there isn't much to do. First create a SAX Parser that deals
with Blast-like output.

    XMLReader oParser = (XMLReader) new BlastLikeSAXParser(); 

Next choose the Content Handler. In this case, we will be using the
class `TutorialEx1Handler`, which takes a reference to an `ArrayList` in
the constructor. When the SAX Parser parses the file, the Content
Handler will populate the `ArrayList` with Hit Ids from the summary
section of the output.

    ContentHandler oHandler =
       (ContentHandler) new TutorialEx1Handler(oDatabaseIdList);  

The final step in the set-up is to connect the Content Handler to the
SAX Parser.

    oParser.setContentHandler(oHandler); 

For the purposes of the tutorial applications, we will simply be reading
output from files on disk. Create a `FileInputStream`, and parse it by
calling the parse method on the SAX Parser.

    oInputFileStream = new FileInputStream(oInput);
    oParser.parse(new InputSource(oInputFileStream));

Finally, having populated the `ArrayList` with HitIds, we simply print
them out.

    System.out.println("Results of parsing");
    System.out.println("==================");
    for (int i = 0; i < oDatabaseIdList.size();i++) {
          System.out.println(oDatabaseIdList.get(i));
    }

#### Step B - Create the logic for parsing

This is simply of matter of writing an XML Content Handler. The full
source is in `eventbasedparsing.TutorialEx1Handler`. The logic here is
trivial, we simply wish to identify Hit Ids that are contained within in
the Summary sections of the output data, and add each Hit Id to the
ArrayList.

    if ( (oNameStack.peek().toString().equals("HitId")) &&
         (this.findInStack("Summary") != -1) ) {
       oDatabaseIdList.add(poAtts.getValue("id"));
    }

#### Running the application

After compiling, if you run the application from the demos directory by
typing the following:

    java eventbasedparsing/TutorialEx1 files/ncbiblast/shortBlastn.out

You should see the following output:

    Results of parsing
    ==================
    U51677
    L38477
    X80457

<Category:Tutorial>
