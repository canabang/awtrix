# 🚀 Awtrix Omni-Logique Framework (v2.1.16)

Ce framework modulaire pour matrices Awtrix centralise toute l'intelligence d'affichage. Il agit comme un processeur : il récupère l'état de vos capteurs Home Assistant, applique une logique métier (Presets) et diffuse le résultat sur vos écrans.

## 📦 Installation
1.  **Médias** : Copiez le contenu du dossier `/images` de ce repo sur votre matrice Awtrix.
2.  **Sensor** : Installez le sensor de discovery `awtrix_topics_template.yaml` (v2.1.16).
3.  **Moteur** : Déployez le script moteur `script_awtrix_moteur_v2.1.16.yaml`.

---

## 🌡️ Presets Intelligents (Logique Métier)

### 🎵 Multimédia (v2.1.16)
Formate automatiquement l'affichage selon le type de contenu :
- **Musique** : `Artiste - Titre` (Icône `music`).
- **Séries TV** : `Nom Série S01E08 - Titre` (Icône `tv_movie`).
- **Films** : `Titre` (Icône `Movie`).
- **Comportement** : L'application s'efface automatiquement si la lecture est mise en **Pause** ou **Arrêtée**. Seul l'état `playing` est diffusé.

### 🌡️ Thermomètre
- Détection d'unité, arrondi à 1 décimale et couleurs dynamiques.

### ⚙️ Générique
Mode neutre (blanc). Affiche un message de bienvenue "Awtrix Omni-Logique" en arc-en-ciel si lancé à vide.

---

## 🛡️ Intelligence & Santé
- **Auto-Discovery** : Plus besoin de configurer les topics MQTT.
- **Health Check** : Ne diffuse que si la matrice est allumée.
- **Auto-Clear** : Envoie un payload vide `{}` pour nettoyer les Custom Apps en fin de lecture.
