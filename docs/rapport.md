# Rapport de Projet : *kuchiguse* - Génération automatisée de decks Anki en japonais

**Cours :** Veille technologique - Volet 2 : Support informatique et IA  
**Date :** Juin 2026

---

## Table des matières

1. [Introduction](#1-introduction)
2. [Explication du projet](#2-explication-du-projet)
3. [Explication des fonctionnalités](#3-explication-des-fonctionnalités)
4. [Conclusion](#4-conclusion)
5. [Référence / Médiagraphie](#5-référence--médiagraphie)

---

## 1. Introduction

Dans le domaine de l'apprentissage des langues, la méthode AJATT (*All Japanese All The Time*) et son dérivé MIA (*Mass Immersion Approach*) prônent une exposition intensive et naturelle à la langue cible à travers des contenus authentiques. L'une des pratiques centrales de cette approche est le *sentence mining* : l'extraction de vocabulaire inconnu depuis des sources réelles - notamment des vidéos YouTube - afin de créer des cartes mémoire dans le logiciel Anki [4]. Ce processus, bien que reconnu comme efficace, est chronophage lorsqu'il est effectué manuellement.

Ce projet s'inscrit dans une démarche personnelle d'automatisation de cet apprentissage. Dans un premier temps, le projet visait à développer un assistant IA via Discord — un prototype sans nom pour lequel j'envisageais d'utiliser le logiciel OpenClaw — déployé sur un Serveur Privé Virtuel (VPS), afin de contourner les limites du matériel local dans un budget inférieur à 10 $/mois. À la suite d'une réévaluation des priorités, le projet a pivoté vers *kuchiguse* : un outil Python développé en parallèle à titre personnel, jugé plus représentatif des compétences acquises et mieux ancré dans les réalités concrètes d'un apprenant autonome en 2026.

La problématique est la suivante : **comment automatiser entièrement la production d'un deck Anki de vocabulaire japonais de qualité professionnelle - depuis l'extraction brute de sous-titres YouTube jusqu'à l'exportation d'un paquet `.apkg` prêt à l'importation - sur un environnement à ressources modestes ?**

---

## 2. Explication du projet

### 2.1 Pivot de projet : du prototype à *kuchiguse*

Le projet initial avec OpenClaw proposait de relier des utilisateurs Discord à des modèles de langage distants (DeepSeek, OpenAI) via une passerelle d'agents hébergée sur un VPS. L'architecture reposait sur Docker Compose pour la conteneurisation, assurant une disponibilité 24/7. Ce cadre restait pertinent pour illustrer des notions de déploiement cloud et de gestion d'agents IA, mais sa démonstration finale s'est avérée limitée dans sa portée pratique.

*kuchiguse*, développé en dehors du cadre scolaire, répondait à un besoin concret et documenté : automatiser la méthode AJATT. Ce pivot a permis de présenter un codebase plus mature, une démonstration plus fonctionnelle, et une chaîne technique plus riche à analyser.

### 2.2 Architecture du pipeline

*kuchiguse* suit un pipeline séquentiel en sept étapes :

1. **Scraping** - yt-dlp [8] liste l'ensemble des vidéos d'une chaîne en mode *flat-playlist*, sans télécharger les médias.
2. **Échantillonnage uniforme** - N vidéos (par défaut 150) sont sélectionnées de façon équidistante sur toute la durée de vie de la chaîne, garantissant un profil lexical représentatif de l'ensemble du corpus du créateur, et non de sa période récente uniquement.
3. **Extraction des sous-titres** - yt-dlp récupère les sous-titres automatiques japonais générés par l'ASR de YouTube, les seuls disponibles pour les créateurs ciblés (contenu non scripté sans sous-titres manuels).
4. **Lemmatisation NLP** - fugashi [3], un wrapper Python de MeCab, tokenise chaque phrase en morphèmes et réduit chaque token à sa forme de dictionnaire grâce au dictionnaire UniDic. Cela permet, par exemple, d'apprendre un verbe conjugué sous sa forme infinitive.
5. **Déduplication persistante** - un fichier `archive.txt` conserve la liste de tous les lemmes déjà exportés lors des exécutions précédentes. Aucun doublon n'est jamais réexporté, même entre deux sessions distinctes.
6. **Enrichissement** - chaque lemme est enrichi : définition bilingue via jamdict [7], accent tonal (*pitch accent*) depuis le CSV kanjium, et synthèse vocale via edge-tts [5] ou Voicevox [6].
7. **Exportation** - genanki [1] génère un fichier `.apkg` autonome, contenant le modèle de carte, les champs, les médias audio et les graphiques d'accent rendus en HTML/CSS.

### 2.3 Distribution et compatibilité

L'outil cible Linux (Fedora, Ubuntu, Arch), ainsi que macOS. Un script `setup.sh` avec détection automatique du gestionnaire de paquets (`dnf`, `apt`, `pacman`, `brew`), un `Makefile` et un `pyproject.toml` permettent une installation reproductible. Le projet est conçu pour fonctionner sans GPU dédié, ce qui le rend adapté à du matériel de consommation courante.

---

## 3. Explication des fonctionnalités

### 3.1 Analyse de fréquence et filtrage du vocabulaire

Les sous-titres extraits par yt-dlp [8], et convertis si nécessaire par ffmpeg [9], sont soumis à MeCab via fugashi [3] et UniDic. Chaque token japonais est réduit à son lemme canonique, et une liste de fréquences est construite sur l'ensemble du corpus échantillonné. Les mots présents dans le deck Kaishi 1.5k [2] - vocabulaire fondamental considéré comme déjà acquis - sont automatiquement exclus de l'export. L'`archive.txt` assure la continuité entre les sessions : une fois un mot appris et archivé, il n'est plus jamais reproposé, quelle que soit la source analysée.

### 3.2 Enrichissement des cartes mémoire

Chaque entrée du deck reçoit les données suivantes :

- **Définition bilingue** - jamdict [7] interroge une base JMDict locale pour fournir lecture, sens en anglais et type grammatical.
- **Accent tonal (*pitch accent*)** - extrait depuis le CSV kanjium et rendu sous forme de graphique entièrement en HTML/CSS *inline*, sans dépendance à des polices externes ni à du JavaScript. Cette approche, inspirée des gabarits Yomichan/Rikaitan, garantit un rendu identique sur Anki Desktop, AnkiDroid et AnkiMobile.
- **Synthèse vocale** - edge-tts [5] est utilisé par défaut : il s'appuie sur l'infrastructure de Microsoft Edge TTS sans nécessiter de clé API. Voicevox [6] - un moteur TTS japonais à haute qualité phonétique, entièrement local - est proposé en alternative pour les utilisateurs souhaitant une prononciation plus naturelle et autonome du réseau. L'audio WAV produit par Voicevox est converti en MP3 via un pipe ffmpeg [9], sans fichier temporaire sur disque.

### 3.3 Format des cartes et exportation `.apkg`

Les cartes reproduisent la structure visuelle du deck Kaishi 1.5k (modulé pour l'écoute seule) [2] : recto avec l'audio seul (apprentissage par l'écoute), verso révélant l'écriture en kanji, la lecture en kana, le graphique d'accent et la définition. Le fichier `.apkg` produit par genanki [1] est un paquet Anki natif et autonome, directement importable sans configuration additionnelle. Les médias audio sont intégrés dans le paquet.

### 3.4 Généricité de l'outil

Le script est conçu pour fonctionner avec n'importe quelle chaîne YouTube proposant des sous-titres automatiques japonais. La totalité de la configuration se centralise dans `config.py` (URL de la chaîne, nombre de vidéos à échantillonner, moteur TTS, filtres actifs, etc.), permettant à tout utilisateur de générer un deck personnalisé depuis ses propres sources d'immersion sans modifier le code source.

---

## 4. Conclusion

*kuchiguse* répond directement à la problématique posée : automatiser la production d'un deck Anki de vocabulaire japonais adapté aux pratiques AJATT/MIA, sans intervention manuelle lors de l'extraction ou de l'enrichissement. Le pivot depuis *OpenClaw* vers ce projet a permis de livrer une démonstration fonctionnelle plus aboutie, ancrée dans un cas d'usage réel et personnel.

Sur le plan technique, le projet illustre la maîtrise de plusieurs domaines : pipeline de traitement de données textuelles, traitement automatique du langage naturel (MeCab/UniDic), intégration de bibliothèques tierces, synthèse vocale locale et distante, et génération de formats propriétaires (`.apkg`, HTML/CSS inline pour le rendu d'accents tonals). Il témoigne également d'une veille technologique active : choix raisonné des outils, connaissance des formats Anki, et compréhension des pratiques de la communauté d'apprenants autonomes.

À terme, le projet pourrait évoluer vers un segment audio pour la phrase entière, l'intégration d'un moteur de sélection sémantique des mots les plus pertinents selon le niveau de l'apprenant, ou la publication en logiciel libre avec documentation complète pour bénéficier à l'ensemble de la communauté apprenant le japonais par immersion.

---

## 5. Référence / Médiagraphie

[1] K. Staley, "genanki: A Python library for generating Anki decks," GitHub repository, 2026. [Online]. Available: https://github.com/kerrickstaley/genanki. Accessed: Jun. 7, 2026.

	- Bibliothèque Python pour exporter des paquets Anki (.apkg).

[2] donkuri, "Kaishi 1.5k: A modern Japanese Anki starter deck," GitHub repository, 2026. [Online]. Available: https://github.com/donkuri/kaishi. Accessed: Jun. 7, 2026.

	- Deck de référence pour le design visuel des cartes.

[3] P. McCann, "fugashi: A Cython MeCab wrapper for Python," GitHub repository, 2026. [Online]. Available: https://github.com/polm/fugashi. Accessed: Jun. 7, 2026.

	- Wrapper Python pour MeCab (analyse morphologique et lemmatisation du japonais).

[4] TheMoeWay, "TheMoeWay Guide: Immersion-based Japanese learning," learnjapanese.moe, 2026. [Online]. Available: https://learnjapanese.moe/guide/. Accessed: Jun. 7, 2026.

	- Guide sur l'immersion (AJATT/MIA) et le sentence mining.

[5] rany2, "edge-tts: Use Microsoft Edge's online text-to-speech from Python without an API key," GitHub repository, 2026. [Online]. Available: https://github.com/rany2/edge-tts. Accessed: Jun. 7, 2026.

	- Outil TTS en ligne (Microsoft Edge) utilisable sans clé API.

[6] VOICEVOX Project, "voicevox_engine: High-quality Japanese neural TTS engine," GitHub repository, 2026. [Online]. Available: https://github.com/VOICEVOX/voicevox_engine. Accessed: Jun. 7, 2026.

	- Moteur TTS japonais open-source de haute qualité, exécutable localement.

[7] Neo Concept Lab, "jamdict: Python library for JMDict and KanjiDic2," GitHub repository, 2026. [Online]. Available: https://github.com/neocl/jamdict. Accessed: Jun. 7, 2026.

	- Bibliothèque pour interroger JMDict/KanjiDic2 localement.

[8] yt-dlp contributors, "yt-dlp: A feature-rich youtube-dl fork," GitHub repository, 2026. [Online]. Available: https://github.com/yt-dlp/yt-dlp. Accessed: Jun. 7, 2026.

	- Outil pour l'extraction des sous-titres et la gestion des vidéos YouTube.

[9] FFmpeg Project, "FFmpeg: A complete, cross-platform solution to record, convert and stream audio and video," ffmpeg.org, 2026. [Online]. Available: https://ffmpeg.org. Accessed: Jun. 7, 2026.

	- Outil de conversion/transcodage audio et vidéo (p. ex. WAV → MP3).