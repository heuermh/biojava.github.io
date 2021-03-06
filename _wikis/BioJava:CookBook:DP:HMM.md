---
title: BioJava:CookBook:DP:HMM
---

How do I make a ProfileHMM?
---------------------------

Profile HMMs (such as those used in the program HMMER) are very
sensitive tools for searching for motifs. A profile HMM is typically
trained from a set of input sequences that contain the motif of interest
using the Baum-Welch algorithm. This algorithm optimises the parameters
of the model until some stopping criteria is satisfied. Once a profile
HMM has been constructed the Viterbi algorithm can be used to determine
the state path most likely to have generated an observed (test)
sequence. If sufficient match states are observed the test sequence can
be deemed to contain the motif, alternatively some scoring metric can be
used (such as log odds) and a cutoff threshold defined. The following
demonstrates the construction and use of a ProfileHMM in BioJava.

The first step is to create the profile HMM.

<java>

`   /*`  
`    * Make a profile HMM over the DNA Alphabet with 12 'columns' and default`  
`    * DistributionFactories to construct the transition and emmission`  
`    * Distributions`  
`    */`  
`   ProfileHMM hmm = new ProfileHMM(DNATools.getDNA(),`  
`                        12,`  
`                        DistributionFactory.DEFAULT,`  
`                        DistributionFactory.DEFAULT,`  
`                        "my profilehmm");`

`   //create the Dynamic Programming matrix for the model.`  
`   dp = DPFactory.DEFAULT.createDP(hmm);`

</java>

At this point you would read in a set of sequences that make up the
training set.

<java>

`   //Database to hold the training set`  
`   SequenceDB db = new HashSequenceDB();`  
`   `  
`   //code here to load the training set`

</java>

Now initialize all of the model parameters to a uniform value.
Alternatively parameters could be set randomly or set to represent a
guess at what the best model might be. Then use the Baum-Welch Algorithm
to optimise the parameters.

<java>

`   //train the model to have uniform parameters`  
`   ModelTrainer mt = new SimpleModelTrainer();`  
`   //register the model to train`  
`   mt.registerModel(hmm);`  
`   //as no other counts are being used the null weight will cause everything to be uniform`  
`   mt.setNullModelWeight(1.0);`  
`   mt.train();`

`   //create a BW trainer for the dp matrix generated from the HMM`  
`   BaumWelchTrainer bwt = new BaumWelchTrainer(dp);`

`   //anonymous implementation of the stopping criteria interface to stop after 20 iterations`  
`   StoppingCriteria stopper = new StoppingCriteria(){`  
`     public boolean isTrainingComplete(TrainingAlgorithm ta){`  
`       return (ta.getCycle() > 20);`  
`     }`  
`   };`  
`   `  
`   /*`  
`    * optimize the dp matrix to reflect the training set in db using a null model`  
`    * weight of 1.0 and the Stopping criteria defined above.`  
`    */`  
`   bwt.train(db,1.0,stopper);`

</java>

Below is an example of scoring a sequence and outputting the state path.

<java>

`   SymbolList test = null;`  
`   //code here to initialize the test sequence`  
`   `  
`   /*`  
`    * put the test sequence in an array, an array is used because for pairwise`  
`    * alignments using an HMM there would need to be two SymbolLists in the `  
`    * array`  
`    */`  
`   `  
`   SymbolList[] sla = {test};`  
`   `  
`   //decode the most likely state path and produce an 'odds' score`  
`   StatePath path = dp.viterbi(sla, ScoreType.ODDS);`  
`   System.out.println("Log Odds = "+path.getScore());`

`   //print state path`  
`   for(int i = 1; i <= path.length(); i++){`  
`     System.out.println(path.symbolAt(StatePath.STATES, i).getName());`  
`   }`

</java>
