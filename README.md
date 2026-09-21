# CLD Studio

**Un outil de modélisation causale interactif, pour construire, visualiser et simuler des diagrammes de boucles causales (CLD).**

Application web autonome (un seul fichier HTML), exécutée entièrement dans le navigateur, sans installation ni serveur.

---

## Sommaire

- [Qu'est-ce que l'outil](#quest-ce-que-loutil)
- [Le fonctionnement du CLD](#le-fonctionnement-du-cld)
- [Ce que l'outil permet de faire](#ce-que-loutil-permet-de-faire)
- [Le modèle 3D](#le-modèle-3d)
- [Dans quel cadre a-t-il été utilisé](#dans-quel-cadre-a-t-il-été-utilisé)
- [Comment l'utiliser](#comment-lutiliser)
- [Structure des données (Excel)](#structure-des-données-excel)
- [Déploiement en ligne](#déploiement-en-ligne)
- [Structure du dépôt](#structure-du-dépôt)

---

## Qu'est-ce que l'outil

CLD Studio permet de construire des diagrammes de boucle causale (Causal Loop Diagram) et, surtout, de les rendre **calculables** : plutôt que de rester une simple carte de flèches, chaque variable du graphe peut être reliée à de vraies données et recalculée automatiquement.

Sa spécificité par rapport aux outils de diagramme classiques (drawio, Miro, Kumu), qui restent qualitatifs, est de permettre de remplacer un lien fléché par un calcul ou une formule branchée sur un tableau de données, sans recourir au formalisme plus lourd des outils de dynamique des systèmes (Vensim, Stella). L'outil reste en développement (*work in progress*).

L'outil est générique : aucune logique métier n'est codée en dur. Le jeu de données utilisé pour l'illustrer (construction bois, isolants) n'est qu'un exemple d'application parmi d'autres — l'outil peut modéliser n'importe quel système à effets de rétroaction (santé publique, énergie, agriculture, économie circulaire, logistique…).

## Le fonctionnement du CLD

Un **lien causal** relie deux variables A → B et porte une **polarité** : il répond à la question « si A augmente, toutes choses égales par ailleurs, qu'arrive-t-il à B ? »

- **Lien positif (+)** : A et B varient dans le même sens.
- **Lien négatif (−)** : A et B varient en sens opposé.

Ce n'est pas une corrélation observée mais une hypothèse d'influence posée par le modélisateur, à tester plutôt qu'à considérer comme acquise. Un lien peut aussi porter un **délai** : l'effet sur la variable cible ne se manifeste qu'après un certain temps.

Une **boucle** est un chemin fermé de liens causaux qui relie une variable à elle-même. C'est elle, et non une variable isolée, qui explique le comportement du système dans le temps. Deux types de boucles, classés automatiquement selon la parité du nombre de liens négatifs qu'elles contiennent :

- **Boucle renforçante (R)** — nombre de liens négatifs **pair** (y compris zéro). Un changement s'amplifie à chaque tour : croissance, emballement, cercle vicieux ou vertueux.
- **Boucle équilibrante (B)** — nombre de liens négatifs **impair**. Un changement se contrecarre à chaque tour, vers une cible ou une contrainte : stabilisation, rareté, saturation.

## Ce que l'outil permet de faire

**Construction du modèle**
- Créer des variables (nom, unité, tag/catégorie, force, valeur fixe ou calculée) et des liens causaux (signe, force, délai), directement depuis l'interface.
- Importer et exporter l'ensemble du modèle via un fichier Excel structuré (feuilles Variables, Liens, Données, Fonctions, Projet).
- Suivre un fichier Excel en direct : toute modification sur le disque est rechargée automatiquement, utile en atelier collaboratif.

**Détection et exploration des boucles**
- Détection automatique de toutes les boucles fermées du graphe, classées R ou B.
- Mise en favori des boucles jugées pertinentes, avec filtre dédié pour les isoler visuellement.
- Exploration du voisinage d'une boucle par **niveaux de proximité** successifs (1, 2, 3…), pour comprendre son contexte sans noyer la lecture dans tout le graphe.
- Réorganisation automatique du graphe (par centralité, par lisibilité, par catégories/tags).

**Calcul à partir de données**
- Relier une variable à un tableau de données externe (catalogue de matériaux, scénarios…) : sa valeur est alors calculée par recherche dans ce tableau, pour une ligne sélectionnée.
- Un moteur d'**itérations** recalcule tout le modèle pas à pas, en convertissant les délais des liens en pas de calcul, pour observer une trajectoire dans le temps plutôt qu'un simple sens qualitatif.
- Changer d'hypothèse en un clic (sélectionner une autre ligne du tableau de données) recalcule instantanément toutes les variables reliées.
- Tableau des itérations et journal d'erreurs de calcul (référence manquante, boucle non convergente…).
- Vue **Récap** de toutes les variables, triées par origine de leur valeur (hypothèse, formule, donnée) et par force.

**Autres fonctionnalités**
- Recherche de variable par nom, avec centrage automatique de la vue.
- Export du graphe en PDF et du projet complet en nouveau fichier Excel.
- Guide interactif intégré (tutoriel pas à pas, rejouable à tout moment), interface en français, anglais, espagnol et allemand.

## Le modèle 3D

Un module dédié permet de visualiser la **coupe d'une paroi** (ici, un panneau à ossature bois) directement à partir du modèle CLD :

- Sélection de la composition de la paroi : parement extérieur, parement intérieur, isolant — chaque choix pouvant être lié à une ligne d'un tableau de données du modèle.
- Réglage de l'écart entre les couches pour une lecture plus claire de la coupe.
- Le bouton **Calculer** relance le calcul global du CLD (comme le ferait le module d'itérations) et met à jour à la fois la vue 3D et un panneau de résultats (variables clés : résistance thermique, coût, etc.), pour rester cohérent avec le reste du modèle.
- Vue interactive : zoom et rotation à la souris pour inspecter la coupe sous tous les angles.

Ce module permet de passer d'une abstraction (des variables et des liens) à une représentation concrète et directement interprétable de l'objet physique modélisé.

## Dans quel cadre a-t-il été utilisé

L'outil a été développé et testé sur un cas d'étude de **façade à ossature bois (FOB)** : modélisation de la chaîne complète, de la fabrication en atelier à la mise en œuvre sur chantier (choix des matériaux, transport, logistique de chantier), avec un jeu de données réel de 123 variables et 251 liens.

Ce cas a notamment servi à interroger le choix de l'isolant (paille biosourcée face à la laine de verre conventionnelle), en allant au-delà d'une simple comparaison de prix unitaire ou de bilan carbone matériau par matériau, grâce à la mise en évidence des boucles de rétroaction du système global.

## Comment l'utiliser

L'outil (`index.html`) est une page web autonome, sans installation ni serveur :

1. Ouvrir `index.html` dans un navigateur.
2. Importer un classeur de données via le menu **Fichier**, ou construire un modèle de zéro avec **+ Créer**.
3. Explorer le graphe : zoom, recherche de variable, filtre des boucles favorites, niveaux de proximité.
4. Lancer un calcul (menu **Itératif**) pour relier les variables aux données et simuler leur évolution.
5. Ouvrir **Modèle 3D** pour visualiser la coupe de paroi selon la composition choisie.

## Structure des données (Excel)

Le classeur importé structure le modèle en plusieurs feuilles :

- **Variables** : liste des variables (id, nom, unité, tag, force, valeur, formule…).
- **Liens** : liste des liens causaux (source, cible, polarité, force, délai).
- **Données — \*** : tableaux de référence (ex. catalogue d'isolants), utilisés par les formules de type recherche.
- **Fonctions** : formules reliant une variable à une colonne d'un tableau de données.
- **Projet** : métadonnées du projet (nom, boucles favorites…).

## Déploiement en ligne

Pour rendre l'outil accessible via une URL publique (ex. GitHub Pages) :

1. Placer `index.html` et le classeur de données à la racine d'un dépôt public.
2. Activer GitHub Pages dans **Settings → Pages** (branche `main`, dossier `/root`).
3. L'outil est alors accessible via `https://<utilisateur>.github.io/<dépôt>/`.

