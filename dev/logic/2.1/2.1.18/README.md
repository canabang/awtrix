# 🚀 Awtrix Omni-Logique (v2.1.18)

Ce système permet de centraliser et d'automatiser l'affichage de vos informations Home Assistant sur vos écrans Awtrix. Il traite les données de vos capteurs via des modes préconfigurés (Multimédia, Thermomètre) et gère la diffusion sur plusieurs matrices en évitant les collisions de messages.

---

## 🛡️ Architecture & Concept (Le Triangle)

Le framework repose sur une architecture robuste en 3 piliers :
1.  **Le Sensor de Discovery** : Scanne et détecte automatiquement vos matrices Awtrix sur le réseau.
2.  **Le Script Moteur** : Reçoit les données, applique les calculs (Presets) et prépare le message.
3.  **Le Broker MQTT** : Diffuse le message final vers vos écrans physiques.

---

## 🛠️ Analyse du Moteur (Fonctionnement pas à pas)

Comprendre comment le moteur traite vos informations :

1.  **Réception & Parallélisme** : Le script peut gérer jusqu'à 10 instances simultanées (mode `parallel`).
2.  **Anti-Collision (Suffixe Auto)** : Si activé, génère un nom d'application unique par appareil (ex: `media_spotify`).
3.  **Logique Métier (Presets)** : Applique le preset choisi pour nettoyer et formater les données.
4.  **Personnalisation (Overrides)** : Priorité aux réglages manuels (Icône, Couleur, Texte via `[[valeur]]`).
5.  **Santé & Diffusion** : Vérification de l'état de l'écran (`on/off`) et envoi MQTT (ou effacement si message vide).

---

## 📋 Référence des Paramètres (Fields)

### 🧩 Paramètres de Base
-   **`entite`** : ID de l'entité source (ex: `sensor.temp_salon`).
-   **`preset`** : Logique à appliquer (`generique`, `thermometre`, `multimedia`).
-   **`customapp`** : Nom de l'application. Si vide, l'affichage sera une **Notification**.
-   **`suffixe_auto`** : (Booléen) Rend le nom de l'application unique par appareil.

### 🎨 Design & Overrides
-   **`message_perso`** : Texte personnalisé (Utilisez `[[valeur]]` pour injecter la donnée du preset).
-   **`icone_perso`** : Nom de l'icône à afficher.
-   **`couleur_perso`** : Couleur du texte (HEX #FFFFFF).
-   **`rainbow`** : (Booléen) Active le mode arc-en-ciel.
-   **`degrade`** : Liste de couleurs pour un dégradé (ex: `["#FF0000","#0000FF"]`).
-   **`position_icone`** : `0` pour gauche (défaut), `1` pour droite.
-   **`fond`** : Couleur d'arrière-plan (HEX).

### ⏱️ Comportement & Vie
-   **`vitesse`** : Vitesse de défilement (Défaut: 50).
-   **`duree`** : Temps d'affichage en secondes (Défaut: 25).
-   **`repetitions`** : Nombre de cycles de défilement.
-   **`vie`** : Durée de vie du message en minutes (Auto-suppression).
-   **`reveiller`** : (Booléen) Allume l'écran si celui-ci était éteint.
-   **`son`** : Nom du fichier audio à jouer sur la matrice.

---

## 🌡️ Preset : Thermomètre Intelligent

### 🧠 Logique de Traitement
-   **Analyse d'Unité** : Le moteur récupère automatiquement l'unité définie dans Home Assistant (°C ou °F).
-   **Formatage** : Arrondi automatique à **1 décimale** pour une lisibilité optimale sur matrice LED.
-   **Colorimétrie par défaut** : 
    - 🔵 **Bleu (`#0000FF`)** : Température ≤ 19°C.
    - 🟢 **Vert (`#2E8B57`)** : Zone de confort (19°C - 23°C).
    - 🔴 **Rouge (`#FF0000`)** : Température ≥ 23°C.

### 🎨 Personnalisation des Seuils
Si vous souhaitez modifier les seuils (ex: Rose si < 17°C), utilisez un template dans le champ **`couleur_perso`** de votre automatisation :
```yaml
couleur_perso: >
  {% set val = states(trigger.entity_id) | float %}
  {% if val < 17 %} #ff00ff
  {% elif val > 25 %} #ff0000
  {% else %} #2e8b57
  {% endif %}
```

---

## 🎵 Preset : Multimédia

### 🧠 Intelligence de Parsing & Formatage
Le moteur traite dynamiquement les attributs pour éviter les valeurs `None` et garantir un affichage propre :

-   **Séries TV** :
    -   *Format* : `Nom Série S01E08 - Titre Episode`.
    -   *Zéro-Padding* : Ajoute automatiquement le "0" devant les saisons/épisodes < 10.
    -   *Icône* : Utilise l'icône animée `tv_movie`.
-   **Musique** :
    -   *Format* : `Artiste - Titre`.
    -   *Icône* : Utilise l'icône `music`.
-   **Films** :
    -   *Format* : `Titre du film`.
    -   *Icône* : `Movie` (ou `plex` via override).

### 🧹 Gestion Anti-Idle
L'application multimédia suit strictement l'état du lecteur :
1.  **Lecture (`playing`)** : L'app est diffusée normalement.
2.  **Autre état (`pause`, `idle`, `off`)** : Le script envoie un payload vide `{}` qui supprime instantanément l'application de la matrice, gardant votre boucle d'affichage propre.

---

## ⚙️ Preset : Générique
-   Affiche l'état brut de l'entité.
-   **Override conseillé** : Utilisez `message_perso: "Statut : [[valeur]]"` pour ajouter du contexte.

---

## 📦 Installation
1.  **Médias** : Copiez le contenu du dossier `/images` sur votre matrice Awtrix dans le dossier icons (via son interface web).
2.  **Discovery** : Installez le sensor `awtrix_topics_template.yaml`.
3.  **Moteur** : Déployez le script `script_awtrix_moteur_v2.1.18.yaml`.
4.  **Automation** : Copiez le modèle `automation_multimedia.yaml`.
