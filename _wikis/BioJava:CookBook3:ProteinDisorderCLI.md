---
title: BioJava:CookBook3:ProteinDisorderCLI
---

Can I use the (protein disorder) predictor from the command line?
-----------------------------------------------------------------

BioJava module *biojava3-protein-disorder* can be compiled into a single
executable JAR file and run using <java> java -jar <jar_file_name>
</java> command. The jar file can be downloaded from the BioJava
repository
[biojava3-protein-disorder-3.0.2-SNAPSHOT.jar](http://www.biojava.org/download/maven/org/biojava/biojava3-protein-disorder/)

Alternatively if you want to integrate the predictor into your code you
can use [ API functions](BioJava:CookBook3:ProteinDisorder "wikilink")
to perform the calculations.

Options supported by the command line executable
------------------------------------------------

     
    JRONN version 3.1b usage 1 August 2011:
    java -jar JRONN_JAR_NAME -i=inputfile <OPTIONS>

    Where -i=input file 
        Input file can contain one or more FASTA formatted sequences.

    All OPTIONS are optional

    Supported OPTIONS are: 
        -o=output file
        -d=disorder value
        -f=V or H 
        -s=statistics file
        -n=number of threads to use

    OPTION DETAILED DESCRIPTION:
        -o full path to the output file, if not specified 
        standard out is used

        -d the value of disorder, defaults to 0.5

        -f output format, V for vertical, where the letters 
        of the sequence and corresponding disorder values are 
        output in two column layout. H for horizontal, where the
        disorder values are provided under the letters of the 
        sequence. Letters and values separated by tabulation in
        this case. Defaults to V.

        -s the file name to write execution statistics to.

        -n the number of threads to use. Defaults to the number of 
        cores available on the computer. n=1 mean sequential 
        processing. Valid values are 1 < n < (2 x num_of_cores)
        Default value will give the best performance.
        
    EXAMPLES: 

        Predict disorder values for sequences from input file /home/input.fasta
        output the results to the standard out. Use default disorder value
        and utilise all cpus available on the computer.

        java -jar JRONN.JAR -i=/home/input.fasta
        
        Predict disorder values for sequences from input file /home/input.fasta
        output the results in horizontal layout to the /home/jronn.out, collect 
        execution statistics to /home/jronn.stat.txt file and limit the number 
        of threads to two. 
        
        java -jar JRONN.JAR -i=/home/input.fasta -o=/home/jronn.out -d=0.6 -n=2 -f=H
         
        The arguments can be provided in any order.
