# 📱 Script Awtrix Universal - Home Assistant

Un script polyvalent pour envoyer des notifications et créer des applications personnalisées sur tous vos appareils Awtrix via Home Assistant.

## 🌟 Fonctionnalités

- ✅ **Détection automatique** de tous les appareils Awtrix
- ✅ **Diffusion simultanée** sur plusieurs appareils
- ✅ **Deux modes** : notifications temporaires ou applications personnalisées
- ✅ **Personnalisation complète** : couleurs, icônes, effets, vitesse
- ✅ **Interface utilisateur** intégrée dans Home Assistant
- ✅ **Documentation intégrée** avec descriptions détaillées

## 📋 Prérequis

- Home Assistant avec intégration MQTT configurée
- Un ou plusieurs appareils Awtrix connectés via MQTT
- Les appareils Awtrix doivent avoir `device_topic` dans leur entity_id
<img width="679" height="693" alt="image" src="https://github.com/user-attachments/assets/847089ec-e52d-41c4-a216-2c61c0e37529" />
<img width="425" height="786" alt="image" src="https://github.com/user-attachments/assets/c32ac27f-232c-4005-8d55-b5079a1132e3" />


## 🚀 Installation

### 1. Copier le script

Copiez le contenu du fichier `script.yaml` dans votre configuration Home Assistant :

**Option A : Via l'interface web**
1. Allez dans `Paramètres` → `Automatisations et scènes` → `Scripts`
2. Cliquez sur `+ CRÉER UN SCRIPT`
3. Passez en mode YAML et collez le contenu

**Option B : Via fichier de configuration**
```yaml
# Dans votre scripts.yaml ou configuration.yaml
script:
  awtrix_dynamique_customapp:
    # [Collez ici le contenu du script]
```

### 2. Redémarrer Home Assistant

Redémarrez Home Assistant pour charger le nouveau script.

### 3. Vérification

Le script apparaîtra dans :
- `Outils de développement` → `Services` → `script.awtrix_dynamique_customapp`

## 📖 Utilisation

### Mode Notification (temporaire)

Envoi d'une notification qui disparaît automatiquement :

```yaml
# Exemple d'automatisation
action:
  - action: script.awtrix_dynamique_customapp
    data:
      message: "Température: 22°C"
      icone: "thermometer"
      color: "#00ff00"
      duree: "10"
```

### Mode Application Personnalisée

Création/mise à jour d'une app permanente sur l'écran :

```yaml
action:
  - action: script.awtrix_dynamique_customapp
    data:
      customapp: "meteo"
      message: "Ensoleillé 25°C"
      icone: "sun"
      color: "#ffaa00"
      rainbow: "true"
```

### Suppression d'une Application

Pour supprimer une application personnalisée :

```yaml
action:
  - action: script.awtrix_dynamique_customapp
    data:
      customapp: "meteo"
      message: " "  # Un simple espace
```

## ⚙️ Paramètres

| Paramètre | Type | Défaut | Description |
|-----------|------|--------|-------------|
| `customapp` | Texte | - | Nom de l'application (vide = notification) |
| `message` | Texte | - | Texte à afficher |
| `icone` | Texte | `warning` | Nom ou numéro de l'icône |
| `color` | Texte | `#2e8b57` | Couleur hexadécimale ou RGB |
| `rainbow` | Texte | `false` | Effet arc-en-ciel (true/false) |
| `scrollspeed` | Texte | `50` | Vitesse de défilement (1-100) |
| `degrade` | Texte | - | Dégradé de 2 couleurs |
| `duree` | Texte | `25` | Durée d'affichage en secondes |

## 💡 Exemple d'usage

### Affichage automatique de température avec code couleur

Voici un exemple concret d'automatisation utilisant le script pour afficher la température d'une chambre avec un système de couleurs intelligent :

```yaml
alias: customapp temp_cham
description: ""
triggers:
  - entity_id:
      - sensor.temperature_chambre
    id: tempcham
    trigger: state
conditions: []
actions:
  - variables:
      # Constantes définies une seule fois
      seuil_chaud: 23
      seuil_confortable: 19
      couleur_chaud: "#8b0000"
      couleur_confortable: "#2e8b57"
      couleur_froid: "#0000ff"
      
      temp_value: "{{ states('sensor.temperature_chambre') | float }}"
      temp_cham: "{{ 'Chambre ' + (temp_value|round(1))|string + '°C' }}"
      color: >
        {% if temp_value >= seuil_chaud %}
          {{ couleur_chaud }}
        {% elif temp_value >= seuil_confortable %}
          {{ couleur_confortable }}
        {% else %}
          {{ couleur_froid }}
        {% endif %}
  - action: script.awtrix_dynamique_customapp ### à adapter si besoin ###
    data:
      icone: temp_ch
      rainbow: "false"
      scrollspeed: "50"
      color: "{{ color }}"
      duree: "25"
      customapp: temp_cham
      message: "{{ temp_cham }}"
mode: single
```

**Ce que fait cette automatisation :**

1. **Déclenchement** : Se lance à chaque changement de `sensor.temperature_chambre`
2. **Variables** : Définit les seuils de température et couleurs associées
   - 🔥 **Chaud** (≥23°C) : Rouge foncé `#8b0000`
   - 🌿 **Confortable** (19-22°C) : Vert océan `#2e8b57` 
   - 🧊 **Froid** (<19°C) : Bleu `#0000ff`
3. **Affichage** : Crée une application personnalisée "temp_cham" sur tous les écrans Awtrix
4. **Format** : "Chambre XX.X°C" avec la couleur appropriée

**Résultat visuel :**
- Texte défilant avec l'icône `temp_ch`
- Couleur adaptée automatiquement selon la température
- Mise à jour instantanée lors des changements

## 🎯 Adaptations possibles

### Personnaliser les seuils de température

```yaml
# Modifier les seuils selon vos préférences
seuil_chaud: 25        # Au lieu de 23
seuil_confortable: 20  # Au lieu de 19
```

### Changer les couleurs

```yaml
# Utiliser d'autres couleurs
couleur_chaud: "#ff4500"        # Orange rouge
couleur_confortable: "#32cd32"  # Vert lime
couleur_froid: "#4169e1"        # Bleu royal
```

### Adapter pour d'autres capteurs

```yaml
# Exemple pour le salon
triggers:
  - entity_id:
      - sensor.temperature_salon  # Changez le capteur
    trigger: state

# Dans les variables
temp_value: "{{ states('sensor.temperature_salon') | float }}"
temp_message: "{{ 'Salon ' + (temp_value|round(1))|string + '°C' }}"

# Dans l'action
data:
  customapp: temp_salon  # Changez le nom de l'app
  message: "{{ temp_message }}"
```

## 🔧 Personnalisation avancée

### Couleurs personnalisées

```yaml
# Couleurs hexadécimales
color: "#ff0000"  # Rouge
color: "#00ff00"  # Vert
color: "#0000ff"  # Bleu

# Couleurs RGB (array)
color: "[255, 0, 0]"     # Rouge
color: "[0, 255, 0]"     # Vert
color: "[0, 0, 255]"     # Bleu

# Couleurs nommées
color: "seagreen"
color: "orange"
color: "purple"
```

### Effets visuels

```yaml
# Arc-en-ciel
rainbow: "true"

# Dégradé de couleurs
degrade: "#ff0000,#0000ff"  # Rouge vers bleu

# Vitesse de défilement
scrollspeed: "20"   # Très lent
scrollspeed: "50"   # Normal
scrollspeed: "100"  # Très rapide
```

## 🎯 Autres cas d'usage possibles

En utilisant la même logique que l'exemple de température, vous pouvez créer des automatisations pour :

- **Humidité** avec seuils sec/optimal/humide
- **Qualité de l'air** avec code couleur selon les PPM
- **Niveau de batterie** des appareils avec alertes
- **Statut de présence** avec couleurs selon les zones
- **Consommation électrique** avec seuils d'alerte

## 🐛 Dépannage

### Le script ne fonctionne pas

1. **Vérifiez MQTT** : Assurez-vous que l'intégration MQTT est active
2. **Vérifiez les entités** : Les appareils Awtrix doivent avoir `device_topic` dans leur entity_id
3. **Consultez les logs** : `Paramètres` → `Système` → `Logs`

### Aucun appareil détecté

```yaml
# Test manuel pour vérifier la détection
action: script.awtrix_dynamique_customapp
data:
  message: "Test"
  icone: "warning"
```

Si rien ne s'affiche, vérifiez que vos appareils Awtrix sont bien configurés avec des entity_id contenant `device_topic`.

### Messages ne s'affichent pas

1. Vérifiez les **topics MQTT** de vos appareils
2. Testez avec une **notification simple** d'abord
3. Vérifiez la **syntaxe JSON** du payload

## 📚 Ressources

- [Documentation Awtrix](https://blueforcer.github.io/awtrix3/)
- [Intégration MQTT Home Assistant](https://www.home-assistant.io/integrations/mqtt/)
- [Templates Home Assistant](https://www.home-assistant.io/docs/configuration/templating/)

## 🤝 Contribution

N'hésitez pas à :
- Signaler des bugs
- Proposer des améliorations
- Partager vos cas d'usage
- Contribuer à la documentation

## 📄 Licence

Ce script est libre d'utilisation et de modification pour la communauté Home Assistant.

---

*Créé avec ❤️ pour la communauté Home Assistant*
