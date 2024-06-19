## LLM05: Vulnérabilités de la chaîne logistique

### Description

La chaine logistique d'un LLM est vulnérable à des attaques impactant l'intégrité des données d'entrainement, des modèles de ML et des plateformes de déploiement. Ces vulnérabilités peuvent mener à des résultats biaisés, des failles de sécurité, voire des défaillances complètes du système. Traditionnelement, les vulnérabilités s'appliquent aux composants logiciels, mais le Machine Learning étend cela aux modèles pré-entrainés et aux données d'entrainement fournies par des tiers, susceptibles d'être manipulés.

Enfin, les plugins d'extension pour LLMs peuvent engendre leurs propres vulnérabilités. Celles-ci sont décrites dans [LLM07 - Conception de plugins non sécurisée](InsecurePluginDesign.md), qui couvre l'écriture de plugins LLM et fournit des informations utiles pour évaluer les plugins crées par des tiers.

### Exemples de vulnérabilités

1. Vulnérabilités traditionelles des packages tiers, incluant l'utilisation de composants obsolètes ou non maintenus. 
2. Utilisation d'un modèle pré-entrainé vulnérable pour le fine-tuning.
3. Emploi de données crowdsourcées empoisonnées pour l'entrainement.
4. Usage de modèles obsolètes ou non maintenus entrainant des problèmes de sécurité.
5. Conditions générales d'utilisation et politiques de confidentialité floues de la part des fournisseurs de modèles, menant à l'utilisation de données sensibles de l'application pour l'entrainement du modèle et à l'exposition d'informations sensibles. Cela peut également s'appliquer à l'utilisation de contenu protégé par des droits d'auteur par le fournisseur de modèle.

### Stratégies de prévention et d'atténuation

1. Vérifier soigneusement les sources et fournisseurs de données, y compris les conditions générales et les politiques de confidentialité, en n'utilisant que des fournisseurs de confiance. Assurez-vous que des mesures de sécurité adéquates et auditées de manière indépendante sont en place. Vérifiez également que les politiques des fournisseurs de modèles en terme de protection des données sont alignés avec les votres. Par exemple, que vos données ne sont pas utilisées pour entrainer leurs modèles. Faites en sorte d'obtenir des assurances légales visant à empêcher l'utilisation de contenu protégé par des droits d'auteur par les mainteneurs de modèles.
2. Utilisez uniquement des plugins sûrs et assurez-vous qu'ils ont été testés pour vos besoins. [LLM07 - Conception de plugins non sécurisée](InsecurePluginDesign.md) fournit des informations complémentaires sur la manière de tester les plugins LLM non sécurisés pour atténuer les risques liés à l'utilisation de plugins tiers.
3. Comprendre et appliquer les recommendations trouvées dans [A06:2021 - Composants vulnérables et obsolètes](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/). Cela inclut le scan de vulnérabilités, la gestion et la mise à jour des composants. Appliquez également ces contrôles dans les environnements de développement ayant accès à des données sensibles.
4. Maintenir à jour un inventaire des composants en utilisant un "Software Bill of Materials" (SBOM) pour vous assurer de disposer d'une liste à jour, précise et signée, empêchant la manipulation des packages déployés. Les SBOMs peuvent être utilisés pour détecter et alerter sur de nouvelles vulnérabilités rapidement.

5. Au moment de la rédaction, les SBOMs ne couvrent pas les modèles, leurs artefacts et leurs jeux de données. Si votre application LLM utilise son propre modèle, vous devriez utiliser les meilleures pratiques MLOps et les plateformes offrant des dépôts de modèles sécurisés avec un suivi des données, des modèles et des expériences.
6. Vous devriez également utiliser la signature de modèle et de code lorsque vous utilisez des modèles et fournisseurs externes.
7. Des tests de détection d'anomalies et de robustesse face aux attaques adverses sur les modèles et les données peuvent aider à détecter les manipulations et les empoisonnements, comme discuté dans [Empoisonnement des données d'entrainement](https://github.com/OWASP/www-project-top-10-for-large-language-model-applications/blob/main/1_0_vulns/Training_Data_Poisoning.md). Idéalement, cela devrait faire partie des pipelines MLOps; cependant, ces techniques émergentes peuvent être plus simples à implémenter dans le cadre d'exercices de red teaming.
8. Mettre en place une surveillance suffisante pour détecter les vulnérabilités des composants et de l'environnement, l'utilisation de plugins non autorisés et les composants obsolètes, y compris le modèle et ses artefacts.
9. Mettre en place une politique de mise à jour pour limiter l'usage de composants obslètes ou vulnérables. Assurez-vous que l'application repose sur une version maintenue des APIs et du modèle sous-jacent.
10. Réviser et auditer régulièrement la sécurité et l'accès des fournisseurs, en vous assurant qu'il n'y a pas de changements dans leur politique de sécurité ou leurs conditions générales.

### Exemples de scénarios d'attaque

1. Un attaquant exploite une librairie Python vulnérable pour compromettre un système. Cela s'est produit lors de la première fuite de données d'Open AI.
2. Un attaquant fournit un plugin LLM pour rechercher des vols, générant de faux liens menant à des escroqueries pour les utilisateurs.
3. Un attaquant exploite le registre de packages PyPi pour tromper les développeurs d'application en téléchargeant un package compromis et en exfiltrant des données ou en escaladant les privilèges dans un environnement de développement de modèles. Cette attaque a déjà été observée.
4. Un attaquant empoisonne un modèle pré-entrainé disponible publiquement spécialisé dans l'analyse économique et la recherche sociale pour créer une backdoor générant de la désinformation et des fausses informations. Il le déploie sur une place de marché de modèles (par exemple, Hugging Face) pour que les victimes l'utilisent.
5. Un attaquant empoisonne des jeux de données disponibles publiquement pour créer une backdoor lors du fine-tuning des modèles. La backdoor favorise subtilement certaines entreprises sur différents marchés.
6. Un employé malveillant d'un fournisseur (développeur externalisé, société d'hébergement, etc.) exfiltre des données, des modèles ou du code en volant de la propriété intellectuelle.
7. Un fournisseur de LLM modifie ses conditions générales et sa politique de confidentialité pour exiger un retrait explicite de l'utilisation des données d'application pour l'entrainement du modèle, menant à l'utilisation de données sensibles pendant l'entrainement.

### Reference Links

1. [ChatGPT Data Breach Confirmed as Security Firm Warns of Vulnerable Component Exploitation](https://www.securityweek.com/chatgpt-data-breach-confirmed-as-security-firm-warns-of-vulnerable-component-exploitation/): **Security Week**
2. [Plugin review process](https://platform.openai.com/docs/plugins/review): **OpenAI**
3. [Compromised PyTorch-nightly dependency chain](https://pytorch.org/blog/compromised-nightly-dependency/): **Pytorch**
4. [PoisonGPT: How we hid a lobotomized LLM on Hugging Face to spread fake news](https://blog.mithrilsecurity.io/poisongpt-how-we-hid-a-lobotomized-llm-on-hugging-face-to-spread-fake-news/): **Mithril Security**
5. [Army looking at the possibility of 'AI BOMs](https://defensescoop.com/2023/05/25/army-looking-at-the-possibility-of-ai-boms-bill-of-materials/): **Defense Scoop**
6. [Failure Modes in Machine Learning](https://learn.microsoft.com/en-us/security/engineering/failure-modes-in-machine-learning): **Microsoft**
7. [ML Supply Chain Compromise](https://atlas.mitre.org/techniques/AML.T0010/): **MITRE ATLAS**
8. [Transferability in Machine Learning: from Phenomena to Black-Box Attacks using Adversarial Samples](https://arxiv.org/pdf/1605.07277.pdf): **Arxiv White Paper**
9. [BadNets: Identifying Vulnerabilities in the Machine Learning Model Supply Chain](https://arxiv.org/abs/1708.06733): **Arxiv White Paper**
10. [VirusTotal Poisoning](https://atlas.mitre.org/studies/AML.CS0002): **MITRE ATLAS**
