# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1.
[Bug Iphone 14](https://www.firstpost.com/tech/news-analysis/iphone-14-and-apple-watch-crash-detection-trigger-false-emergency-sos-calls-on-basic-roller-coasters-11423741.html).

L'Iphone 14 est équipé d'un système d'appel d'urgence des secours en cas d'accident de voiture. Le problème est que ce système a tendance à se déclencher pendant des montagnes russes ou autres attractions à sensations fortes.

Le bug est global, car il est dû à une stimulation inattendue du logiciel par l'utilisateur, car à cause de la vitesse, des freinages et mouvements brusques, les informations transmisses par les capteurs au logiciel lui font croire à un accident de voiture. Cela arrive malgré un apprentissage du logiciel avec plusieurs millions d'heures d'accidents de voitures.
   
Cela entraîne un désagrément pour les clients qui voient l'arrivée des secours alors que leur intégrité physique est complète, et surtout pour les secours qui se sont déplacés pour aucune raison. Pour Apple, c'est une mauvaise publicité et cela peut entraîner une baisse de ventes de ce nouveau produit.

Apple propose comme solutions de détecter les zones d'attractions pour ne plus que les dispositifs d'urgence ne se déclenchent et d'améliorer l'algorithme responsable du problème.

Tester ce dispositif dans de nombreuses situations extrêmes comme les parcs d'attraction, le karting ou autre, de manière plus importante et à plus grande échelle aurait pu permettre de se rendre compte du bug avant. Il aurait en effet fallu vérifier si le dispositif ne fonctionnait pas dans de nombreuses situations autant que vérifier s'il marchait dans les situations où il le devait.

2.
[UnmodifiableNavigableSet can be modified by pollFirst() and pollLast()](https://issues.apache.org/jira/browse/COLLECTIONS-799)

Ce bug permettait de modifier un UnmodifiableNavigableSet qui par définition n'était pas censé pouvoir être modifié. Ce bug était réalisable en exécutant les méthodes pollFirst() et pollLast() de la Collection en question. Le résultat attendu lors de l'appel de ces méthodes était une exception, mais dans ce cas, les méthodes étaient bien exécutées et modifiaient la collection.


Le bug a été corrigé en ajoutant un throw d'exception pour chacune des deux méthodes.


Ce bug est un bug local, car il s'agit probablement d'un oubli étant donné que d'autres collections similaires lançaient déjà des exceptions pour ce genre de cas.


Le contributeur qui a corrigé ce bug a également ajouté des tests qui vérifiaient bien que l'appel des deux fonctions lançait bien une exception.

3.
Netflix utilise la méthode de test de tolérance aux bugs. Pour cela, ils ont pis en place ce qui est appelé le "Chaos Engineering". Dans le cas de Netflix, cela consiste à mettre en place un service appelé "Chaos Monkey" qui va shutdown des instances de machines virtuelles servant à faire tourner des services de production de Netflix. L'entreprise a même poussé le concept encore plus loin en créant "Chaos Kong" qui simule la mise hors service des serveurs d'une région entière. Le papier liste 4 prérequis pour mener à bien cette méthode de test :


- Premièrement, il faut construire une hypothèse autour d'un comportement stable du système. Un exemple donné par Netflix est la mesure du nombre de commencements de visionnages par seconde (stream Start Per Second SPS).
  Cette hypothèse peut alors servir à prévoir les attentes suite à un échec système ou autres.
- Ensuite, il faut avoir une liste de variables indépendantes pouvant être variés. Ici, on parle de variables venant du monde réel. Le papier donne l'exemple du temps de latence pour les requêtes, un échec d'un service interne, un échec d'une requête entre deux services internes, etc...
- Ensuite, l'intérêt du "Chaos Engineering" est de lancer ces expériences directement dans l'environnement de production. L'intérêt ici est de tester l'intégration des services entre eux dans la production.
- Le dernier point est d'automatiser les expériences pour qu'elles tournent en continue dans le système en production. L'utilité ici est de voir que chaque expérience se conclut correctement dans le temps.


Pour évaluer le résultat de ces expérimentations, Netlflix peut observer le SPS. Comme l'exemple donné dans l'article, ils peuvent simuler l'arrêt d'un service (non-essentiel au visionnage), et comparer un groupe de test qui n'a plus accès au service et un groupe où tout fonctionne. L'important pour eux est que le nombre de visionnages par seconde ne soit pas plus bas pour le groupe qui a le service arrêté par rapport au groupe témoin. Netflix n'est pas la seule entreprise à mettre en place cette méthode, l'article cite également : Amazon, Google, Microsoft et Facebook. On peut imaginer que ce concept peut être étendu à des sites de ventes en ligne par exemple. Ils pourraient par exemple observer le nombre de ventes par minutes en fonction de l'échec de certains services en production.

4.
La spécification formelle permet une abstraction de la machine, du langage et de la plateforme. Elle permet également d'avoir un langage bas niveau donc plus rapide et beaucoup plus "solide", notamment par rapport à JavaScript. Il est également plus simple de compiler d'un langage vers le WebAssembly et le langage est plus simple à lire car clairement formalisé.


L'implémentation du WebAssembly doit évidemment tout de même être testée, car les erreurs de syntaxes par l'utilisateur ou lors de la compilation depuis un autre langage sont possibles. Des bugs plus globaux sont également possibles, ainsi une couverture plus globale est nécessaire.

5.
Les avantages principaux de la mécanisation sont le renforcement du typage et de la sémantique du WebAssembly, ce qui entraîne de légères différences de syntaxe. La formalisation originelle propose 46 règles de réduction contre 65 règles pour la formalisation de ce papier. Cela permet d'assurer plus d'opérations pour rendre plus solide le langage.



La nouvelle spécification est donc très semblable à l'ancienne, mais le renforcement du typage et de la sémantique permet d'améliorer cette spécification.



Isabelle fournit aussi un interpréteur permettant aussi de tester la spécification.


Cela n'empêche pas le besoin de tester la spécification, car il peut y avoir des erreurs d'implémentation ou de compilation depuis un autre langage.