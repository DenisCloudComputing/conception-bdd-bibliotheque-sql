# 📚 Conception d'une base de données pour une librairie

![Statut du projet](https://img.shields.io/badge/Statut-En%20cours-orange)
![Licence](https://img.shields.io/badge/Licence-MIT-blue)
![Niveau](https://img.shields.io/badge/Niveau-Formation%20AWS%20Re%2FStart-purple)

> 📖 Dépôt pédagogique documentant, étape par étape, la conception complète d'une base de
> données relationnelle — du premier entretien avec le client jusqu'aux scripts SQL finaux.

---

## 🎯 À propos de ce projet

Une librairie locale (fictive) a besoin d'une base de données pour gérer **son stock**,
**ses ventes** et **le planning de ses événements** (dédicaces d'auteurs, venues d'artistes).

Dans ce scénario, je tiens le rôle du **concepteur de base de données** : ma mission s'arrête
à la conception et au développement de la base — le développement de l'application cliente
est assuré par un partenaire (hors périmètre de ce dépôt).

Ce dépôt documente **l'intégralité de ma démarche**, phase par phase, avec l'idée suivante :

> 💡 **Un bon concepteur de base de données ne commence jamais par écrire du SQL.**
> Il commence par **comprendre le métier**, poser les bonnes questions, et modéliser
> patiemment avant d'implémenter quoi que ce soit.

---

## 🗺️ Pourquoi ce dépôt existe

Ce projet a un **double objectif** :

1. 🎓 **Pédagogique** — Servir de référence claire et guidée pour toute personne qui apprend
   la conception de bases de données (comme moi actuellement), en montrant **le raisonnement
   complet**, pas seulement le résultat final.
2. 💼 **Portfolio** — Démontrer à un recruteur ma capacité à mener une démarche de conception
   **de bout en bout** : recueil des besoins, modélisation conceptuelle, modélisation logique,
   implémentation SQL — avec une communication claire à chaque étape.

---

## 🧠 Méthode de travail : le format AFFIRMATION → Solution → Justification

Pour chaque question ou problème rencontré, j'applique systématiquement la même structure :

| Étape | Contenu |
|---|---|
| **🔴 AFFIRMATION (Question)** | Le problème ou la question posée, tel qu'il se présente réellement |
| **✅ Solution** | La réponse ou la décision de conception retenue |
| **💡 Justification** | Le raisonnement technique **derrière** la solution — le "pourquoi du comment" |

> 💡 **Pourquoi ce format ?** Une solution sans justification n'apprend rien à celui qui la lit.
> En expliquant systématiquement le raisonnement, ce dépôt devient un **outil d'apprentissage**,
> pas juste une suite de résultats à copier.

---

## 🗂️ Structure du dépôt

Le projet est organisé en **4 phases chronologiques**, à parcourir dans l'ordre :

```
conception-bdd-bibliotheque-sql/
│
├── 01-lancement-du-projet/          📋 Recueil des besoins
├── 02-modelisation-conceptuelle/    🧩 Entités, attributs, MCD
├── 03-modelisation-logique/         🔗 Tables, clés, normalisation
├── 04-implementation-sql/           💻 Scripts SQL, requêtes
└── ressources/                      🖼️ Schémas et diagrammes
```

Chaque dossier contient son propre `README.md` qui sert de **sommaire** pour la phase, avec
des liens directs vers chaque exercice.

---

## 📊 Avancement du projet

| Phase | Description | Statut |
|---|---|---|
| [**Phase 1**](./01-lancement-du-projet/README.md) | Lancement du projet & recueil des besoins | ![Terminée](https://img.shields.io/badge/-Terminée-brightgreen) |
| [**Phase 2**](./02-modelisation-conceptuelle/README.md) | Modélisation conceptuelle (MCD) | ![En attente](https://img.shields.io/badge/-À%20venir-lightgrey) |
| [**Phase 3**](./03-modelisation-logique/README.md) | Modélisation logique (MLD) | ![En attente](https://img.shields.io/badge/-À%20venir-lightgrey) |
| [**Phase 4**](./04-implementation-sql/README.md) | Implémentation SQL | ![En attente](https://img.shields.io/badge/-À%20venir-lightgrey) |

---

## 🛠️ Compétences démontrées dans ce dépôt

- ✅ **Analyse des besoins** : entretiens, ateliers, analyse documentaire
- ✅ **Analyse critique de documents métier** (formulaires, reçus, rapports, plannings)
- ✅ **Extraction et classification de données** (formats, identifiants, données calculées)
- 🔜 Modélisation conceptuelle (entités-associations, cardinalités)
- 🔜 Normalisation de bases de données (1NF, 2NF, 3NF)
- 🔜 Écriture de scripts SQL (`CREATE TABLE`, contraintes, requêtes)
- ✅ **Bonnes pratiques Git/GitHub** : commits descriptifs, structure de dépôt claire, documentation continue

---

## 🚀 Comment naviguer dans ce dépôt (pour les apprenants)

Si tu découvres ce dépôt pour apprendre la conception de bases de données, voici le parcours
recommandé :

1. **Commence ici** en lisant ce README pour comprendre le contexte global
2. **Ouvre le dossier [`01-lancement-du-projet`](./01-lancement-du-projet/README.md)** et
   suis les exercices dans l'ordre (1 → 2 → 3 → récapitulatif)
3. **Passe au dossier suivant** une fois la phase précédente terminée — chaque phase s'appuie
   sur les découvertes de la précédente
4. À chaque exercice, essaie de **répondre par toi-même avant de lire ma solution** : c'est
   la meilleure façon d'apprendre

> 💡 **Astuce :** clique sur les badges de statut dans le tableau d'avancement ci-dessus pour
> savoir quelles phases sont déjà consultables.

---

## ⚙️ Outils et technologies utilisés

| Outil | Usage |
|---|---|
| **Git & GitHub** | Versionnement et publication du projet |
| **VS Code** | Rédaction de la documentation et du code SQL |
| **Markdown** | Format de toute la documentation pédagogique |
| **SQL** *(à venir en Phase 4)* | Implémentation physique de la base de données |

---

## 👤 Auteur

**Denis** — Formation *AWS Re/Start*, en reconversion vers les métiers de la donnée et du cloud.

- 🔗 GitHub : [@DenisCloudComputing](https://github.com/DenisCloudComputing)

> 💬 N'hésite pas à ouvrir une **Issue** sur ce dépôt si tu repères une erreur, si tu as une
> question, ou si tu veux discuter d'un choix de conception !

---

## 📄 Licence

Ce projet est sous licence **MIT** — voir le fichier [`LICENSE`](./LICENSE) pour plus de détails.
Tu es libre de t'en inspirer pour ton propre apprentissage.