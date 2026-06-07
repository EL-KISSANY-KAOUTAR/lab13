# LAB 13 : Bypass de la Détection de Root Android avec Objection

## Objectif du laboratoire

De nombreuses applications Android vérifient si un appareil est **rooté** avant de fonctionner. Cette protection est couramment utilisée par :

- Les applications bancaires
- Les applications de paiement
- Certaines applications professionnelles
- Les jeux utilisant des mécanismes anti-triche

L'objectif de ce laboratoire est de comprendre comment utiliser **Frida** et **Objection** pour analyser le comportement d'une application Android et contourner les mécanismes de détection de root dans un environnement de test autorisé.

---

## Installation des dépendances

### Installation de Frida et Objection

```bash
pip install frida==16.5.9 frida-tools==13.6.0
pip install objection
```

### Vérification de l'installation

```bash
frida --version
adb devices
```

Exemple de résultat attendu :

```bash
16.5.9

List of devices attached
emulator-5554    device
```

---

## Installation d'Objection

Objection est un outil construit au-dessus de Frida permettant d'interagir facilement avec les applications Android en cours d'exécution.

### Installation avec pipx

```bash
pip install --user pipx
```

```bash
pipx ensurepath
```

### Vérification de l'installation

![Installation pipx](https://github.com/user-attachments/assets/575afb64-a77d-40b3-979f-f2e7a91733d7)

![Configuration pipx](https://github.com/user-attachments/assets/67104c33-6273-4cea-91b4-f10be7471ab2)

![Installation Objection](https://github.com/user-attachments/assets/59d480cd-83f6-4217-893c-cb311ad86154)

---

## Préparation de l'appareil Android

L'objectif de cette étape est de permettre à **Frida** installé sur le PC de communiquer avec un appareil Android (émulateur ou téléphone physique).

### Vérification de la connexion ADB

```bash
adb devices
```

Résultat attendu :

```bash
List of devices attached
emulator-5554    device
```

### Démarrage de Frida Server

Assurez-vous que :

- Le fichier `frida-server` correspondant à votre architecture Android est installé.
- Le serveur Frida est lancé sur l'appareil Android.
- La communication entre le PC et Android fonctionne correctement.

Vérification :

```bash
frida-ps -U
```

Si une liste de processus Android apparaît, la communication est opérationnelle.

---

## Démarrage d'Objection sur l'application cible

Pour attacher Objection à l'application OWASP MSTG UnCrackable Level 1 :

```bash
objection -g owasp.mstg.uncrackable1 explore
```

### Résultat obtenu

![Connexion Objection](https://github.com/user-attachments/assets/888dae1a-b52c-47f9-94df-abec88846edd)

### Analyse de la sortie

On observe les éléments suivants :

```text
Using USB device 'Android Emulator 5554'
```

L'émulateur Android est correctement détecté.

```text
Agent injected and responds ok!
```

L'agent Frida a été injecté avec succès dans l'application cible.

```text
owasp.mstg.uncrackable1 on (google: 13) [usb] #
```

Objection est connecté à l'application et prêt à recevoir des commandes.

---

## Désactivation de la détection de Root

Dans la console Objection, exécuter :

```bash
android root disable
```

Résultat :

```text
(agent) Registering job 112328. Type: root-detection-disable
```

Cette sortie indique que le module de bypass de détection de root a été chargé avec succès.

---

## Observations

À ce stade du laboratoire, on peut conclure que :

- ✅ Frida fonctionne correctement
- ✅ Objection fonctionne correctement
- ✅ L'application cible est attachée avec succès
- ✅ Le script `root-detection-disable` est chargé
- ✅ Le bypass est prêt à intercepter les vérifications de root

---

## Conclusion

Ce laboratoire a permis de :

- Installer et configurer Frida et Objection.
- Vérifier la communication entre Android et le poste d'analyse.
- Attacher Objection à une application Android.
- Charger le module de contournement de détection de root.
- Préparer l'environnement pour l'analyse dynamique de l'application.

Le laboratoire démontre comment Frida et Objection peuvent être utilisés pour étudier le comportement d'une application Android en temps réel dans un cadre de test et d'apprentissage autorisé.

---
**Application testée :** OWASP MSTG UnCrackable Level 1  
**Outils utilisés :** Frida 16.5.9, Frida-Tools 13.6.0, Objection 1.11.0, Android Emulator (Android 13)
