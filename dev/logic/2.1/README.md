# 🚀 Awtrix Omni-Logique Framework (v2.1.11)

Ce framework modulaire pour matrices Awtrix centralise toute l'intelligence d'affichage. Il agit comme un processeur : il récupère l'état de vos capteurs Home Assistant, applique une logique métier (Presets) et diffuse le résultat sur vos écrans.

## 📦 Installation
1.  **Médias** : Copiez le contenu du dossier `/images` de ce repo sur votre matrice Awtrix ou utilisez vos propres images/icônes.
2.  **Sensor** : Installez le sensor de discovery `awtrix_topics_template.yaml` (v2.1.11).
3.  **Moteur** : Déployez le script moteur `script_awtrix_moteur_v2.1.11.yaml`.

## 🧱 Architecture en 3 Paliers
1.  **Le Moteur (Cœur YAML)** : Le cerveau qui transforme les données capteurs en messages Awtrix.
2.  **L'Interface (Blueprint)** : (À venir) Interface visuelle pour créer vos apps.
3.  **L'Usage (UI Ready)** : Vos automatisations finales, simples et propres.

---

## 💡 Fonctionnalités Clés

### ⚙️ Le processeur d'état (Entité Source)
Le script lit l'état de l'**Entité source** sélectionnée. Si aucune entité n'est fournie, il fonctionne en mode texte simple.

### 🏷️ Substitution Dynamique (Tags)
Dans le champ **Message personnalisé**, vous pouvez injecter dynamiquement l'état de votre capteur en utilisant le tag `[[valeur]]`.
*Exemple :* Si votre entité est un capteur d'humidité à 65%, écrire `Humidité : [[valeur]]` affichera `Humidité : 65%`.

### 🌡️ Presets Intelligents (Logique Métier)
Les presets automatisent la mise en forme selon le type de donnée :
- **Thermomètre** : 
    - Extrait la valeur numérique de l'entité.
    - Détecte l'unité de mesure (`°C` ou `°F`).
    - Arrondit la valeur à **1 décimale**.
    - Change la couleur dynamiquement : **Bleu** (<19°), **Vert** (Confort), **Rouge** (>23°).
- **Générique** : Affiche l'état brut de l'entité (ex: "On/Off", "Ouvert", "1240W") sans icône et en blanc par défaut. C'est le mode "page blanche" à personnaliser selon vos besoins.

---

## 🛠️ Intelligence de Diffusion & Santé

### 🛰️ Discovery Automatique
Le sensor `sensor.awtrix_topics_list` scanne votre réseau pour trouver toutes vos matrices. Vous n'avez plus besoin de renseigner manuellement vos topics MQTT.

### 🛡️ Health Check (Vérification de Santé)
Avant chaque envoi, le moteur vérifie l'état de l'entité `light.[nom]_matrix`. Si l'écran est éteint, le message n'est pas envoyé, ce qui économise les ressources de votre serveur et réduit le trafic MQTT inutile.

### ⏳ Gestion du "Lifetime" (Durée de vie)
Réglable en **Minutes**. Idéal pour les rappels qui doivent disparaître après un temps donné (ex: laisser une alerte "Poubelle" pendant 60 minutes, puis l'effacer automatiquement).

### 🗑️ Suppression d'une Application
Pour supprimer manuellement une application personnalisée de l'écran, appelez le script avec le **Nom de l'App** rempli, mais laissez l'**Entité source** et le **Message personnalisé** vides.

---

## 📖 Référence des Variables

| Variable          | Fonction                                                             |
| :---------------- | :------------------------------------------------------------------- |
| **entite**        | Le capteur Home Assistant à surveiller.                              |
| **preset**        | La "recette" logique à appliquer aux données.                        |
| **customapp**     | Nom de l'App (laisse vide pour une Notification éphémère).           |
| **message_perso** | Votre texte. Utilisez `[[valeur]]` pour y insérer l'état du capteur. |
| **icone_perso**   | Pour forcer une icône (écrase celle du preset).                      |
| **couleur_perso** | Pour forcer une couleur HEX (ex: `#2E8B57`).                         |
| **vie**           | Temps de présence sur l'écran en **minutes** avant auto-suppression. |
