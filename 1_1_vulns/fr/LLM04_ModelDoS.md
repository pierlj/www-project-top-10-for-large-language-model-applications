## LLM04: Déni de Service du Model

### Description

Un attaquant interagit avec un LLM de façon à consommer une quantité exceptionnellement élevée de ressources, ce qui entraîne une baisse de la qualité de service pour lui et les autres utilisateurs, ainsi que des coûts en ressource potentiellement élevés. De plus, il est possible pour un attaquant de manipuler la fenêtre de contexte d'un LLM. Ce problème devient de plus en plus préoccupant en raison de l'utilisation croissante des LLMs dans des applications diverses, de leur consommation intensive en ressources de calcul, de l'imprévisibilité des inputs des utilisateurs et d'une méconnaissance générale des développeurs envers cette vulnérabilité. La fenêtre de contexte d'un LLM représente la longueur maximale du texte que le modèle peut gérer, couvrant à la fois l'input et l'output du modèle. C'est une caractéristique cruciale des LLMs car elle fixe la taille et la complexité du texte que le modèle peut traiter à un instant t. La taille de la fenêtre de contexte est définie par l'architecture du modèle et peut varier d'un modèle à l'autre.

### Exemples de vulnérabilités

1. Envoi de requêtes créant des un grand volume de tâches dans une file d'attente, entraînant un usage réccurent de ressources, e.g. avec LangChain ou AutoGPT.
2. Emission de requêtes particulièrement coûteuses en ressources, utilisant une orthographe ou des phrases inhabituelles.
3. Saturation continue de l'input: Un attaquant envoie un flux d'input au LLM qui sature la fenêtre de contexte, forçant le modèle à consommer des ressources de calcul excessives. 
4. Inputs longs et récurrents: L'attaquant envoie de manière répétée des inputs longs au LLM, saturant sa fenêtre de contexte.
5. Expansion récursive du contexte: L'attaquant construit un input qui déclenche une expansion récursive du contexte, forçant le LLM à traiter des inputs de plus en plus longs.
6. Variable-length input flood: The attacker floods the LLM with a large volume of variable-length inputs, where each input is carefully crafted to just reach the limit of the context window. This technique aims to exploit any inefficiencies in processing variable-length inputs, straining the LLM and potentially causing it to become unresponsive.
6. Envoi massif d'inputs de longueurs variables: L'attaquant inonde le LLM avec un grand nombre de requêtes de taille variable, chacune étant conçue pour approcher ou atteindre la limite de la fenêtre de contexte. Cette technique exploite l'inefficacité du traitement d'inputs de longueurs différentes, risquant de surcharger le LLM et de le rendre inopérant.

### Stratégies de prévention et d'atténuation

1. Implémenter des processus de validation sur les inputs pour s'assurer qu'ils respectent les limites définies et pour filtrer tout contenu malveillant.
2. Limiter les ressources allouées à chaque requête, de sorte que les reqûetes plus complexes s'exécutent plus lentement.
3. Mettre en place du rate limiting sur le nombre de requêtes qu'un utilisateur ou une adresse IP unique peut effectuer dans un intervalle de temps donné.
4. Limiter le nombre d'actions en attente et le nombre d'actions totales dans un système utilisant les réponses du LLM. 
5. Contrôler de maniière continue l'utilisation des ressources du LLM pour identifier des pics anormaux qui pourraient indiquer une attaque de DoS.
6. Définir des limites strictes sur la taille des inputs en fonction de la fenêtre de contexte du LLM afin d'éviter la surcharge et l'épuisement des ressources.
7. Sensibiliser les développeurs aux vulnérabilités de type DoS des LLMs et leur fournir des directives pour une implémentation sécurisée.

### Exemples de scenarios d'attaque

1. Un attaquant envoie de manière répétée des requêtes coûteuses à un modèle hébergé, entraînant une baisse de la qualité de service pour les autres utilisateurs et des coûts en resources plus élevés pour l'hébergeur.
2. Un outil basé sur un LLM, chargé de collecter des information, rencontre une chaine de caractères sur une page web à priori inoffensive. Celle-ci déclenche une série de requêtes supplémentaires par l'outil, consommant des ressources de manière excessive.
3. Un attaquant envoie de manière continue des inputs qui dépassent la fenêtre de contexte du LLM. L'attaquant peut utiliser des scripts ou des outils automatisés pour envoyer un volume conséquent d'inputs, dépassant les capacités de traitement du LLM. En conséquence, le LLM a besoin de plus de ressources de calcul, ce qui entraîne un ralentissement notable voire une indisponibilité totale du système.
4. Un attaquant envoie une série d'inputs séquentiels au LLM, chacun étant conçu pour être juste en dessous de la taille de la fenêtre de contexte. Ainsi, l'attaquant épuise la capacité de traitement du LLM. Celui-ci peine à traiter chaque input et les ressources en calcul deviennent insuffisantes, entraînant une dégradation des performances ou un déni de service complet.
5. Un attaquant exploite les mécanismes récursifs du LLM pour déclencher une expansion du contexte de manière répétée. En construisant un input qui exploite le comportement récursif du système, l'attaquant force le modèle à consommer une quantité importante de ressources de calcul pour traiter des inputs de plus en plus longs. Cette attaque peut conduire à un déni de service en rendant le LLM inopérant ou en le faisant crasher.
6. Un attaquant envoie un grand nombre de requêtes de taille variables, conçues pour approcher la taille limite de la fenêtre de contexte du LLM. En faisant cela, l'attaquant cherche à exploiter les inefficacités du traitement d'inputs de longueurs différentes, empêchant le LLM de répondre à des requêtes légitimes.
7. While DoS attacks commonly aim to overwhelm system resources, they can also exploit other aspects of system behavior, such as API limitations. For example, in a recent Sourcegraph security incident, the malicious actor employed a leaked admin access token to alter API rate limits, thereby potentially causing service disruptions by enabling abnormal levels of request volumes.
7. Bien que les attaques de DoS visent généralement à submerger les ressources du système, elles peuvent aussi exploiter les caractéristiques du système, tels que les limitations de l'API. Par exemple, lors d'un récent incident de sécurité chez Sourcegraph, l'acteur malveillant a utilisé un token d'accès administrateur compromis pour modifier le rate limiting de l'API, ce qui a causé des perturbations de service en permettant des volumes de requêtes anormaux.

### Références

1. [LangChain max_iterations](https://twitter.com/hwchase17/status/1608467493877579777): **hwchase17 sur Twitter**
2. [Sponge Examples: Energy-Latency Attacks on Neural Networks](https://arxiv.org/abs/2006.03463): **Arxiv White Paper**
3. [OWASP DOS Attack](https://owasp.org/www-community/attacks/Denial_of_Service): **OWASP**
4. [Learning From Machines: Know Thy Context](https://lukebechtel.com/blog/lfm-know-thy-context): **Luke Bechtel**
5. [Sourcegraph Security Incident on API Limits Manipulation and DoS Attack ](https://about.sourcegraph.com/blog/security-update-august-2023): **Sourcegraph**
