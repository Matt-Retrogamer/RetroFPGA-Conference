# **FPGA et Rétro Gaming : Préserver le Passé avec Précision**

![Logo de la Conférence](imgs/SGF.png)

## Table des Matières

1. [Introduction et Objectifs de la Conférence](#1-introduction-et-objectifs-de-la-conférence-2-minutes)
   - [Contexte Historique du Rétro Gaming et de l'Émulation](#contexte-historique-du-rétro-gaming-et-de-lémulation)
2. [Lexique et Concepts Clés du FPGA](#2-lexique-et-concepts-clés-du-fpga-3-minutes)
3. [FPGA vs Émulation Logicielle : Deux Approches, Deux Visions](#3-fpga-vs-émulation-logicielle--deux-approches-deux-visions-15-minutes)
   - [Émulation Logicielle : Une Approche Orientée Utilisateur](#émulation-logicielle--une-approche-orientée-utilisateur)
   - [FPGA : Une Approche Orientée Hardware](#fpga--une-approche-orientée-hardware)
   - [FPGA vs Émulation : Synthèse des Différences](#fpga-vs-émulation--synthèse-des-différences)
4. [Processus de Développement d'un Core FPGA - Exemple de Q*bert](#4-processus-de-développement-dun-core-fpga---exemple-de-qbert-15-minutes)
5. [Aspects Pratiques et Perspectives de l'Utilisateur Final](#5-aspects-pratiques-et-perspectives-de-lutilisateur-final-15-minutes)
6. [Perspectives Futures de la Technologie FPGA dans le Rétro Gaming](#6-perspectives-futures-de-la-technologie-fpga-dans-le-rétro-gaming)
7. [Conclusion](#7-conclusion)
8. [Session de Questions-Réponses](#8-session-de-questions-réponses-10-minutes)

---

## 1. Introduction et Objectifs de la Conférence (2 minutes)

### Présentation des Intervenants

- **Matt (Modérateur)** : Développeur logiciel et utilisateur expérimenté du MiSTer FPGA.
- **Pierco (Développeur de cores FPGA)** : Contributeur actif au projet MiSTer FPGA, notamment avec le développement de cores tels que celui de *Q*bert*.
- **Lars (Utilisateur du MiSTer FPGA)** : Passionné de rétro gaming, il apportera son point de vue d’utilisateur final.
- **Biggie (Chaîne YouTube Bigkam Gaming)** : Passionné d’arcade, il partagera son expérience de l'utilisation du MiSTer FPGA dans le monde de l'arcade, en se concentrant sur la connectivité, la fiabilité et la qualité pour les jeux compétitifs.

### Introduction Générale

La conférence explore comment le FPGA est utilisé pour la préservation du rétro gaming. Nous allons comparer l'approche FPGA à l'émulation logicielle, expliquer le processus de développement d'un core sur FPGA, et discuter des avantages pour les utilisateurs finaux, avec un focus particulier sur le MiSTer FPGA. L'objectif est de montrer comment le FPGA peut fournir une expérience de jeu fidèle et authentique, proche des consoles et machines d'origine.

### Contexte Historique du Rétro Gaming et de l'Émulation

Depuis les débuts du jeu vidéo, les technologies ont évolué rapidement, rendant certains matériels obsolètes et difficiles à trouver. L'émulation logicielle est apparue comme une solution pour **préserver et rendre accessibles** ces jeux anciens. Cependant, elle présente des limitations en termes de fidélité et de performance. L'arrivée des FPGA a ouvert de nouvelles perspectives pour la préservation du rétro gaming, en permettant une **reproduction matérielle précise** des systèmes originaux.

---

## 2. Lexique et Concepts Clés du FPGA (3 minutes)

### Lexique des Termes Essentiels

Pour faciliter la compréhension, voici quelques termes clés expliqués simplement :

- **FPGA (Field-Programmable Gate Array)** : Imaginez un **caméléon matériel** capable de se transformer en n'importe quel dispositif électronique. Un FPGA est un circuit intégré programmable qui peut imiter le comportement de différents matériels spécifiques, comme des consoles ou des machines d'arcade.

- **Core** : C'est le **costume** que le caméléon (FPGA) enfile pour devenir un système spécifique. Un core est un module FPGA qui implémente une machine ou un système particulier (ex. : une console ou un jeu d'arcade).

- **ROM** : Une ROM est le fichier qui contient les données originales du jeu ou de l'application, obtenu en "copiant" le contenu d'une mémoire ROM physique. Elle est injectée dans le core pour être exécutée par le FPGA.

- **PCB (Printed Circuit Board)** : C'est la **carte mère** où tous les composants électroniques sont connectés, comme le squelette d'une console ou d'une machine d'arcade.

- **CPU/RAM/ROM** : Les **organes vitaux** du système :
  - **CPU (Central Processing Unit)** : Le **cerveau** qui traite les instructions.
  - **RAM (Random Access Memory)** : La **mémoire à court terme** où sont stockées les données temporaires.
  - **ROM (Read-Only Memory)** : La **mémoire à long terme** qui contient les données permanentes, comme les jeux ou les programmes.

- **ALM (Adaptive Logic Modules)** : Nombre de blocks logiques programmables sur le FPGA (41500 sur le MiSTer FPGA).

- **BRAM (Block RAM)** : La mémoire intégrée et rapide du FPGA, utilisée pour stocker des données temporaires essentielles au fonctionnement.

- **DSP Blocks (Digital Signal Processing)** : Les **modules spécialisés** du FPGA pour effectuer des calculs mathématiques complexes, notamment pour le traitement de signaux numériques.

- **HDL (Hardware Description Language)** : Le langage utilisé pour donner des instructions FPGA sur la forme qu'il doit prendre. Des langages comme **Verilog** ou **VHDL** permettent de décrire et de modéliser le fonctionnement du hardware dans un FPGA.

- **MRA (MAME ROM Assignment)** : Un format de fichier utilisé dans le projet MiSTer pour organiser les ROMs et définir les configurations de cores arcade.

### Importance des Termes pour la Conférence

Ce lexique est essentiel pour aligner le vocabulaire entre les différents intervenants et l'audience, afin d’éviter toute confusion pendant la présentation. Les termes seront expliqués de manière accessible au fur et à mesure.

---

## 3. FPGA vs Émulation Logicielle : Deux Approches, Deux Visions (15 minutes)

### Émulation Logicielle : Une Approche Orientée Utilisateur

#### Explication du Concept

L'émulation logicielle peut être comparée à un **imitateur** qui reproduit les chansons d'un artiste sans forcément connaître sa technique vocale. Elle se concentre sur la **reproduction du résultat final** perçu par l’utilisateur. Le développeur d’un émulateur ne cherche pas nécessairement à recréer le hardware, mais plutôt à s'assurer que le jeu ou le système **fonctionne visuellement et auditivement de la même manière** que l'original, du point de vue de l’utilisateur.

#### Processus de Développement

Le développeur d'émulation logicielle commence par analyser **le rendu final** (graphismes, sons, interactions) et cherche à les reproduire à travers un logiciel moderne, quel que soit le hardware sous-jacent. Cela signifie qu’il peut :

- **Interpréter et traduire** les commandes du jeu ou du système en utilisant les capacités du CPU moderne (ordinateur ou Raspberry Pi).
- **Adapter les performances** du système d'origine en les optimisant pour que l’émulation fonctionne sur différentes configurations, en ajustant le code pour s’adapter à la puissance de calcul disponible.
- **Ne pas reproduire** les composants internes spécifiques du hardware (comme les circuits logiques, les registres ou les mémoires statiques), mais plutôt trouver des équivalents logiciels qui rendent le même résultat visuel et sonore.

#### Exemple de l'Émulation de la Super Nintendo

Dans le cas de l’émulation logicielle de la **Super Nintendo**, le développeur va s'assurer que les jeux se chargent correctement, que les graphismes et les sons sont fidèles à l'original, et que les commandes répondent bien. Cependant, certains détails du hardware (comme la latence des entrées ou les timings précis) peuvent différer légèrement en fonction des capacités du CPU moderne et des ajustements effectués pour que le jeu fonctionne correctement sur différentes plateformes.

### FPGA : Une Approche Orientée Hardware

#### Explication du Concept

Le FPGA est comme un **artisan** qui recrée une œuvre d'art en respectant chaque détail et chaque technique de l'artiste original. Il vise à reproduire **fidèlement le hardware original** d'une machine de jeu ou d'un système d'arcade, au niveau des composants physiques. Cette approche repose sur la recréation du comportement exact de chaque élément du hardware (comme les processeurs, les circuits logiques et les mémoires), pour que la machine se comporte **comme si elle était l'originale**.

Cependant, bien que le FPGA permette de viser une reproduction exacte (*cycle accurate*) du hardware, il est également possible de produire des implémentations qui ne sont pas totalement fidèles. Par exemple, certaines machines recréées en FPGA peuvent tourner plus rapidement que les originales ou produire des sons légèrement différents en fonction des limitations ou des optimisations appliquées.

#### Avantage Supplémentaire : Flexibilité des Sorties Vidéo et Audio

Un des avantages majeurs du FPGA est la **reproduction exacte des signaux vidéo et audio** d’origine. Cela permet d’utiliser à la fois des écrans modernes et des **écrans cathodiques d'époque (CRT)** sans avoir besoin de **traduire ou adapter** les signaux vidéo. Le FPGA permet de reproduire les résolutions et les timings vidéo originaux, et avec une interface adaptée, un joueur peut donc connecter un système FPGA à un CRT pour une expérience encore plus authentique. De même, les signaux audio sont gérés **directement par les cores**, sans traitement ou conversion intermédiaire, assurant une sortie sonore aussi proche que possible du matériel d'origine.

#### Processus de Développement

Le développeur FPGA part des **datasheets** et des spécifications techniques du hardware qu’il souhaite reproduire. Il va recréer chaque composant en utilisant des langages de description de matériel, comme le **Verilog** ou le **VHDL**, sans se préoccuper du résultat visuel ou sonore final avant d'avoir tout reconstitué. Ce processus se fait étape par étape :

- **Reproduction des circuits logiques** qui traitent les instructions et contrôlent les processus internes.
- **Simulation des mémoires** et autres composants périphériques.
- **Test indépendant de chaque bloc hardware** avant de les connecter aux autres éléments.

#### Découverte du Résultat Final

Une fois que tous les blocs du hardware sont en place, le développeur injecte la **ROM** originale du jeu. À ce moment-là, il découvre souvent le rendu final du jeu ou du système pour la première fois. Contrairement à un développeur d'émulateur logiciel, il n’a pas besoin de vérifier le rendu à chaque étape : il se concentre sur la recréation précise des composants matériels.

#### Exemple de la Super Nintendo en FPGA

Dans le cas de la **Super Nintendo** implémentée sur FPGA, chaque composant du hardware est recréé, y compris le CPU, les circuits graphiques et les contrôleurs. L'objectif est de reproduire le fonctionnement original de la console de manière exacte (*cycle accurate*), mais il est aussi possible que certains cores FPGA ne respectent pas totalement ces standards, notamment dans les systèmes où des optimisations ou des ajustements sont appliqués.

### FPGA vs Émulation : Synthèse des Différences

#### Approche Matérielle vs Logicielle

- **FPGA** : Reproduction fidèle de **chaque composant du hardware** original.
- **Émulation Logicielle** : Reproduction du **résultat final** (graphismes, sons) perçu par l'utilisateur.

#### Test et Validation

- **FPGA** : Le développeur se concentre sur la reproduction du hardware et ne teste le résultat final qu'une fois que tous les composants matériels minimum sont recréés.
- **Émulation Logicielle** : Le développeur teste régulièrement le résultat visuel et sonore au cours du développement pour s'assurer que tout fonctionne correctement.

#### Fidélité et Performance

- **FPGA** : Offre une expérience utilisateur **parfaitement fidèle** à celle d’un système d’origine, car il reproduit, idéalement, le hardware à l’identique, sans compromis.
- **Émulation Logicielle** : Peut entraîner des **latences** ou des imperfections dues à la différence entre le hardware d'origine et la plateforme moderne sur laquelle elle tourne (ex. : *input lag*, différences dans les timings).

#### Limitation Technique du FPGA

Bien que le FPGA soit excellent pour la reproduction fidèle de machines rétro comme la Super Nintendo ou la Mega Drive, il rencontre des limites techniques pour les systèmes plus modernes comme la Dreamcast ou la GameCube. Théoriquement, il est possible de recréer ces consoles sur un FPGA, mais cela nécessiterait une **quantité disproportionnée de blocs logiques** (CPU, GPU, etc.) et une carte FPGA extrêmement coûteuse. En comparaison, des émulateurs logiciels comme **Dolphin** pour la GameCube permettent de simuler ces consoles de manière efficace sur des **PC modernes bien plus abordables**, offrant un bon compromis entre fidélité et coût.

#### Complexité de Développement

- **FPGA** : Le développement FPGA est plus **technique** et nécessite une compréhension détaillée du hardware original.
- **Émulation Logicielle** : Permet d’adapter plus facilement les jeux à différentes plateformes.

---

## 4. Processus de Développement d'un Core FPGA - Exemple de Q*bert (15 minutes)

### Étapes du Développement

1. **Choix du Core**

   - **Sélection du Jeu** : Pierco commence par choisir le core à développer. Pour *Q*bert*, il s’agit de recréer fidèlement le jeu d'arcade. Ce choix est motivé par la disponibilité des ressources techniques (documentation, schémas) et l'importance du jeu dans l'histoire du rétro gaming.

2. **Recherche de Matériel (Documentation, Schémas Électroniques, PCB)**

   - **Collecte d'Informations** : Cette étape est cruciale pour comprendre comment le hardware d'origine fonctionne. Pierco recherche des **datasheets**, des **schémas électroniques**, et parfois même des images ou des scans des **PCB** du système original. Cela permet de comprendre l'interaction entre les différents composants du jeu.

3. **Recherche des Composants HDL**

   - **Cores de CPU** : Trouver ou reproduire le core du processeur utilisé dans la machine originale.
   - **Composants Personnalisés** : Recherche ou création de descriptions HDL pour les composants spécifiques utilisés dans le système (ex. : circuits logiques, contrôleurs).
   - **Dumps de PLD/PAL** : Obtenir les dumps des composants logiques programmables tels que les **PLD** (Programmable Logic Devices) ou **PAL** (Programmable Array Logic) utilisés dans le hardware original.

4. **Simulation (Verilator, Modelsim, Icarus, etc.)**

   - **Test Logiciel** : Avant de porter le core sur le FPGA, Pierco utilise des outils de simulation pour **développer** le comportement des composants logiques en **HDL**. Des logiciels comme **Verilator**, **Modelsim** ou **Icarus** permettent de simuler le fonctionnement du hardware afin de détecter d’éventuelles erreurs.

5. **Portage sur MiSTer**

   - **Implémentation sur FPGA** : Après les tests de simulation, Pierco porte le core sur la plateforme **MiSTer FPGA**. Les composants implémentés lors de la phase de simulation son intégrés aux Framework MiSTer, permettant au système d'être exécuté sous forme de core sur la plateforme.

6. **Débogage (Signal Tap)**

   - **Analyse des Signaux** : Le core est ensuite débogué pour vérifier le bon fonctionnement de tous les éléments. Des outils comme **Signal Tap** (un analyseur logique) sont utilisés pour examiner en profondeur les signaux et détecter les anomalies.

7. **Publication**

   - **Mise à Disposition** : Une fois le core testé et débogué, il est publié pour que la communauté puisse en profiter, en tant que core stable et jouable sur la plateforme MiSTer FPGA.

---

## 5. Aspects Pratiques et Perspectives de l'Utilisateur Final (15 minutes)

### Complexité du Setup

- **MiSTer FPGA** : Les solutions basées sur MiSTer FPGA sont principalement des projets "Do It Yourself", nécessitant une certaine expertise technique pour assembler les différentes pièces et installer les cores.
- **Solutions Propriétaires (ex. : Analogue)** : Offrent une expérience **plug-and-play**, sans nécessité de configuration complexe, mais souvent à un prix plus élevé.

### Coût d'Entrée

- **Investissement Financier** : Le coût d'un setup FPGA (comme MiSTer FPGA) est généralement plus élevé que celui des solutions d’émulation logicielle (ex. : Raspberry Pi) en raison du matériel spécialisé.
- **Investissement en Temps** : Nécessite du temps pour l'assemblage et la configuration, afin de bénéficier d'une expérience de jeu **fidèle à l'original**.

### Public Cible

- **Utilisateurs Exigeants** : Destiné aux passionnés de rétro gaming qui recherchent une expérience identique à celle des consoles ou machines d'arcade originales.
- **Soucieux de la Fidélité** : Pour ceux qui ne veulent aucun compromis sur la latence ou la précision du rendu.

### Expérience Utilisateur du MiSTer FPGA (par Lars et Biggie)

#### Témoignage de Lars

- **Découverte du MiSTer FPGA** : Lars partagera comment il a découvert la plateforme en cherchant une solution alliant fidélité et performance, après avoir testé plusieurs autres solutions d'émulation logicielle.
- **Premières Impressions** : Impressionné par la différence fondamentale entre les solutions FPGA et les émulations logicielles, notamment en termes de précision et de latence.
- **Comparaison avec l’Émulation** : Il expliquera comment il a testé et comparé les deux types de solutions, soulignant l’absence quasi totale de **latence** sur MiSTer FPGA. Il mentionnera que des petits décalages peuvent parfois apparaître avec l'émulation, affectant l’expérience utilisateur, surtout sur des jeux rapides et exigeants.
- **Avantages et Inconvénients**:
  - **Avantages** : Fidélité de l’expérience par rapport aux consoles d’origine, communauté active autour du projet.
  - **Inconvénients** : Coût élevé du matériel FPGA, complexité du setup par rapport à des solutions d’émulation plus accessibles.

#### Focus sur l’Arcade (par Biggie)

- **Connectivité avec les Systèmes Arcade** : Biggie discutera de la facilité avec laquelle le MiSTer FPGA peut être intégré aux systèmes arcade.
- **Fiabilité pour les Sessions Longues ou Compétitives** : Il soulignera la robustesse du MiSTer FPGA pour des sessions de jeu intensives.
- **Qualité de la Reproduction des Jeux d’Arcade** : Accent sur la précision et l’authenticité, cruciales pour les jeux compétitifs comme les *shoot 'em ups* et les jeux de combat.
- **Importance pour les Joueurs Compétitifs** : La **précision** et l'**absence de lag** sont des critères essentiels pour une expérience authentique.

### Pourquoi Choisir le MiSTer FPGA ?

- **Reproduction Fidèle du Hardware Original** : Permet une expérience de jeu authentique, proche des machines d'époque.
- **Flexibilité de la Plateforme** : Offre des cores pour une large gamme de consoles et de systèmes d'arcade.
- **Projet Open-Source** : Bénéficie d'une communauté active et de mises à jour régulières.
- **Robustesse Logicielle** : Plateforme durable avec un cycle de vie logiciel solide.

---

## 6. Perspectives Futures de la Technologie FPGA dans le Rétro Gaming

### Évolutions Possibles

- **Amélioration des FPGA** : Augmentation des capacités pour reproduire des systèmes plus modernes.
- **Accessibilité** : Réduction des coûts avec la démocratisation de la technologie.
- **Communauté Grandissante** : Plus de développeurs contribuant à la préservation.

### Impact Potentiel sur la Préservation Numérique

- **Préservation Précise** : Conservation exacte des systèmes originaux.
- **Éducation** : Outils pour l'apprentissage et la recherche en informatique et électronique.
- **Culture** : Maintien du patrimoine vidéoludique pour les générations futures.

---

## 7. Conclusion

### Résumé des Points Clés

- **Compréhension des Deux Approches** : FPGA vs Émulation Logicielle et leurs implications sur la fidélité et la performance.
- **Processus de Développement** : Aperçu détaillé de la création d'un core FPGA, illustré par l'exemple de *Q*bert*.
- **Expérience Utilisateur** : Témoignages mettant en avant les avantages du MiSTer FPGA pour les passionnés et les joueurs compétitifs.
- **Perspectives Futures** : Importance de la technologie FPGA dans la préservation du rétro gaming et son potentiel pour l'avenir.

### Pour Aller Plus Loin

- **Impliquer le Public** : Encouragement à rejoindre la communauté, à essayer le MiSTer FPGA, ou à contribuer au développement des cores.
- **Préservation Culturelle** : Insistance sur l'importance de préserver le patrimoine vidéoludique pour les générations futures.
- **Continuer l'Échange** : Invitation à poursuivre les discussions et les explorations dans ce domaine passionnant.

---

## 8. Session de Questions-Réponses (10+ minutes)

### Modération par Matt

- **Interaction avec le Public** : Matt animera la session de questions-réponses en encourageant le public à poser des questions.
- **Thématiques des Questions** :
  - **Aspects Techniques** : Développement FPGA, outils utilisés, défis rencontrés.
  - **Comparaison avec l'Émulation Logicielle** : Avantages, inconvénients, cas d'usage spécifiques.
  - **Perspectives Futures** : Évolutions technologiques, impact sur la préservation du rétro gaming.
  - **Sujets Spécifiques** : Par exemple, le développement du core *Q*bert*.
- **Réponses des Intervenants** : Pierco, Lars et Biggie apporteront leurs expertises respectives pour répondre aux questions et approfondir les sujets abordés.
- **Objectif** : Assurer que tous les aspects de la présentation sont bien compris et explorer davantage les points d'intérêt du public.

---
