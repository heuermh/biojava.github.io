---
title: BioJava:CookBook:Distribution:Bayes
---

Using Distributions to make a naive Bayes classifier
----------------------------------------------------

[ Naive bayes classifiers](wp:Naive_Bayesian_classifier "wikilink") are
one of the simplist examples of probabilistic classifiers. Despite their
obvious weaknesses and naive assumptions they are also surprisingly
effective. Most commonly they are used for [supervised
learning](wp:Supervised_learning "wikilink") and classify observations
based on maximum likelihood.

Essentially, the classifier consists of two or more sets of probability
"feature" vectors or classes. These classes are generally based on
training examples. New observations are classified based on which class
they most closely represent. A very common application is spam filtering
based on word usage. Spam email frequently contains phrases and words
that are not so frequently found in non-spam email. By analysing the
word frequency of an email usnig a Bayes classifier one can determine a
probability that an email is spam.

In the simple example below we use BioJava arrays of `Distribution`s to
represent feature vectors for GT and AC rich sequences. The classifier
then calculates the most likely class for new observations. The
application is somewhat similar to a weight matrix with a non-uniform
null (background) distribution except that an entire sequence is
classified not subsequences as would be the case with a weight matrix. A
Bayes classifier can also have more than two classes where as a weight
matrix cannot.

The example consists of three java classes. The `BayesClassifier` holds
`Classification` objects (one for each class the classifier will
evaluate) and evaluates new observations against these classes. The
`TestRun` class is a simple program with a `main` method to demo the
application.

BayesClassifier.java
--------------------

<java> /\*

`* BayesClassifier.java`  
`*`  
`* Created on December 7, 2005, 1:32 PM`  
`*/`

package bayes;

import java.util.HashMap; import java.util.Map; import
org.biojava.bio.dist.Distribution; import
org.biojava.bio.symbol.IllegalSymbolException; import
org.biojava.bio.symbol.SymbolList;

/\*\*

`* Simple Naive Bayes classifier`  
`* @author Mark Schreiber`  
`*/`

public class BayesClassifier {

`   private Map name2Classifier;`  
`   private Map name2Prior;`  
`   private double totalPrior;`  
`   `  
`   /** Creates a new instance of BayesClassifier */`  
`   public BayesClassifier() {`  
`       name2Classifier = new HashMap();`  
`       name2Prior = new HashMap();`  
`       totalPrior = 0.0;`  
`   }`  
`   `  
`   /**`  
`    * Adds (or replaces if the name is the same) a `  
`    * classification. Note that adding another classification`  
`    * after some observations have already been evaluated`  
`    * will cause the previous evaluations to be invalid with`  
`    * respect to this one due to the prior weight.`  
`    * @param name The name off the classfication (eg positive)`  
`    * @param featureVector The features of the classification model`  
`    * @param prior The prior weight given to the classification.`  
`    * Doesn't need to be a probability. When the`  
`    * probability of a classification is calculated`  
`    * weights will be normalized to probabilities.`  
`    */`  
`   public void addClassification(String name,`  
`           Distribution[] featureVector,`  
`           double prior){`  
`       Classification c = new Classification();`  
`       c.setFeatureVector(featureVector);`  
`       `  
`       totalPrior += prior;`  
`       name2Prior.put(name, new Double(prior));`  
`       name2Classifier.put(name, c);`  
`   }`  
`   `  
`   /**`  
`    * The prior probability for the named classification.`  
`    * @return The prior weight set for that classification normalized as a`  
`    * probability.`  
`    */`  
`   public double getPriorProb(String classificationName){`  
`       Double pc = (Double)name2Prior.get(classificationName);`  
`       `  
`       return pc.doubleValue()/totalPrior;`  
`   }`  
`   `  
`   /**`  
`    * The natural log of the probability of the named class given`  
`    * the observation.`  
`    */`  
`   public double logProbClass(String classificationName,`  
`           SymbolList obs) throws IllegalSymbolException{`  
`       if(! name2Classifier.containsKey(classificationName) || `  
`               ! name2Prior.containsKey(classificationName)){`  
`           throw new IllegalArgumentException(classificationName+"not found");`  
`       }`  
`       `  
`       Classification c = (Classification)name2Classifier.get(classificationName);`  
`       `  
`       return Math.log(getPriorProb(classificationName))+c.pObservation(obs);                `  
`   }`

} </java>

Classification.java
-------------------

<java> /\*

`* Classification.java`  
`*`  
`* Created on December 7, 2005, 1:38 PM`  
`*`  
`*/`

package bayes;

import java.util.Iterator; import org.biojava.bio.dist.Distribution;
import org.biojava.bio.symbol.IllegalSymbolException; import
org.biojava.bio.symbol.Symbol; import org.biojava.bio.symbol.SymbolList;

/\*\*

`*`  
`* @author Mark Schreiber`  
`*/`

class Classification {

`   private Distribution[] featureVector;`  
`   `  
`   /** Creates a new instance of Classification */`  
`   public Classification() {`  
`       featureVector = new Distribution[0];`  
`   }`

`  /**`  
`   * Getter for the featureVector`  
`   * @return the actual feature vector, not a copy.`  
`   */`  
`   public Distribution[] getFeatureVector(){`  
`       return this.featureVector;`  
`   }`  
`   `  
`   /**`  
`    * Setter for the featureVector`  
`    * @param featureVector the vector of features as an array of Distributions`  
`    */`  
`   public void setFeatureVector(Distribution[] featureVector){`  
`       this.featureVector = featureVector;`  
`   }`

`   /**`  
`    * The probability of the observation given the feature vector of the class.`  
`    * @return the natural log probability.`  
`    * @throws IllegalSymbolException if obs contains symbols that are not from `  
`    * the alpahbet of the distributions in the feature vector.`  
`    */`  
`   double pObservation(SymbolList obs) throws IllegalSymbolException{`  
`       if(obs == null) throw new IllegalArgumentException("obs cannot be null");`  
`       //obs and featureVector need to be the same length`  
`       if(obs.length() != featureVector.length){`  
`           throw new IllegalArgumentException("obs and featureVector need to be the same length");`  
`       }`  
`       `  
`       double p = 0.0;`  
`       int i = 0;`  
`       for(Iterator it = obs.iterator(); it.hasNext(); i++){`  
`           Symbol s = (Symbol)it.next();`  
`           Distribution d = featureVector[i];`  
`           p += Math.log(d.getWeight(s));`  
`       }`  
`       return p;`  
`   }`

} </java>

TestRun.java
------------

<java> /\*

`* TestRun.java`  
`*/`

package bayes;

import org.biojava.bio.dist.Distribution; import
org.biojava.bio.dist.SimpleDistribution; import
org.biojava.bio.seq.DNATools; import org.biojava.bio.symbol.SymbolList;

/\*\*

`*`  
`* @author Mark Schreiber`  
`*/`

public class TestRun {

`   Distribution[] feat1;`  
`   Distribution[] feat2;`  
`   SymbolList seq1;`  
`   SymbolList seq2;`  
`   BayesClassifier c;`  
`   `  
`   /** Creates a new instance of TestRun */`  
`   public TestRun() throws Exception{`  
`       c = new BayesClassifier();`  
`       initFeat1(); initFeat2();`  
`       c.addClassification("class1", feat1, 0.5);`  
`       c.addClassification("class2", feat2, 0.5);`  
`       `  
`       seq1 = DNATools.createDNA("gtctgaagtg"); //gt rich (class1)`  
`       seq2 = DNATools.createDNA("accaacgtac"); //ac rich (class2)`  
`   }`  
`   `  
`   /**`  
`    * runs the classification demo.`  
`    */`  
`   public void classify() throws Exception{`  
`       double p1 = 0.0;`  
`       double p2 = 0.0;`  
`       `  
`       p1 = c.logProbClass("class1", seq1);`  
`       System.out.println("log p(class1 | seq1) = "+p1);`  
`       p2 = c.logProbClass("class2", seq1);`  
`       System.out.println("log p(class2 | seq1) = "+p2);`  
`       System.out.println("logratio p(class1 | seq1) / p(class2 | seq1) = "+(p1 -p2));`  
`       `  
`       System.out.print("\n");`  
`       `  
`       p1 = c.logProbClass("class1", seq2);`  
`       System.out.println("log p(class1 | seq2) = "+p1);`  
`       p2 = c.logProbClass("class2", seq2);`  
`       System.out.println("log p(class2 | seq2) = "+p2);`  
`       System.out.println("logratio p(class1 | seq2) / p(class2 | seq2) = "+(p1 -p2));`  
`   }`  
`   `  
`   /**`  
`    * Initiates a feature vector for GT rich sequences.`  
`    */ `  
`   private void initFeat1() throws Exception{`  
`       feat1 = new Distribution[10];`  
`       for(int i = 0; i < feat1.length; i++){`  
`           feat1[i] = new SimpleDistribution(DNATools.getDNA());`  
`           //gt rich`  
`           feat1[i].setWeight(DNATools.a(), 0.1);`  
`           feat1[i].setWeight(DNATools.c(), 0.1);`  
`           feat1[i].setWeight(DNATools.g(), 0.4);`  
`           feat1[i].setWeight(DNATools.t(), 0.4);`  
`       }`  
`   }`  
`   `  
`    /**`  
`    * Initiates a feature vector for AC rich sequences.`  
`    */ `  
`   private void initFeat2() throws Exception{`  
`       feat2 = new Distribution[10];`  
`       for(int i = 0; i < feat2.length; i++){`  
`           feat2[i] = new SimpleDistribution(DNATools.getDNA());`  
`           //ac rich`  
`           feat2[i].setWeight(DNATools.a(), 0.4);`  
`           feat2[i].setWeight(DNATools.c(), 0.4);`  
`           feat2[i].setWeight(DNATools.g(), 0.1);`  
`           feat2[i].setWeight(DNATools.t(), 0.1);`  
`       }`  
`   }`  
`   `  
`   /**`  
`    * Runs the demo`  
`    * @param args the command line arguments`  
`    */`  
`   public static void main(String[] args) throws Exception{`  
`       TestRun tr = new TestRun();`  
`       tr.classify();`  
`   }`  
`   `

} </java>
