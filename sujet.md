# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### Question 1

Un bug logiciel critique (CVE-2024-21410) a été découvert dans Microsoft Exchange Server en février 2024. Ce bug permettait d’exploiter des failles dans les protocoles NTLM pour obtenir une élévation de privilèges. Cette vulnérabilité zero-day permettait aux attaquants d’utiliser des hachages Net-NTLMv2 compromis pour s’authentifier sur des serveurs vulnérables et compromettre des opérations sensibles au nom des utilisateurs ciblés.

Les organisations utilisant Exchange Server ont été exposées à des risques de violations de données et d’accès non autorisé à des informations sensibles. Microsoft a dû déployer un correctif en urgence, ce qui a impacté sa réputation en matière de sécurité.

Ce bug exploitait une faille connue dans les attaques de relais NTLM, donc des tests approfondis, comme des analyses fuzzing et des simulations d’attaques de relais, auraient probablement permis d’identifier cette vulnérabilité plus tôt.

Microsoft a depuis activé par défaut la Protection Avancée pour l'Authentification (EPA) afin de prévenir des incidents similaires.

### Question 2

Le bug COLLECTIONS-802 dans le projet Apache Commons Collections concerne une violation du contrat de l'itérateur remove() dans la classe ReferenceMap. Ce bug peut être classé comme local, car il est limité à un comportement spécifique de cette classe et ne s'étend pas à d'autres parties du projet ou à des interactions externes.

Lorsqu’un élément était supprimé via l’itérateur d’une ReferenceMap, le contrat de la méthode remove() n’était pas respecté, entraînant un comportement inattendu. Cela pouvait causer des incohérences ou des erreurs lorsque l'on tentait d’utiliser l’itérateur après suppression. Ce problème provenait d’une mauvaise gestion de l'état interne de la ReferenceMap après la suppression.

Les contributeurs ont corrigé le problème en ajustant la logique interne de l’itérateur pour garantir que l’état de la ReferenceMap reste cohérent après chaque appel de remove().

Les contributeurs ont ajouté des tests unitaires pour s’assurer que le bug ne réapparaisse pas. Ces tests vérifient que la méthode remove() fonctionne correctement dans divers scénarios. Ces tests permettent de renforcer la robustesse et la qualité du projet en détectant d’éventuelles régressions lors de futures modifications.

### Question 3

Le papier de Netflix sur le Chaos Engineering détaille une approche méthodique pour tester la résilience des systèmes distribués en production, en simulant des défaillances pour évaluer leur capacité à maintenir leurs fonctionnalités.

**Expériences concrètes :**
- Simulation de pannes, comme l'arrêt de serveurs ou des latences réseau ciblant des microservices.
- Tests sous conditions dégradées, par exemple la défaillance de services non critiques (ex: favoris) tout en préservant les fonctions essentielles (ex: streaming vidéo).
- Surcharge volontaire de certains composants pour simuler des pics de trafic imprévus.

**Exigences :**
- Outils d'observabilité (Atlas, Mantis) pour la télémétrie en temps réel.
- Mécanismes d'injection de pannes (Chaos Monkey pour les serveurs, FIT pour les microservices).
- Plateformes automatisées comme ChAP pour mener des tests sans affecter significativement les utilisateurs.

**Variables observées :**
- Indicateurs clés de performance (temps de réponse, taux d’erreur).
- Comparaison des métriques de santé entre clusters affectés et non affectés.
- Impact sur l'expérience utilisateur.

**Résultats principaux :**
- Identification des points faibles de l’architecture.
- Renforcement de la résistance du système grâce à des mécanismes de secours et des redondances.
- Amélioration des dépendances critiques et de leur gestion en cas de panne.

Netflix n'est pas la seule entreprise à utiliser le Chaos Engineering, d'autres comme Amazon, Google ou Microsoft utilisent également ces pratiques, adaptées à leurs systèmes.

Ce type d'approche pourrait être utilisé dans d'autres domaines, quelques exemples :
- Simulations de latence dans des API d'e-commerce pour tester la fiabilité du paiement.
- Pannes de bases de données dans les services financiers pour valider les mécanismes de secours.
- Interruptions réseau dans les plateformes logistiques pour évaluer la continuité du suivi des livraisons.  

Les variables observées seraient la disponibilité, les temps de réponse et la satisfaction des utilisateurs, avec des outils et des précautions similaires pour limiter les impacts.

### Question 4

La spécification formelle de WebAssembly (Wasm) offre plusieurs avantages majeurs :

**1. Précision et clarté :** La formalisation garantit une description sans ambiguïté du comportement de WebAssembly. Les règles de validation et les sémantiques d'exécution sont définies de manière concise, évitant les malentendus, contrairement à des spécifications plus complexes comme celles du bytecode de la JVM.

**2. Détection des erreurs :** Lors de l'élaboration de la spécification, l'approche formelle a permis d'identifier et de corriger des erreurs variées, qu'il s'agisse de simples coquilles ou de problèmes sémantiques complexes (ex: cas extrêmes pouvant bloquer un programme).

**3. Base pour les tests :** La spécification a permis le développement d'un interpréteur de référence et de suites de tests exhaustives, assurant la cohérence entre les implémentations dans différents environnements.

**4. Efficacité de l'implémentation :** La conception formelle simplifie la validation et la compilation en rendant ces processus linéaires, réduisant ainsi les surcoûts tout en garantissant sécurité et portabilité.

Cependant, la spécification formelle ne dispense pas des tests. Ces derniers restent indispensables pour :
- Vérifier la compatibilité et les performances des implémentations.
- Assurer l'intégration avec les environnements hôtes (comme JavaScript).
- Détecter les problèmes spécifiques à une implémentation que la formalisation seule ne peut anticiper.

En conclusion, bien que la spécification formelle renforce la fiabilité de WebAssembly, les tests pratiques demeurent essentiels pour garantir son bon fonctionnement dans des conditions réelles et variées.

### Question 5

Ce papier propose une spécification formelle mécanisée de WebAssembly à l'aide d'Isabelle, un assistant de preuve. Cette approche complète la spécification initiale et apporte plusieurs avantages :

**1. Rigueur accrue :** La spécification mécanisée garantit l'absence d'erreurs et la cohérence interne des règles et sémantiques de WebAssembly. Elle permet de prouver formellement des propriétés essentielles, comme la sécurité des types.

**2. Preuves automatisées :** Grâce à Isabelle, la spécification facilite la vérification automatique des propriétés du langage, réduisant les risques d'erreurs dans sa conception et assurant la validité des extensions ou modifications.

**3. Amélioration de la spécification initiale :** Le processus de mécanisation a permis d'identifier et de corriger des ambiguïtés et des imprécisions dans la spécification originale, affinant ainsi ses sémantiques officielles.

**4. Artéfacts dérivés :** Cette formalisation a conduit à la création d'outils vérifiés, comme :
- Un algorithme de vérification des types, sûr et complet.
- Un interpréteur d’exécution vérifié pour WebAssembly.  
  Ces outils renforcent la fiabilité des implémentations.

**5. Vérification :** L'auteur a encodé les sémantiques de WebAssembly dans Isabelle et prouvé des propriétés clés, comme la sécurité des types, en couvrant des aspects comme le contrôle de flux et la gestion mémoire.

La mécanisation améliore la confiance dans la spécification et ses implémentations, mais elle ne remplace pas les tests. Ceux-ci restent indispensables pour couvrir des scénarios pratiques liés aux environnements réels, tels que les interactions avec le matériel ou les intégrations (ex: JavaScript).

En résumé, la spécification mécanisée renforce la robustesse théorique de WebAssembly et ses outils, tandis que les tests assurent sa fiabilité dans des contextes concrets et variés.