# Rapport de Projet : Assistant IA Discord via OpenClaw
**Cours : Veille technologique**
**Volet 2 : Support informatique et IA**

---

## 1. Introduction
*Poids : 5 points | Objectif : Cadrer le projet.*

* **Contexte :** Présentation des contraintes matérielles initiales (infrastructure locale limitée) et de la nécessité de déporter les capacités de calcul vers une infrastructure cloud à faible coût (budget de moins de 10 $/mois).
* **Problématique :** Comment un développeur ou un technicien peut-il disposer d'un assistant IA personnalisé, performant et disponible en tout temps sans dépendre d'abonnements cloud onéreux ?
* **Solution proposée :** Déploiement d'une passerelle d'agents OpenClaw sur un Serveur Privé Virtuel (VPS) connecté directement à une interface Discord.

---

## 2. Explication du projet
*Poids : 10 points | Objectif : Détailler l'architecture et les choix technologiques.*

* **Architecture technique :** Description du flux de données direct : Utilisateur (Discord) $\leftrightarrow$ Bot Discord $\leftrightarrow$ Instance OpenClaw (VPS Hostinger) $\leftrightarrow$ API de LLM distants (DeepSeek / OpenAI).
* **Justification de l'infrastructure :** * Choix du VPS (Hostinger) pour assurer une disponibilité 24/7 et contourner les limites de la machine locale.
  * Utilisation de Docker pour la conteneurisation et la facilité de déploiement.
  * Rôle central d'OpenClaw en tant que gestionnaire d'agents, éliminant le besoin d'outils d'orchestration tiers (comme n8n).
* **Gestion budgétaire :** Analyse du coût de l'hébergement VPS et de la consommation des jetons (tokens) d'API pour prouver le respect du budget de 10 $.

---

## 3. Explication des fonctionnalités
*Poids : 10 points | Objectif : Décrire ce que fait concrètement le système.*

* **Interface de communication :** Interaction textuelle naturelle avec l'IA directement depuis l'application Discord (via des mentions ou des canaux dédiés).
* **Gestion des agents et des rôles :** Explication de la configuration des invites système (*system prompts*) dans OpenClaw pour spécialiser le comportement de l'IA (ex. : mode expert en support Linux Fedora, mode réviseur de code).
* **Disponibilité et résilience :** Gestion de la persistance du bot grâce aux politiques de redémarrage automatique de Docker (`unless-stopped`) sur le VPS.

---

## 4. Conclusion
*Poids : 5 points | Objectif : Bilan et perspectives.*

* **Synthèse :** Résumé de l'adéquation entre les objectifs initiaux et la démonstration finale.
* **Retombées :** Valeur ajoutée de ce projet pour ton quotidien de développeur (centralisation de l'aide technique, réduction des coûts).
* **Perspectives d'évolution :** Possibilité d'intégrer une base de connaissances locale (RAG léger) ou d'étendre les capacités d'automatisation des agents OpenClaw.

---

## 5. Référence / Médiagraphie

[1] Hostinger, « VPS Hosting Plans », *Hostinger VPS*, 2026. [En ligne]. Disponible sur: https://www.hostinger.com/vps-hosting.

[2] Docker Documentation, « Install Docker Engine », *Docker Docs*, 2026. [En ligne]. Disponible sur: https://docs.docker.com/engine/install/.

[3] Docker Documentation, « Docker Compose overview », *Docker Docs*, 2026. [En ligne]. Disponible sur: https://docs.docker.com/compose/.

[4] OpenClaw, « OpenClaw: An open-source OpenAI API compatible gateway », *GitHub Repository*, 2026. [En ligne]. Disponible sur: https://github.com/openclaw/openclaw.

[5] Discord Developer Portal, « Applications and Bot Management », *Discord Developers*, 2026. [En ligne]. Disponible sur: https://discord.com/developers/applications.