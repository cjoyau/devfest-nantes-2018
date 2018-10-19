# devfest-nantes-2018
Compte-rendu des sessions du DevFest Nantes 2018 auxquelles j'ai assisté.

# Day 1 (18/10/2018)

## Shaping your app’s architecture with Kotlin and Architecture Components

Refactoring de l'application Android [Plaid](https://github.com/nickbutcher/plaid) avec Kotlin et les composants d'architecture Android. Intéressant de voir comment cet apport permet de réduire le volume de code, le couplage et la dette technique.

**Couches cibles** :
- UI (Views)
- Domain (Use Cases)
- Data (Repositories)

## La conquête de l'espace se déroule dans votre poche

La [découverte de l'espace en VR](https://github.com/an0rak-dev/PlanetaryConquest) avec un simple smartphone.  
Comparaison des tickets d'entrée des **plateformes VR de Google** :

- **Daydream** : ~700 €, smartphone haut de gamme
- **Cardboard** : moins de 200 €, smartphone inclus

**Aspects techniques** : Utilisation des bindings du SDK Android vers OpenGL. **Problématique** : nécessité de gérer les rendu œil droit et gauche. Le SDK (via la feature VR) permet de gérer simplement la mise à jour du rendu lors de mouvements du casque.

Les **degrés de liberté** sont les mouvements permis par le matériel VR : 3 pour Cardboard (mouvements de tête), 6 pour Vive et Oculus (+ déplacements du corps). Ça se traduit en axes permettant de calculer un angle de vision (le ray tracing dont on entend parler côté cartes graphiques en ce moment).

**Limitations** : le motion sickness apparaît en dessous de 60 FPS, idéalement il faudrait 90. On est limités par la puissance (et la batterie) du téléphone. Alternative plus simple qu'OpenGL : **Unity**.

## Deliver search-friendly JavaScript-powered websites -- Un site JavaScript search-friendly !

Le **SEO** pour les applications JavaScript. Rappel des principes de base :

- **Crawling** : parcours des urls disponibles sur le site web, piloté par le robots.txt
  - **Problématique** : Les URLs sont liées à du routing JS.
- **Indexing** : codes HTTP, pondération du contenu des pages et suivi des liens
  - **Problématique** : une page de base d'une application JS ne contient pas de données, elles sont chargées en JS.
  - Pas de suivi de la navigation via des events JS (onclick etc.)

**Solutions** : Google bot.  
Indexation de 1er niveau : URLs de base. 2ème niveau : contenu chargé quand les ressources sont disponibles.  
Rediriger proprement les accès directs aux pages chargées via du JS pour éviter les 404.

**Outils de rendu dynamique** : [puppeteer](https://github.com/GoogleChrome/puppeteer) / [rendertron](https://github.com/GoogleChrome/rendertron)  
**Outils de diagnostic** : [g.co/mobile-friendly](https://g.co/mobile-friendly) / [g.co/SearchConsole](https://g.co/SearchConsole)

## L'infrastructure as code avec Terraform

Présentation de [Terraform](https://github.com/hashicorp/terraform), outil intéressant pour gérer la mise à l'échelle de son infrastructure cloud GCP, AWS, Azure, Kubernetes. Peut aussi pour gérer des outils annexes (GitHub, Jira, Grafana) via leurs API.

Terraform est utilisable via un **CLI**. Il a son propre langage de configuration (HCL) pour s'affranchir du langage propre à chaque outil. Démo faite avec Intellij, un [plugin](https://plugins.jetbrains.com/plugin/7808-hashicorp-terraform--hcl-language-support) existe pour l'autocomplétion.

Il est nécessaire de penser l'architecture en couches avant de définir la topologie, ça facilite la mise à l'échelle et évite de se retrouver avec un monolithe. Terraform peut être utilisé sur une infrastructure Cloud déjà existante, il est également possible de définir une couche d'abstraction avec des modules custom ou pré-existants disponibles sur [terraform module registry](https://registry.terraform.io/).

## Neuro Game Experience : Votre pensée prend le contrôle !

Partenariat entre le CHU, une ESN et une école. Recherche sur les douleurs fantômes après une amputation : visualiser les membres en VR permet d'apaiser le cerveau.

**Techniques** : Électro-encéphalographie P300. La machine apprend les signaux émis par le cerveau pour bouger la main et les reproduit dans un environnement VR. Pour que le traitement soit mieux accepté par le patient, utilisation d'une cabine qui scanne le corps en 3D pour générer un avatar, et utilisation de jeux (gamification).

Ces techniques seront utilisées en rééducation dans un premier temps. **Projets futurs** : utilisation de ces technos pour piloter un exosquelette, ou dans un cadre non médical piloter de la domotique.

## What Can We Learn With JavaScript Fatigue?

Que peut-on apprendre de la multitude de frameworks et librairies JS disponibles ?

**Concepts importants à garder en tête** : on choisit des technologies pour résoudre des problèmes, il faut donc se poser la question puis choisir la techno adaptée, et trouver la balance entre le coût et le gain.

**Focus sur les frameworks web** : permettent de se concentrer sur les choses qui comptent mais les technologies web bougent très vite et nécessitent de s'adapter en permanence.

**Conseil** : accepter qu'on ne peut pas tout connaître et qu'on apprend quand on est confronté à un problème. Construire des choses plutôt que suivre des tutoriels pour apprendre.

# Day 2 (19/10/2018)

## Git Dammit!

Cours didactique sur les principales difficultés rencontrées avec git, pour résoudre le fameux TDM (t'as 2 minutes ? J'ai un problème avec git). **Points importants** :

- **Soigner ses commits** pour avoir un historique lisible et faciliter les revues de code. C'est un prérequis pour les projets open source.
- **Pas de modification directe dans la branche partagée**, fonctionner par feature branch et pull request soumise à revue de code
- **Traiter les revues de code** par amend (dernier commit) et fixup (commit plus ancien) des commits d'origine, puis réécrire l'historique avec un rebase et l'option ```--autosquash```, suivi d'un force push sur votre branche personnelle
- **Utiliser les différentes options de reset** pour supprimer des commits, et bien comprendre la différence entre **fetch** (mise à jour des remote-tracking branch) et **pull** (fetch+merge)
- **Tips complémentaires** : utilisation de git diff. Splitter des modifs en plusieurs commits : ```git add -p```  
Défaire des modifs sur un seul fichier : option --, par exemple : ```git checkout aeb123 -- path/to/file```

## Istio, we have a problem! Understanding and fixing bugs with a service-mesh

Utilisation d'[Istio](https://github.com/istio/istio), un ensemble de services pour monitorer et avoir de la visibilité sur des microservices déployés sur un cluster Kubernetes. Principaux composants :

- **Service Graph** : graphe permettant de visualiser les composants déployés et les liens de communication entre eux
- **Dashboard** pour monitorer la couche applicative, basé sur [Grafana](https://github.com/grafana/grafana) et des données fournies par [Prometheus](https://github.com/prometheus/prometheus)
- **Zipkin** : outil pour rechercher / filtrer / visualiser les conversations entre services
- **Gestion du traffic** : routing du traffic basé sur des en-têtes HTTP pour faciliter le déploiement de corrections (canary deployment) ou mirroring pour reproduire l'utilisation de l'application en conditions réelles. Blue/Green deployment : routage d'un % du traffic vers une nouvelle version du service.

Peut aussi gérer des fonctionnalités de **circuit breaker** sans impacter le code, mais est moins complet que la solution [Hystrix](https://github.com/Netflix/Hystrix) de Netflix.

## Testing Out Kotlin

L'apprentissage de [Kotlin](https://github.com/JetBrains/kotlin) par les tests. Depuis l'année dernière c'est le 2ème langage officiellement supporté pour le développement Android.

Guidelines de Google :
- 70% de tests unitaires
- 20% de tests d'intégration
- 10% de tests end-to-end

Kotlin permet de définir des extensions de fonctions et des extensions de properties. Avec les mots-clés **infix** et **inline** combinés à l'utilisation de **lambdas**, le code utilisant les fonctions devient très facile à lire, surtout pour des assertions dans les tests unitaires.

## Concourse : CI/CD version 2020 !

[Présentation de Concourse](https://github.com/loganmzz/concourse-presentation-introduction) : la CI/CD à base de pipelines et de Docker imaginée par Pivotal, la société derrière le framework Spring et Cloud Foundry.

Explications sur le principe de **fan-in / fan-out**, approche différente des pipelines  en silos. Tout est basé sur de la **configuration as code** en Yaml. La solution est cloud-native, à base de conteneurs et de volumes Docker. Toute la démo des pipelines est faite en ligne de commande, l'IHM de [Concourse](https://github.com/concourse/concourse) est assez simple.

La gestion du cache est faite par tâche, ce qui peut vite devenir assez gourmand notamment avec des tâches Maven (.m2 dupliqué). **Contournement** : déclarer le cache en tant que ressource et le passer entre les tâches. On peut rentrer dans les conteneurs (hijack) pour débugger les pipelines.

## Créer un datapipeline en 20 minutes avec Kafka Connect

L'utilisation du framework **Kafka Connect** chez iAdvize pour créér des data pipelines en temps réel, scalables et résilients.  
Démo faite sur la gestion du consentement GDPR.

**Rappel** : [Kafka](https://github.com/apache/kafka) est une plateforme distribuée de streaming de données. Kafka Connect permet de streamer des données en entrée et en sortie de Kafka à l'aide de connecteurs (clés en main pour la plupart) et fournit une API REST de configuration. Une UI faite par Landoop est disponible sur GitHub : [Kafka Connect UI](https://github.com/Landoop/kafka-connect-ui).

## Détectez et trackez les aliens qui se cachent dans vos dépendances

Les **CVE** (Common Vulnerabilities and Exposures) sont répertoriés sur le site du [NIST](https://nvd.nist.gov/) du gouvernement américain. La criticité des vulnérabilités est évaluée à l'aide d'un score sur une échelle de 10 : le **CVSS** (Common Vulnerability Scoring System).

Démonstration faite avec une vulnérabilité dans **Spring Data REST** permettant de naviguer sur le file system du serveur. Utilisation de l'intégration continue (Jenkins) et de l'outil [Dependency Check](https://www.owasp.org/index.php/OWASP_Dependency_Check) fourni pas l'**OWASP** pour analyser les failles de sécurité dans les dépendances des projets.

L'outil [Dependency Track](https://www.owasp.org/index.php/OWASP_Dependency_Track_Project) permet de suivre les failles sur les projets. Une image Docker est disponible (nécessite minimum 4 Go de RAM). L'outil s'interface avec les jobs Jenkins et peut pousser des notifications dans Slack et Teams.
