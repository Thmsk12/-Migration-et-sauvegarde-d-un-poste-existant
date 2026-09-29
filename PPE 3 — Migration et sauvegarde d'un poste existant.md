---
title: PPE 3 — Migration et sauvegarde d'un poste existant

---



# Projet 3 — Migration et sauvegarde d'un poste existant

**Cours :** IT Essentials — Mettre en service un poste de travail ICT avec le système d'exploitation (ICT-187)
**Classe :** E1A
**Projet :** Projet 3 — Migration et sauvegarde d'un poste existant
**Étudiant :** Thomas
**Environnement :** VMware Workstation / Windows 10 Pro
**Date :** 16.09.2026

---

# 1. Introduction

## 1.1 Contexte

Dans le cadre de ce projet, l'objectif est de simuler la migration d'un poste de travail vieillissant vers une nouvelle installation propre du système d'exploitation.

L'utilisateur souhaite conserver ses données personnelles et ses paramètres essentiels malgré la réinstallation complète de Windows.

La migration doit donc être réalisée en plusieurs étapes afin d'éviter toute perte de données :

1. Inventaire des données existantes.
2. Inventaire des logiciels et paramètres.
3. Création d'un support de sauvegarde.
4. Sauvegarde des données utilisateur.
5. Sauvegarde des favoris du navigateur.
6. Sauvegarde de la configuration réseau.
7. Vérification de la sauvegarde.
8. Réinstallation propre de Windows.
9. Réinstallation des logiciels nécessaires.
10. Restauration des données.
11. Reconfiguration du poste.
12. Vérification finale.

---

# 2. Objectifs du projet

Les objectifs de ce projet sont les suivants :

* Réaliser un inventaire des données présentes sur le poste.
* Identifier les données importantes à conserver.
* Identifier les logiciels nécessaires.
* Sauvegarder les documents utilisateur.
* Sauvegarder les favoris du navigateur.
* Sauvegarder les paramètres réseau.
* Vérifier l'intégrité de la sauvegarde.
* Effectuer une réinstallation propre de Windows.
* Restaurer les données après la réinstallation.
* Réinstaller les logiciels nécessaires.
* Reconfigurer les paramètres essentiels.
* Vérifier que les données ont bien été récupérées.
* Documenter une procédure reproductible.

---

# 3. Environnement de travail

## 3.1 Machine virtuelle

Pour réaliser ce projet, une **nouvelle machine virtuelle** a été créée dans VMware Workstation afin de simuler un poste de travail devant être migré.

La machine virtuelle utilise :

| Élément                        | Configuration      |
| ------------------------------ | ------------------ |
| Logiciel de virtualisation     | VMware Workstation |
| Système d'exploitation         | Windows 10 Pro     |
| Architecture                   | 64 bits            |
| Mémoire RAM                    | 4 Go               |
| Processeur                     | 2 cœurs            |
| Disque système                 | Environ 60 Go      |
| Disque de sauvegarde           | Environ 20 Go      |
| Lettre du disque de sauvegarde | E:                 |


> 

### 📸 CAPTURE À INSÉRER — Configuration de la VM




* RAM
* processeur
* disque dur
* lecteur CD/DVD
* deuxième disque virtuel

![image](https://hackmd.io/_uploads/Sy_hK-_Fzx.png)

---

# 4. Création du deuxième disque de sauvegarde

Afin de simuler un support externe de sauvegarde, un deuxième disque virtuel a été ajouté à la machine virtuelle.

Ce disque est utilisé uniquement pour stocker les données qui devront être conservées pendant la réinstallation de Windows.

Dans Windows, ce disque apparaît sous le nom :

**Sauvegarde (E:)**

Le disque système contenant Windows reste sur `C:`.

La configuration finale avant la migration est donc :

```text
C:\
└── Windows 10 Pro

E:\
└── Sauvegarde
```

Le lecteur `D:` correspond au lecteur CD/DVD virtuel utilisé pour l'ISO de Windows.


* Disque 0
* Partition C:
* Deuxième disque
* `Sauvegarde (E:)`

![Capture d'écran 2026-09-16 103045](https://hackmd.io/_uploads/SJ47oZ_FGe.png)



---

# 5. Inventaire des données

Avant de procéder à la sauvegarde, un inventaire des données présentes sur le poste a été réalisé.

L'objectif est de déterminer quelles données doivent impérativement être conservées avant la réinstallation.

## 5.1 Données utilisateur

Les catégories suivantes ont été identifiées :

* Documents
* Images
* Téléchargements
* Bureau
* Favoris du navigateur
* Configuration réseau

Les données utilisateur sont principalement situées dans :

```text
C:\Users\Utilisateur tk\
```

### 📸 CAPTURE - Dossier utilisateur

![Capture d'écran 2026-09-16 113644](https://hackmd.io/_uploads/SJCcaW_tMx.png)



```text
C:\Users\Utilisateur tk\
```
![Capture d'écran 2026-09-16 144155](https://hackmd.io/_uploads/B156RWOFzg.png)

![Capture d'écran 2026-09-16 144203](https://hackmd.io/_uploads/SJn30-OKGe.png)

![image](https://hackmd.io/_uploads/S1mnAZuFMe.png)

---

# 6. Inventaire des logiciels

Un inventaire des logiciels présents sur le poste a également été réalisé.

Dans cette nouvelle VM, aucun logiciel supplémentaire comme LibreOffice ou 7-Zip n'était installé initialement.

Les logiciels réellement présents sur la machine doivent être indiqués dans le tableau ci-dessous.

| Logiciel       | Présent avant migration | À réinstaller |
| -------------- | ----------------------- | ------------- |
| Google Chrome  | Oui                     | Oui           |
| Microsoft Edge | Oui                     | Selon besoin  |
| LibreOffice    | Oui                     | Oui           |
| 7-Zip          | Oui                     | Oui           

> Les logiciels non présents sur la machine ne sont pas considérés comme faisant partie de l'inventaire à restaurer.

### 📸 CAPTURE - Applications installées



`Paramètres → Applications → Applications et fonctionnalités`

![image](https://hackmd.io/_uploads/ryiMNG_tMg.png)


---

# 7. Préparation du support de sauvegarde

Le deuxième disque virtuel a été configuré dans Windows avec la lettre de lecteur :

```text
E:
```

Le volume est nommé :

```text
Sauvegarde
```

Il apparaît donc dans l'Explorateur de fichiers sous :

```text
Sauvegarde (E:)
```

Un dossier principal destiné à contenir toutes les données sauvegardées a été créé :

```text
E:\SAUVEGARDE
```

La structure prévue est la suivante :

```text
E:\
└── SAUVEGARDE
    ├── Documents
    ├── Images
    ├── Téléchargements
    ├── Bureau
    ├── Favoris
    └── Configuration
```

### 📸 CAPTURE — Disque E:

![image](https://hackmd.io/_uploads/rytnEGuFGg.png)


et le dossier :

```text
SAUVEGARDE
```
![image](https://hackmd.io/_uploads/H1GANG_YGx.png)
![Capture d'écran 2026-09-16 150840](https://hackmd.io/_uploads/SJwWrMOtfg.png)
![image](https://hackmd.io/_uploads/By5xBMdtfe.png)
![image](https://hackmd.io/_uploads/BJNSSfOYfe.png)

![image](https://hackmd.io/_uploads/BJHVrzdYMg.png)



---

# 8. Sauvegarde des documents

Les documents personnels présents dans le dossier utilisateur sont copiés vers le deuxième disque.

Le dossier d'origine est :

```text
C:\Users\Utilisateur tk\Documents
```

La destination de sauvegarde est :

```text
E:\SAUVEGARDE\Documents
```

La copie permet de conserver les documents indépendamment du disque système.

### 📸 CAPTURE  — Documents avant sauvegarde

![Capture d'écran 2026-09-16 113644](https://hackmd.io/_uploads/r1H2rM_Yzl.png)


### 📸 CAPTURE À INSÉRER — Documents sauvegardés

![Capture d'écran 2026-09-16 150755](https://hackmd.io/_uploads/HkuaHM_tfe.png)


---

# 9. Sauvegarde des images

Les images présentes dans le profil utilisateur sont également sauvegardées.

Dossier d'origine :

```text
C:\Users\Utilisateur tk\Images
```

Destination :

```text
E:\SAUVEGARDE\Images
```

Cette copie permet de conserver les fichiers images lors de la réinstallation du système.
![Capture d'écran 2026-09-16 151412](https://hackmd.io/_uploads/BygtrLf_Fzg.png)

![image](https://hackmd.io/_uploads/B11HUzutfe.png)


---

# 10. Sauvegarde des téléchargements

Le dossier Téléchargements est également sauvegardé.

Dossier d'origine :

```text
C:\Users\Utilisateur tk\Downloads
```

Destination :

```text
E:\SAUVEGARDE\Téléchargements
```

Les fichiers présents dans ce dossier sont ainsi conservés avant la réinstallation.

![image](https://hackmd.io/_uploads/Sy2F8fuYze.png)


---

# 11. Sauvegarde du Bureau

Les éléments présents sur le Bureau peuvent également contenir des documents ou raccourcis importants.

Le dossier d'origine est :

```text
C:\Users\Utilisateur tk\Desktop
```

La sauvegarde est réalisée vers :

```text
E:\SAUVEGARDE\Bureau
```
![Capture d'écran 2026-09-16 151717](https://hackmd.io/_uploads/BydlwMOFzl.png)

![image](https://hackmd.io/_uploads/SyGeDGOFGl.png)


---

# 12. Sauvegarde des favoris Google Chrome

Les favoris du navigateur font partie des données importantes à conserver.

Ils ont été exportés depuis Google Chrome sous la forme d'un fichier HTML.

Dans Google Chrome :

1. Ouvrir le gestionnaire de favoris.
2. Cliquer sur les trois points.
3. Sélectionner **Exporter les favoris**.
4. Choisir le disque de sauvegarde.
5. Enregistrer le fichier.

Le fichier exporté contient les favoris du navigateur.

Le fichier a été placé dans le dossier :

```text
E:\SAUVEGARDE\Favoris
```

Le format utilisé est :

```text
.html
```

### 📸 CAPTURE  — Favoris Chrome

![Capture d'écran 2026-09-16 132954](https://hackmd.io/_uploads/SkdBvMutGx.png)



### 📸 CAPTURE  — Fichier HTML

![Capture d'écran 2026-09-16 151947](https://hackmd.io/_uploads/r1c2vzdtzx.png)

![image](https://hackmd.io/_uploads/ryxjvGOKGg.png)


---

# 13. Sauvegarde de la configuration réseau

La configuration réseau du poste a également été sauvegardée.

Cette étape permet de conserver les informations nécessaires pour reconfigurer le réseau après la réinstallation.

La commande suivante a été utilisée :

```cmd
ipconfig /all > E:\SAUVEGARDE\Configuration\reseau.txt
```

Cette commande récupère les informations affichées par `ipconfig /all` et les enregistre dans un fichier texte.

Le fichier obtenu est :

```text
E:\SAUVEGARDE\Configuration\reseau.txt
```

Il peut notamment contenir :

* adresse IPv4
* masque de sous-réseau
* passerelle
* adresse MAC
* serveurs DNS
* informations DHCP

### 📸 CAPTURE  — Commande ipconfig

![Capture d'écran 2026-09-16 140918](https://hackmd.io/_uploads/S1Ez_zOtzx.png)



### 📸 CAPTURE  — Fichier réseau

![Capture d'écran 2026-09-16 140918](https://hackmd.io/_uploads/HyaI_MuFMx.png)

![Capture d'écran 2026-09-16 112634](https://hackmd.io/_uploads/HJ_Rdz_Kfx.png)


---

# 14. Sauvegarde des informations système

Afin de conserver des informations supplémentaires sur l'ancien environnement, la commande suivante peut être utilisée :

```cmd
systeminfo > E:\SAUVEGARDE\Configuration\systeminfo.txt
```

Le fichier est enregistré dans :

```text
E:\SAUVEGARDE\Configuration\systeminfo.txt
```

Cette information permet de conserver une trace de la configuration système avant la migration.

### 📸 CAPTURE À INSÉRER

![image](https://hackmd.io/_uploads/HJDr5M_Kfg.png)


---

# 15. Vérification de la sauvegarde

Avant de réinstaller Windows, il est nécessaire de vérifier que les données ont bien été copiées.

Cette étape est importante car la réinstallation du système peut entraîner la suppression des données présentes sur le disque système.

La vérification consiste notamment à comparer :

* le nombre de fichiers ;
* la taille des données ;
* les noms des fichiers ;
* la présence des différents dossiers.

---

## 15.1 Comparaison des dossiers

Exemple de tableau de vérification :

| Données         | Emplacement original                | Emplacement sauvegarde          | Nombre de fichiers | Vérification |
| --------------- | ----------------------------------- | ------------------------------- | -----------------: | ------------ |
| Documents       | `C:\Users\Utilisateur tk\Documents` | `E:\SAUVEGARDE\Documents`       |                  X | OK           |
| Images          | `C:\Users\Utilisateur tk\Images`    | `E:\SAUVEGARDE\Images`          |                  X | OK           |
| Téléchargements | `C:\Users\Utilisateur tk\Downloads` | `E:\SAUVEGARDE\Téléchargements` |                  X | OK           |
| Bureau          | `C:\Users\Utilisateur tk\Desktop`   | `E:\SAUVEGARDE\Bureau`          |                  X | OK           |
| Favoris         | Chrome                              | `E:\SAUVEGARDE\Favoris`         |                  X | OK           |

Les valeurs `X` doivent être remplacées par les valeurs réellement observées.

---

# 16. Vérification de la taille des fichiers

Pour chaque dossier important, les propriétés peuvent être consultées afin de comparer la taille des données originales avec celle de la sauvegarde.

Par exemple :

```text
Dossier original :
C:\Users\Utilisateur tk\Documents

Dossier sauvegardé :
E:\SAUVEGARDE\Documents
```

Les informations suivantes sont comparées :

* nombre de fichiers ;
* taille du dossier ;
* présence des fichiers importants.

disc C:

![image](https://hackmd.io/_uploads/r1GQMm_YGl.png)



disc E:
![image](https://hackmd.io/_uploads/HyL1fQuFfg.png)


---

# 17. Structure finale de la sauvegarde

Avant de procéder à la réinstallation, la sauvegarde doit contenir les différentes catégories de données nécessaires.

Structure :

```text
E:\SAUVEGARDE
│
├── Documents
│
├── Images
│
├── Téléchargements
│
├── Bureau
│
├── Favoris
│   └── favoris.html
│
└── Configuration
    ├── reseau.txt
    └── systeminfo.txt
```
### 📸 CAPTURE — Sauvegarde complète
![image](https://hackmd.io/_uploads/r1wrbQOYGe.png)


![Capture d'écran 2026-09-16 114055](https://hackmd.io/_uploads/BkUJiMdtGg.png)


---

# 18. Réinstallation propre de Windows 10 Pro

Une fois la sauvegarde terminée et vérifiée, la réinstallation propre du système peut être réalisée.

L'objectif est de simuler un poste vieillissant qui doit être entièrement réinstallé.

L'installation est réalisée à partir de l'ISO de Windows 10 Pro.

La procédure consiste à :

1. Démarrer la machine virtuelle sur l'ISO.
2. Lancer l'installation de Windows.
3. Choisir l'installation personnalisée.
4. Sélectionner le disque système.
5. Supprimer/formater les anciennes partitions du système.
6. Installer Windows 10 Pro sur le disque système.
7. Terminer la configuration initiale.

### ⚠️ Attention

Le disque contenant la sauvegarde doit être conservé.

Dans cette simulation :

```text
C: → système Windows → réinstallé

E: → sauvegarde → conservé
```

Le disque `E:` ne doit donc pas être formaté pendant la réinstallation.

### 📸 CAPTURE  — Installation Windows
![Capture d'écran 2026-09-16 114134](https://hackmd.io/_uploads/SyTQifuFMl.png)

![Capture d'écran 2026-09-16 114238](https://hackmd.io/_uploads/ry9Gof_YGe.png)
![Capture d'écran 2026-09-16 114415](https://hackmd.io/_uploads/BJ3rifdtfl.png)


### 📸 CAPTURE  — Choix du disque

![Capture d'écran 2026-09-16 132325](https://hackmd.io/_uploads/HJgFsMutfl.png)



---

# 19. Première ouverture de Windows après réinstallation

Après l'installation, Windows 10 Pro démarre sur un environnement propre.

Le système ne contient plus les anciennes données utilisateur présentes avant la réinstallation.

Cette situation permet de vérifier que la migration fonctionne correctement.

![Capture d'écran 2026-09-16 153819](https://hackmd.io/_uploads/rkXSnMdFze.png)



---

# 20. Vérification du disque de sauvegarde

Après la réinstallation, le deuxième disque doit être accessible.

Dans l'Explorateur de fichiers, le disque apparaît sous :

```text
Sauvegarde (E:)
```

Le dossier suivant doit toujours être présent :

```text
E:\SAUVEGARDE
```

Les données sauvegardées avant la réinstallation doivent être accessibles.

![image](https://hackmd.io/_uploads/SkUXpG_Fzg.png)




---

# 21. Réinstallation des logiciels

Après la réinstallation du système, les logiciels nécessaires identifiés lors de l'inventaire doivent être réinstallés.

Dans cette VM, seuls les logiciels réellement présents avant la migration doivent être considérés comme nécessaires.

Par exemple :

| Logiciel       | Avant       | Après        | Vérification |
| -------------- | ----------- | ------------ | ------------ |
| Google Chrome  | Présent     | Réinstallé   | OK           |
| Microsoft Edge | Présent     | Présent      | OK           |
| LibreOffice    | Non présent | Non installé | —            |
| 7-Zip          | Non présent | Non installé | —            |

Les logiciels supplémentaires peuvent être installés uniquement s'ils sont nécessaires au projet.

![Capture d'écran 2026-09-16 151517](https://hackmd.io/_uploads/BkTtTzutze.png)

---

# 22. Restauration des documents

Les documents sont maintenant restaurés depuis le disque de sauvegarde.

Source :

```text
E:\SAUVEGARDE\Documents
```

Destination :

```text
C:\Users\Utilisateur tk\Documents
```

Les fichiers sont ainsi replacés dans le profil du nouvel utilisateur.

![image](https://hackmd.io/_uploads/SymZRG_tfg.png)


---

# 23. Restauration des images

Les images sont restaurées depuis :

```text
E:\SAUVEGARDE\Images
```

vers :

```text
C:\Users\Utilisateur tk\Images
```

![Capture d'écran 2026-09-16 151412](https://hackmd.io/_uploads/r1b7AzdYMl.png)
![Capture d'écran 2026-09-16 151423](https://hackmd.io/_uploads/SJcmAfuYzx.png)


---

# 24. Restauration des téléchargements

Les fichiers téléchargés sont restaurés depuis :

```text
E:\SAUVEGARDE\Téléchargements
```

vers :

```text
C:\Users\Utilisateur tk\Downloads
```

![image](https://hackmd.io/_uploads/HkCS0GOtzg.png)


---

# 25. Restauration du Bureau

Les données sauvegardées du Bureau sont restaurées depuis :

```text
E:\SAUVEGARDE\Bureau
```

vers :

```text
C:\Users\Utilisateur tk\Desktop
```

![image](https://hackmd.io/_uploads/HyMtRfdYfg.png)


---

# 26. Restauration des favoris Chrome

Les favoris Google Chrome sont restaurés à partir du fichier HTML sauvegardé précédemment.

Le fichier de sauvegarde est situé dans :

```text
E:\SAUVEGARDE\Favoris
```

Dans Chrome :

1. Ouvrir le gestionnaire de favoris.
2. Cliquer sur les trois points.
3. Sélectionner **Importer les favoris**.
4. Sélectionner le fichier HTML sauvegardé.
5. Vérifier que les favoris apparaissent.

### 📸 CAPTURE  — Importation

![image](https://hackmd.io/_uploads/H16hRfOYMx.png)
![Capture d'écran 2026-09-16 155109](https://hackmd.io/_uploads/SkTx1m_YGx.png)

![image](https://hackmd.io/_uploads/SJR1kX_FGx.png)

### 📸 CAPTURE  — Vérification

![image](https://hackmd.io/_uploads/rJTNkQ_tGx.png)


---

# 27. Reconfiguration du réseau

Après la réinstallation, la configuration réseau est vérifiée.

Le fichier sauvegardé précédemment :

```text
E:\SAUVEGARDE\Configuration\reseau.txt
```

permet de retrouver les informations de la configuration précédente.

La commande suivante permet de vérifier la configuration actuelle :

```cmd
ipconfig /all
```

![image](https://hackmd.io/_uploads/ryeRPkXdYGl.png)


---

# 28. Test de la connexion réseau

Un test de connexion est réalisé afin de vérifier que le réseau fonctionne correctement.

Test de l'adresse IP :

![Capture d'écran 2026-09-16 155414](https://hackmd.io/_uploads/rkwxg7_Kze.png)

```cmd
ping 8.8.8.8
```


Test de résolution DNS :
![Capture d'écran 2026-09-16 155445](https://hackmd.io/_uploads/HkiWe7uFfl.png)

```cmd
ping google.com
```

Si les réponses sont reçues correctement, la connexion réseau est fonctionnelle.



---

# 29. Vérification de l'intégrité après restauration

Après la restauration, les données sont vérifiées individuellement.

Les contrôles effectués sont :

* ouverture des documents ;
* ouverture des images ;
* vérification des fichiers téléchargés ;
* vérification du contenu du Bureau ;
* vérification des favoris Chrome ;
* vérification du fonctionnement des logiciels ;
* vérification de la connexion réseau.

---

# 30. Tableau de vérification finale

| Élément               | Sauvegardé | Restauré | Vérifié |
| --------------------- | ---------- | -------- | ------- |
| Documents             | ✅          | ✅        | ✅       |
| Images                | ✅          | ✅        | ✅       |
| Téléchargements       | ✅          | ✅        | ✅       |
| Bureau                | ✅          | ✅        | ✅       |
| Favoris Chrome        | ✅          | ✅        | ✅       |
| Configuration réseau  | ✅          | ✅        | ✅       |
| Logiciels nécessaires | ✅          | ✅        | ✅       |
| Connexion réseau      | ✅          | ✅        | ✅       |

---

# 31. Comparaison avant / après

| Élément              | Avant migration | Après migration           |
| -------------------- | --------------- | ------------------------- |
| Système              | Windows 10 Pro  | Windows 10 Pro réinstallé |
| Documents            | Présents        | Restaurés                 |
| Images               | Présentes       | Restaurées                |
| Téléchargements      | Présents        | Restaurés                 |
| Bureau               | Présent         | Restauré                  |
| Favoris Chrome       | Présents        | Restaurés                 |
| Configuration réseau | Sauvegardée     | Vérifiée                  |
| Logiciels            | Inventoriés     | Réinstallés               |
| Disque de sauvegarde | E:              | E:                        |
| Connexion Internet   | Fonctionnelle   | Testée                    |

---

# 32. Procédure de migration reproductible

La procédure suivante peut être réutilisée pour un autre poste de travail.

## Étape 1 — Inventaire

Identifier :

* les documents ;
* les images ;
* les téléchargements ;
* les fichiers du Bureau ;
* les favoris ;
* les logiciels ;
* les paramètres réseau ;
* les autres données importantes.

## Étape 2 — Préparation de la sauvegarde

Connecter un support externe ou un espace réseau suffisamment grand.

Créer une structure organisée :

```text
SAUVEGARDE
├── Documents
├── Images
├── Téléchargements
├── Bureau
├── Favoris
└── Configuration
```

## Étape 3 — Sauvegarde

Copier toutes les données importantes vers le support de sauvegarde.

Exporter les favoris du navigateur.

Sauvegarder la configuration réseau.

## Étape 4 — Vérification

Comparer :

* nombre de fichiers ;
* taille ;
* noms ;
* contenu.

Ouvrir quelques fichiers afin de vérifier qu'ils sont lisibles.

## Étape 5 — Réinstallation

Réinstaller proprement le système d'exploitation sur le disque système.

**Ne pas supprimer le support contenant la sauvegarde.**

## Étape 6 — Réinstallation des logiciels

Réinstaller les logiciels nécessaires identifiés lors de l'inventaire.

## Étape 7 — Restauration

Copier les données depuis la sauvegarde vers les nouveaux dossiers utilisateur.

Importer les favoris.

## Étape 8 — Reconfiguration

Configurer :

* réseau ;
* imprimantes si nécessaire ;
* comptes ;
* paramètres essentiels.

## Étape 9 — Vérification finale

Vérifier :

* documents ;
* images ;
* téléchargements ;
* Bureau ;
* favoris ;
* logiciels ;
* réseau.

---



## 33.1 Configuration du deuxième disque

Lors de la préparation de la sauvegarde, un deuxième disque virtuel a été ajouté dans VMware.

Ce disque apparaît dans Windows sous le nom :

```text
Sauvegarde (E:)
```

La lettre `E:` a donc été utilisée pour toutes les opérations de sauvegarde.

![image](https://hackmd.io/_uploads/SkD0g7dFMl.png)


---

## 33.2 Utilisation des commandes CMD

La commande suivante a été utilisée pour sauvegarder la configuration réseau :

```cmd
ipconfig /all > E:\SAUVEGARDE\Configuration\reseau.txt
```

L'utilisation de `E:` est nécessaire puisque le support de sauvegarde correspond au deuxième disque virtuel.

---

# 34. Résultat final

La migration a permis de simuler le remplacement d'un poste Windows par une installation propre du système d'exploitation.

Les données importantes ont été sauvegardées sur un deuxième disque virtuel avant la réinstallation.

Après l'installation de Windows, les données sauvegardées ont pu être restaurées et les paramètres essentiels ont été reconfigurés.

La vérification finale permet de confirmer que les données nécessaires sont de nouveau disponibles sur le poste.

---

# 35. Conclusion

Ce projet a permis de mettre en pratique une procédure complète de migration et de sauvegarde d'un poste de travail.

La principale étape de sécurité consiste à **réaliser et vérifier la sauvegarde avant toute réinstallation**.

La séparation entre le disque système `C:` et le disque de sauvegarde `E:` permet de conserver les données pendant la réinstallation du système.

La procédure réalisée peut être reproduite sur un autre poste en suivant les mêmes étapes :

```text
Inventaire
    ↓
Sauvegarde
    ↓
Vérification
    ↓
Réinstallation Windows
    ↓
Réinstallation des logiciels
    ↓
Restauration des données
    ↓
Reconfiguration
    ↓
Vérification finale
```

Le projet permet ainsi de répondre à l'objectif principal : **réinstaller proprement un poste tout en conservant les données et paramètres essentiels de l'utilisateur.**

---

