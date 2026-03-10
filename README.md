# 🐾 Distributeur de Croquettes Connecté (ESPHome & Home Assistant)

Un projet de distributeur de croquettes pour animaux (chiens / chats), rendu intelligent grâce à un ESP8266 (Wemos D1 Mini) sous **ESPHome** et parfaitement intégré à **Home Assistant**.

## ✨ Fonctionnalités

- 🕰️ **Planification des repas** : Gestion de 2 repas par jour configurables directement depuis l'interface Home Assistant.
- ⚖️ **Portions ajustables** : Réglage précis de la durée de distribution (en secondes) pour contrôler la quantité de croquettes servie, accessible via un curseur dédié.
- 🕹️ **Contrôle manuel** : 
  - Déclenchement à distance depuis Home Assistant via un bouton ("Distribuer maintenant").
  - Déclenchement local grâce à un **bouton physique** placé sur le distributeur.
- 🤖 **Mode Automatique / Manuel** : Possibilité de désactiver les repas planifiés à tout moment à l'aide d'un interrupteur ("Mode Automatique").
- 🌐 **Web Serveur Local** : Paramétrage et accès garantis via une page web locale (serveur web embarqué), même si Home Assistant n'est pas accessible.
- 🔄 **Mise à jour OTA** : Mises à jour du microprogramme intégrées sans fil directement depuis l'interface ESPHome.

## 🛠️ Matériel Requis

- **Microcontrôleur** : Wemos D1 Mini (ou équivalent ESP8266)
- **Actionneur mécanique** : Moteur DC (5V ou 12V), un servomoteur ou une vis sans fin pour faire tomber les croquettes.
- **Module de puissance** : Module MOSFET pour piloter la tension de l'actionneur.
- **Bouton poussoir** : Pour déclencher une portion manuellement à côté de la gamelle.
- **Alimentation** : Adaptée à l'actionneur choisi.

## 🔌 Schéma de Câblage (Pinout ESP8266)

| Composant | Pin D1 Mini | Commentaire / Mode |
|-----------|-------------|--------------------|
| **Gate du MOSFET** (Moteur) | `D1` | Pilotage de la distribution en sortie standard. |
| **Bouton Physique** | `D2` | Utilisé en `INPUT_PULLUP` (inversé = actif à l'état bas). |

## 🚀 Installation & Paramétrage

### 1. Préparation Côté Home Assistant
L'ESP étant programmé pour aller chercher la date depuis *Home Assistant*, vous devez impérativement créer au préalable deux entités de type `input_datetime` (Heure uniquement) :

1. Entité 1 : `input_datetime.repas_1`
2. Entité 2 : `input_datetime.repas_2`

Ces entités détermineront quand s'activera le script de distribution au quotidien.

### 2. Flash ESPHome
1. Clonez ce dépôt.
2. Éditez dans `distributeur.yaml` les informations Wi-Fi et les mots de passe.
3. Assurez-vous d'utiliser une intégration NTP locale appropriée (le fuseau par défaut est `Europe/Brussels`).
4. Flashez la carte D1 Mini via un câble USB ou over-the-air (OTA).

## 🛡️ Résilience, Sécurité & Fonctionnalités Avancées
- **Sauvegarde d'état :** La durée définie de la distribution ainsi que les états automatiques/manuels sont conservés dans la mémoire RAM du module qui redémarre dessus (`restore_value: yes`).
- **Logique Autonome :** Grâce au service SNTP qui tourne sur l'ESP, l'heure est synchronisée de manière indépendante. De plus, tant que le module sauvegarde les heures récupérées via HA, une coupure de votre serveur box domotique n'empêchera pas votre animal d'être nourri à son heure (assuré par la logique lambda interne).

---
Projet propulsé par ESPHome 🚀
