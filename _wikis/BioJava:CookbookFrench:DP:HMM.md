---
title: BioJava:CookbookFrench:DP:HMM
---

Comment créer un ProfileHMM?
----------------------------

Les profils HMM (comme ceux utilisés par le programme HMMER) sont des
outils très sensibles pour chercher des motifs dans une séquence. Un
profil HMM est entrainé de manière habituelle à partir d'un ensemble de
séquences qui contiennent le motif d'interêt en utilisant l'algorithme
de Baum-Welch. Cet algorithme optimise les paramètres du modèle jusqu'à
ce qu'un critère de sortie soit satisfait. Une fois qu'un profil HMM a
été construit, l'algorithme de Viterbi peut être utiliser pour
déterminer le parcours d'états les plus probable d'avoir générer une
séquence observée (test). Si un nombre suffisant d'états d'identité sont
observés, alors on considère que la séquence test contient le motif.
Alternativement, on peut utiliser une métrique de pointage (comme log
odds) et définir un point-limite (*cutoff threshold*). Le programme
suivant montre la construction et l'utilisation d'un *ProfileHMM* avec
BioJava.

La première étape est la création du profil HMM.

<java> /\*

`* créer un profile HMM sur un Alphabet d'ADN avec 12 "columns" et les valeurs par défaut`  
`* pour les DistributionFactories pour construire les Distributions de transition et d'émission`  
`*/`  
`ProfileHMM hmm = new ProfileHMM(DNATools.getDNA(),`  
`                     12,`  
`                     DistributionFactory.DEFAULT,`  
`                     DistributionFactory.DEFAULT,`  
`                     "my profilehmm");`

//créer la matrice de programmation dynamique pour le modèle. dp =
DPFactory.DEFAULT.createDP(hmm); </java>

Ici, vous feriez la lecture d'un ensemble de sÉquences qui forment
l'ensemble d'entrainement.

<java> //DB pour contenir l'ensemble d'entrainement. SequenceDB db = new
HashSequenceDB();

//votre code ici pour charger les sÉquences dans l'ensemble </java>

Initialiser maintenant tous les paramètres du modèle à une valeur
uniforme. Alternativement, les paramètres pourraient être déterminer de
manière aléatoire ou établis pour représenter une estimation du meilleur
modèle possible. Utiliser ensuite l'algorithme de Baum-Welch pour
optimiser les parametres.

<java> //former le modèle pour avoir des paramètres uniformes
ModelTrainer mt = new SimpleModelTrainer();

//enregistrer le modèle à former mt.registerModel(hmm);

//puisqu'aucun autre compte est utiliser, la valeur null rendra tout
uniforme mt.setNullModelWeight(1.0); mt.train();

//créer un formateur BW pour la matrice dp genérée à partir du HMM
BaumWelchTrainer bwt = new BaumWelchTrainer(dp);

//implémentation anonyme du critère d'arrêt pour arrêter après 20
itérations StoppingCriteria stopper = new StoppingCriteria(){

`  public boolean isTrainingComplete(TrainingAlgorithm ta){`  
`       return (ta.getCycle() > 20);`  
`     }`  
`   };`  
`   `

/\*

`* optimisé la matrice dp en tenant compte de l'ensemble d'entrainement de db en utilisant un`  
`* modèle vide avec un poids de 1.0 et le critère d'arret défini ci-dessus.`  
`*/`

bwt.train(db,1.0,stopper); </java>

Vous trouverez ci-dessous un exemple d'évaluation d'une séquence et la
sortie des parcours d'état.

<java> SymbolList test = null;

//ici, code pour initialiser la séquence test

/\*

`* mettre la séquence dans un tableau; un tableau est utiliser car pour les alignements par paire`  
`* utilisant un HMM, vous avez besoin de 2 SymbolLists dans le tableau.`  
`*/`  
`   `  
`SymbolList[] sla = {test};`  
`   `  
`//décoder le parcours d'état le plus probable et produire la valeur 'odds'`

StatePath path = dp.viterbi(sla, ScoreType.ODDS);
System.out.println("Log Odds = "+path.getScore()); //imprimer le
parcours d'état

`   for(int i = 1; i <= path.length(); i++){`  
`     System.out.println(path.symbolAt(StatePath.STATES, i).getName());`  
`   }`

</java>
