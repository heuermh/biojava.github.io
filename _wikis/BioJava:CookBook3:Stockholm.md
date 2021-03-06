---
title: BioJava:CookBook3:Stockholm
---

How to Read Multiple Sequence Alignment files in Stockholm format
-----------------------------------------------------------------

StockholmFileParser is used to read MSA files written in Stockholm file
format. This example demonstrates how you can read one or more
StockholmStructure object(s) from a Stockholm file.

The StockholmFileParser class can read a single structure object file, a
multiple structure objects file, or an InputStream.

### To read a single object from a file, you can simply write

<java> public static void main(String[] args){

`   try {`  
`       StockholmFileParser parser = new StockholmFileParser();`  
`       String pathName= "stockholmFilePathAndName";`  
`       StockholmStructure structure = parser.parse(pathName);`  
`           `  
`       //use read structures`  
`           `  
`   } catch (IOException e) {`  
`       e.printStackTrace();`  
`   } catch (Exception e) {`  
`       e.printStackTrace();`  
`   }`

} </java>

### Also you can read multiple alignments within the same file as follows

<java> public static void main(String[] args){

`   try {`  
`       StockholmFileParser parser = new StockholmFileParser();`  
`       String sourcePath=settingsManager.getSourcePath();`  
`       String fileName= settingsManager.getFileName();`  
`       FileInputStream inStream = new FileInputStream(new File(sourcePath,fileName));`  
`       String outputPath=settingsManager.getOutputPath();`  
`       parser.parse(inStream,STRUCTURES_TO_SKIP);//if you don't want to start from first structure`  
`       do {`  
`           structures = parser.parse(inStream, MAX_PER_ITERATION);`  
`           for (int i = 0; i < structures.size(); i++) {`  
`               StockholmStructure structure = structures.get(i);`  
`               List`<AbstractSequence<? extends AbstractCompound>`> sequences = structure.getBioSequences(true);`  
`               final String accessionNumber = structure.getFileAnnotation().getAccessionNumber();`  
`               final String identification = structure.getFileAnnotation().getIdentification().toString();`  
`               manageRelatedSequences(accessionNumber, identification,sequences);`  
`           }`  
`       } while (structures.size()== MAX_PER_ITERATION);`  
`   } catch (FileNotFoundException e) {`  
`       e.printStackTrace();`  
`   } catch (IOException e) {`  
`       e.printStackTrace();`  
`   } catch (Exception e) {`  
`       e.printStackTrace();`  
`   }`

} </java>

### Some times you don't have a reference to the file or input stream

Some times you use the parser in a place other than where it was
created.

For example, you can create a StockholmFileParser in a function <java>

`   public StockholmFileParser getStockholmFileParser(String filePathName) {`  
`       StockholmFileParser parser = new StockholmFileParser();`  
`       try {`  
`           parser.parse(filePathName, 0);`  
`       } catch (ParserException e) {`  
`           e.printStackTrace();`  
`       } catch (IOException e) {`  
`           e.printStackTrace();`  
`       }`  
`       return parser;`  
`   }`

</java>

Then you use the created parser in another function, where you don't
have a reference to its underling data source <java>

`   public void useParser(StockholmFileParser parser) {`  
`       final int MAX_PER_ITTERATION = 10;`  
`       List`<StockholmStructure>` structures;`  
`       long count= 0;`  
`       int successfullyRead = 0;`  
`       do {`  
`           try {`  
`               structures = parser.parseNext(MAX_PER_ITTERATION);`  
`               successfullyRead = structures.size();`  
`           } catch (IOException e) {`  
`               e.printStackTrace();`  
`           }`  
`           count += successfullyRead;`  
`           System.out.println("reached "+count);`  
`           `  
`           //use read structures`  
`           `  
`       } while (successfullyRead== MAX_PER_ITTERATION);`  
`       System.out.println("TOTAL COUNT = "+count);`  
`   }`

</java>
