---
title: Coding exercise
---

### Task 1

Write a FASTA parser

Your solution should:

-   be a Java code of high quality (maintability, reusability, OO
    design, etc)
-   be efficient
-   be capable of reading badly formatted FASTA files
-   be capable of reading large files
-   has convenient API
-   be possible to extend to reading FASTQ files

Please refrain from using any libraries that are not part of a standard
Java 6 development kit in your production code. For testing code fill
free to use any library you feel comfortable with.

### Task 2

Write a FASTA writer

-   Use your parser to read a FASTA file which contains sequences with
    ambiguous characters (you choose whether this is going to be
    ambiguous DNA or protein sequence)
-   Write two FASTA output files one with sequences which contains
    ambiguous characters and another one without.

### Submission

Please submit your completed exercise to **gsocexercise at gmail dot
com** by Friday the 6 of April inclusive. Your submission should be a
ZIP archive that contains an executable JAR file with your FASTA parser
and writer as well as

-   your source files in the *src* directory
-   your documentation files in the *docs* directory
-   the test data file named data.fasta up to 10Kb in size
-   The executable JAR containing the program. This should be called
    *runme.jar*.
-   a pure ASCII text file called *choices.txt* describing the
    significant design choices you made, uncertainties you had regarding
    the project, and the decisions you made when resolving them.

