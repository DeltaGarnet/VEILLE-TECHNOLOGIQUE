# Proposition de projet : Analyse de performance et optimisation de LLM locaux sur matériel grand public

**Session :** Hiver 2026  
**Thème :** L’IA au service du développeur  
**Volet :** Volet 2 – Support informatique et IA

---

## Introduction
L'essor des modèles de langage de grande taille (LLM) transforme radicalement la productivité des développeurs. Toutefois, la dépendance aux API cloud pose des défis en matière de confidentialité, de coûts et de disponibilité. Ce projet s'inscrit dans le thème « L’IA au service du développeur » en explorant la viabilité de l'exécution locale de modèles de pointe sur une configuration matérielle intermédiaire. L'objectif est de déterminer si un environnement local peut offrir une alternative crédible pour des tâches de codage et de traitement de données complexes.

## Prérecherche

Conformément aux étapes d'exploration, trois concepts ont été envisagés avant de fixer le choix final:

1.  **API de traduction technique avec OpenSpec :** Création d'un service de traduction gérant des glossaires spécifiques via une approche *Specification Driven Development*. Bien que pertinent, ce projet s'éloignait de l'aspect infrastructure.
2.  **Génération automatisée de documentation avec GSD :** Utilisation de l'outil *Get-Shit-Done* pour maintenir les README de dépôts GitHub. Ce projet était axé sur le flux de travail logiciel pur.
3.  **Implémentation et analyse de performance d'un LLM local (Choix retenu) :** Mise en place d'un serveur d'inférence privé pour tester des modèles comme **Gemma 4** et **Qwen 3.5**.

### Justification du choix
Le choix s'est porté sur l'implémentation locale en raison de la valeur stratégique de l'autonomie technologique. Tester les limites d'une **GTX 1070 (8 Go VRAM)** couplée à **32 Go de RAM** permet d'évaluer concrètement la faisabilité de l'IA pour un développeur indépendant ou une PME ne souhaitant pas investir dans des infrastructures serveurs coûteuses.

## Objectifs du projet
L'objectif principal est de valider la performance de modèles légers (quantifiés) face à des modèles de classe supérieure.
* **Technique :** Configurer un environnement d'inférence stable (Ollama ou LM Studio).
* **Analytique :** Comparer la vitesse (tokens/sec) et la pertinence des réponses entre Gemma 4 (31B vs 26B) et Qwen 3.5.
* **Qualitatif :** Évaluer la précision de la traduction technique avec glossaire et la génération de code.

## MVP (Minimum Viable Product)
Le produit minimum viable, réalisable en 7 jours de travail, comprendra :
* Un serveur d'inférence local fonctionnel capable de basculer entre au moins deux modèles.
* Un script de test automatisé pour mesurer la vitesse d'inférence.
* Un rapport comparatif incluant :
    * Tests de génération de code (Python).
    * Tests de traduction chinois-français avec intégration d'un glossaire de termes spécifiques.
    * Comparaison des résultats avec un modèle de référence (Claude 4.5 Sonnet).

## Méthodologie
Le projet sera segmenté en quatre étapes majeures:
1.  **Préparation (Jour 1-2) :** Installation des pilotes CUDA, configuration de l'environnement d'inférence et sélection des versions quantifiées (GGUF/EXL2) adaptées aux 8 Go de VRAM.
2.  **Tests de Performance (Jour 3-4) :** Mesures brutes de latence et de consommation de mémoire pour les modèles Gemma 4 et Qwen 3.5.
3.  **Évaluation Qualitative (Jour 5-6) :** Exécution des scénarios de traduction de romans et de débogage de code. Comparaison manuelle et assistée (via Claude 4.5 Sonnet).
4.  **Analyse et Documentation (Jour 7) :** Rédaction du rapport final et validation des objectifs.

## Outils et technologies
* **Matériel :** NVIDIA GTX 1070 (8 Go VRAM), 32 Go RAM DDR4.
* **Logiciels d'inférence :** Ollama, LM Studio ou llama.cpp.
* **Modèles :** Gemma 4 (versions 26B et 31B quantifiées), Qwen 3.5.
* **Scripts :** Python pour l'automatisation des benchmarks et la gestion du glossaire.

## Résultats attendus
À la fin de ce projet, je serai en mesure de démontrer:
* Le seuil de jouabilité d'un modèle 31B sur une carte graphique de génération précédente.
* L'efficacité de l'IA locale pour la traduction de niche (romans chinois) par rapport aux traducteurs génériques.
* Une recommandation claire sur le meilleur rapport "performance/ressources" pour un poste de travail de développeur standard.

## Utilisation de l'IA
**Avez-vous utilisé l'IA pour la rédaction de cette proposition ?** Oui.

### Annexe : Prompts utilisés
>  Crée une proposition formelle en suivant les consignes. mes 3 idées sont ainsi: implémenter et analyser les performances d'un LLM local, API de traduction technique avec gestion de glossaire avec OpenSpec, et générer des readme/documentation à des repository automatiquement avec GSD. J'ai décidé d'aller avec le LLM local parce que je crois que ça va m'être utile, je suis curieux de tester les performances et résultats de différents modèles sur mon ordi avec ma vieille gtx 1070 8go de vram 32go RAM, j'aimerais tester gemma 4 31B vs 26B comme légers modèles et peut être certains qwen 3.5. j'aimerais les tester en codage et en traduction de romans chinois avec glossaire, pour ensuite les benchmark. J'évaluerai le résultat en comparant avec la traduction officielle, moi-même et avec un plus gros modèle comme claude sonnet. (copie des consignes)