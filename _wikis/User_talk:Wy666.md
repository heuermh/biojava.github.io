---
title: User talk:Wy666
---

I have tested the demos\\seq\\TestGenbank.java on my computer, but the
results of the features are not as the same order as the original file.
How could I get the features in the same order of the original file?

demos\\seq\\TestGenbank.java:

for(Iterator i = seq.features(); i.hasNext(); ) {

`         Feature f = (Feature) i.next();`  
`         System.out.println("\t" + f.getType() + "\t" + f.getLocation() + "\t" +               f.getAnnotation().asMap());`  
`       }`
