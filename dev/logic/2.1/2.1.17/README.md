# 🚀 Awtrix Omni-Logique Framework (v2.1.17)

Ce framework modulaire pour matrices Awtrix centralise toute l'intelligence d'affichage. Il agit comme un processeur : il récupère l'état de vos capteurs Home Assistant, applique une logique métier (Presets) et diffuse le résultat sur vos écrans.

## 📦 Installation
1.  **Médias** : Copiez le contenu du dossier `/images` de ce repo sur votre matrice Awtrix.
2.  **Sensor** : Installez le sensor de discovery `awtrix_topics_template.yaml` (v2.1.17).
3.  **Moteur** : Déployez le script moteur `script_awtrix_moteur_v2.1.17.yaml`.
4.  **Exemples** : Inspirez-vous du fichier `automation_multimedia.yaml` pour créer vos premières règles.

---

## 🛠️ Nouveautés v2.1.17
-   **Mode Parallèle** : Supporte l'envoi simultané de plusieurs messages (Multi-room, alertes critiques).
-   **Anti-Idle Absolu** : Nettoyage immédiat des Custom Apps dès que le média n'est plus en lecture.

---

## 🌡️ Presets Intelligents (Logique Métier)

### 🎵 Multimédia
Formate automatiquement l'affichage selon le type de contenu :
- **Musique** : `Artiste - Titre` (Icône `music`).
- **Séries TV** : `Nom Série S01E08 - Titre` (Icône `tv_movie`).
- **Films** : `Titre` (Icône `Movie`).

### 🌡️ Thermomètre
- Détection d'unité, arrondi à 1 décimale et couleurs dynamiques.

### ⚙️ Générique
Mode neutre (blanc). Affiche un message de bienvenue "Awtrix Omni-Logique" en arc-en-ciel si lancé à vide.

---

## 🛡️ Intelligence & Santé
- **Auto-Discovery** : Plus besoin de configurer les topics MQTT.
- **Health Check** : Ne diffuse que si la matrice est allumée.
- **Auto-Clear** : Envoie un payload vide `{}` pour nettoyer les Custom Apps proprement.
