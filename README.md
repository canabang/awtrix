# 📱 HA-AWTRIX-MATRIX (Standard Snapshot T5.2)

![Awtrix](https://img.shields.io/badge/HA-Awtrix-blue?style=for-the-badge&logo=home-assistant)
![T5.2](https://img.shields.io/badge/Doctrine-Snapshot_T5.2-2E8B57?style=for-the-badge)

Package universel pour la gestion dynamique des afficheurs matriciels **Awtrix** via MQTT. Ce dépôt respecte la structure normalisée Antigravity.

---

## 🏗️ ARCHITECTURE DU DÉPÔT

Le projet est organisé selon une architecture duale permettant stabilité et itération :

- **`/` (Main Branch)** : Contient les versions de production stables.
    - `script.yaml` : Moteur de diffusion MQTT universel.
    - `automatisations.yaml` : Logique métier (ex: Température).
    - `template.yaml` : Capteurs composites.
- **`/dev/` (Snapshot Branch)** : Historique des versions et fichiers agnostiques.
    - Structure : `dev/[Nature]/[Version]/[Patch]/[Fichier]_v[X.Y.Z].yaml`.
- **`/images/`** : Bibliothèque locale d'assets visuels pour l'Awtrix.

---

## 🚀 FONCTIONNALITÉS CLÉS

- 📡 **Auto-Découverte** : Identifie dynamiquement tous les appareils Awtrix du système.
- 🎭 **Double Mode** : Supporte les notifications éphémères et les applications personnalisées (custom apps).
- 🎨 **Design Premium** : Intégration native des palettes SeaGreen et gestion des dégradés.
- 🧪 **Agnosticisme Dev** : Fichiers Snapshot prêts pour la portabilité via jetons `A_CHANGER_*`.

---

## 🛠️ INSTALLATION & USAGE

### 1. Prérequis
- Intégration MQTT active sur Home Assistant.
- Appareils Awtrix avec `device_topic` dans leur `entity_id`.

### 2. Déploiement
Copiez les fichiers de la racine vers votre dossier `packages/` ou `scripts/` de Home Assistant.

### 3. Exemples d'Appel
```yaml
action: script.awtrix_dynamique_customapp
data:
  message: "Température: 22°C"
  icone: "temp_ch"
  color: "#2e8b57"
```

---

## 📜 GOUVERNANCE & MAINTENANCE
Le fichier [PROJET_RULES.md](./PROJET_RULES.md) définit les règles de nomenclature et de sécurité applicables à ce dépôt. Toute modification de la logique doit impérativement faire l'objet d'un nouveau Snapshot dans `/dev/` avant pivot vers `Main`.

---
*Développé avec ❤️ par Antigravity — Excellence Domotique.*
