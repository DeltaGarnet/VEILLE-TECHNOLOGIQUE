# Proposition de projet : Assistant IA Discord via OpenClaw

### Synopsis du projet
L'objectif de ce projet est de mettre en place un chatbot IA privé et personnalisé accessible directement depuis un serveur Discord personnel. Le système repose sur le déploiement d'une instance OpenClaw sur un serveur privé virtuel (VPS) à faible coût, configurée pour orchestrer les requêtes entre l'interface Discord et des API de modèles de langage (LLM) distantes et économiques, offrant ainsi un outil de support et d'assistance disponible en tout temps.

---

### Explication des liens et ressources techniques

Pour mener à bien ce projet, quatre axes documentaires et techniques sont nécessaires :
1. **L'infrastructure d'hébergement (VPS) :** Le choix de l'hébergeur est crucial pour respecter le budget de moins de 10 $ par mois tout en garantissant une disponibilité de calcul pour le serveur OpenClaw. Hostinger propose des solutions VPS d'entrée de gamme adaptées à ce type de projet léger.
2. **La conteneurisation (Docker) :** Pour installer OpenClaw proprement sur l'architecture Linux du VPS sans conflits de dépendances, Docker et son outil de déploiement Docker Compose sont indispensables.
3. **La passerelle d'IA (OpenClaw) :** C'est le cœur logiciel du projet. La documentation officielle permettra de configurer le fichier de routage des requêtes et de l'utiliser comme gestionnaire d'agents.
4. **L'interface utilisateur (Discord API) :** Le portail des développeurs de Discord est le passage obligatoire pour créer le bot, générer ses accès de sécurité et lui permettre de lire et répondre aux messages textuels.

---

### Références documentaires (Norme IEEE)

[1] Hostinger, « VPS Hosting Plans », *Hostinger VPS*, 2026. [En ligne]. Disponible sur: https://www.hostinger.com/vps-hosting.

[2] Docker Documentation, « Install Docker Engine », *Docker Docs*, 2026. [En ligne]. Disponible sur: https://docs.docker.com/engine/install/.

[3] Docker Documentation, « Docker Compose overview », *Docker Docs*, 2026. [En ligne]. Disponible sur: https://docs.docker.com/compose/.

[4] OpenClaw, « OpenClaw: An open-source OpenAI API compatible gateway », *GitHub Repository*, 2026. [En ligne]. Disponible sur: https://github.com/openclaw/openclaw.

[5] Discord Developer Portal, « Applications and Bot Management », *Discord Developers*, 2026. [En ligne]. Disponible sur: https://discord.com/developers/applications.