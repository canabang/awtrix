# Plan d'Implémentation : Awtrix Framework (v2.0.0)

Ce plan définit l'architecture du framework modulaire Awtrix, optimisé pour la performance et le partage communautaire.

## 🎯 Objectifs
- **Performance** : Découverte ciblée des topics MQTT et vérification de l'état de l'écran.
- **Flexibilité** : Structure à 3 niveaux (Cœur YAML / Blueprint / UI Ready).
- **Agnosticisme** : Indépendant du préfixe MQTT utilisé.
- **Francisation** : Variables et documentation en français.

## 🧱 Architecture (3 Niveaux)
1. **Cœur (YAML)** : Script moteur + Sensor de topics (Non-éditables en UI).
2. **Interface (Blueprint)** : Pour une création facile et visuelle des applications.
3. **Usage (UI Ready)** : Automatisations stockées dans `automations.yaml` standard.

## 🛠️ Spécifications Techniques

### 1. Découverte des Topics (Sensor)
Le sensor `sensor.awtrix_topics_list` scanne les domaines `sensor` et `input_text` pour trouver les ID contenant `device_topic`. L'état renvoyé est une liste JSON des préfixes MQTT.

### 2. Variables du Moteur (Francisées)
| Variable                   | Description                                  |
| :------------------------- | :------------------------------------------- |
| `entite`                   | Entité source (sensor, binary_sensor, etc.)  |
| `preset`                   | Type de logique métier à appliquer           |
| `message_perso`            | Texte personnalisé (priorité 1)              |
| `icone_perso`              | Icône personnalisée (priorité 1)             |
| `couleur_perso`            | Couleur personnalisée (priorité 1)           |
| `seuil_bas` / `seuil_haut` | Limites pour la logique de couleur           |
| `duree`                    | Durée d'affichage de l'application           |
| `vitesse`                  | Vitesse de défilement du texte               |
| `degrade`                  | Activation/Désactivation des dégradés        |
| `rainbow`                  | Activation/Désactivation du mode arc-en-ciel |
| `position_icone`           | Position de l'icône (0: gauche, 1: droite)   |
| `reveiller`                | Réveiller l'écran si éteint (wakeup)         |
| `customapp`                | Nom de l'app (laisser vide pour Notification)|

### 3. Intelligence de Diffusion
- **Vérification d'état** : Par défaut, le moteur vérifie que `light.[nom]_matrix` est allumé avant d'envoyer.
- **Gestion du Réveil** : Si la variable `reveiller` est à `true`, le paramètre `"wakeup": true` est ajouté au payload (utile pour les notifications importantes).
- **Hiérarchie de Décision** :
  1. **Surcharge** (si une variable `_perso` est remplie).
  2. **Preset** (si un type de logique est sélectionné).
  3. **Valeur brute** (état direct de l'entité).

---

## 📚 Bibliothèque de Presets (Liste Initiale)

### 🌡️ Environnement
1. **`thermometre`** :
   - Message : `[valeur]°C`
   - Logique : Froid (<19) / Confort (19-23) / Chaud (>23).
2. **`hygrometre`** :
   - Message : `[valeur]%`
   - Logique : Sec (<40) / Confort (40-60) / Humide (>60).
3. **`qualite_air`** :
   - Message : `CO2 [valeur] ppm`
   - Logique : Vert (<800) / Orange (800-1200) / Rouge (>1200).

### 🚪 Sécurité & Présence
4. **`fenetre` / `porte`** :
   - Logique : Icône Ouverte (Rouge) / Fermée (Vert).
5. **`presence`** :
   - Logique : "Présent" / "Absent".

### ⚡ Énergie, Lumières & Appareils
6. **`batterie`** :
   - Logique : Alerte < 20% (Rouge), Plein = 100% (Vert).
7. **`energie`** :
   - Message : `[valeur] W` ou `[valeur] kW`.
   - Logique : Couleur selon intensité (paramétrable).
8. **`lumiere`** :
   - Message : `On / Off` ou `[X] Allumées`.
   - Logique : Icône ampoule dynamique (Jaune/Gris).
9. **`litiere`** :
   - Logique : Statut d'entretien (Bac/Stock).

### 🎵 Multimédia (Smart Detection)
10. **`media_player`** :
   - Logique : Recherche intelligente d'attributs (`artist` vs `series_title`).
   - Message : `Artiste - Titre` (formaté selon la source).
   - Effacement auto si `paused` ou `idle`.

---

## 📑 Étapes de réalisation
1. **[Phase 1]** : Déploiement du Sensor de topics dans `templates/`.
2. **[Phase 2]** : Déploiement du Script Moteur universel.
3. **[Phase 3]** : Création du Blueprint compagnon.
4. **[Phase 4]** : Documentation et exemples de migration.
