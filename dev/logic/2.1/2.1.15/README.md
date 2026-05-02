# 🚀 Awtrix Omni-Logique Framework (v2.1.15)

Ce framework modulaire pour matrices Awtrix centralise toute l'intelligence d'affichage. Il agit comme un processeur : il récupère l'état de vos capteurs Home Assistant, applique une logique métier (Presets) et diffuse le résultat sur vos écrans.

## 📦 Installation
1.  **Médias** : Copiez le contenu du dossier `/images` de ce repo sur votre matrice Awtrix.
2.  **Sensor** : Installez le sensor de discovery `awtrix_topics_template.yaml` (v2.1.15).
3.  **Moteur** : Déployez le script moteur `script_awtrix_moteur_v2.1.15.yaml`.

---

## 🌡️ Presets Intelligents (Logique Métier)

### 🎵 Multimédia (Nouveau v2.1.15)
Formate automatiquement l'affichage selon le type de contenu :
- **Musique** : `Artiste - Titre` (Icône `music`).
- **Séries TV** : `Nom de la Série S01E08 - Titre de l'épisode` (Icône `plex`).
- **Films / Vidéo** : `Titre` (Icône `plex`).
- **Sécurité** : Affiche "Source manquante" en rouge si aucune entité n'est détectée.

### 🌡️ Thermomètre
- Détection d'unité (`°C`/`°F`), arrondi à 1 décimale et couleurs dynamiques (Bleu/Vert/Rouge).

### ⚙️ Générique
Mode neutre (blanc) affichant l'état brut de l'entité. Affiche un message de bienvenue "Awtrix Omni-Logique" en arc-en-ciel si lancé à vide.

---

## 🏷️ Substitution Dynamique (Tags)
Utilisez le tag `[[valeur]]` dans le champ **Message personnalisé** pour injecter l'état du capteur.
*Exemple :* `Cuisine : [[valeur]]` -> `Cuisine : 21.5°C`.

## 🛡️ Intelligence & Santé
- **Auto-Discovery** : Plus besoin de configurer les topics MQTT manuellement.
- **Health Check** : Ne diffuse que si la matrice est allumée (`light.matrix` = on).
- **Lifetime** : Auto-suppression réglable en **minutes**.
