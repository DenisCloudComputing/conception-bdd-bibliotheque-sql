# 📘 Exercice 1 — Lancement d'un projet de base de données

## 🎯 Contexte

Une bibliothèque locale souhaite une base de données pour suivre :
- Le stock
- Les comptes rendus de ventes
- Le planning des venues d'auteurs et d'artistes

Les propriétaires et employés y accéderont via une application web développée par un partenaire.
Ma mission se limite à la **conception et au développement de la base de données**.

---

## ❓ Question 1

> **AFFIRMATION (QUESTION) :** Quel type d'information dois-je obtenir des parties prenantes
> avant de pouvoir commencer à modéliser la base de données ?

### ✅ Solution

Avant toute modélisation, je dois recueillir les catégories d'information suivantes :

| Catégorie | Exemples de questions à poser |
|---|---|
| **Processus métier** | Comment se déroule une vente ? Comment le stock est-il réapprovisionné ? |
| **Entités et attributs** | Quelles informations décrivent un livre (ISBN, titre, prix, auteur, éditeur, catégorie) ? |
| **Relations et règles de gestion** | Un livre peut-il avoir plusieurs auteurs ? Un auteur peut-il avoir plusieurs livres ? |
| **Contraintes métier** | Un livre en rupture de stock peut-il être vendu en précommande ? |
| **Utilisateurs et rôles** | Qui a accès à quoi ? (propriétaire, caissier, gestionnaire de stock) |
| **Volumétrie** | Combien de livres en stock ? Combien de ventes par jour ? |
| **Besoins de reporting** | Quels rapports de ventes sont attendus (par jour, par auteur, par catégorie) ? |
| **Systèmes existants** | Utilisent-ils déjà un logiciel de caisse ou un tableur à migrer ? |
| **Contraintes techniques** | Budget, hébergement, technologie déjà imposée par le partenaire de développement ? |

### 💡 JUSTIFICATION

La modélisation d'une base de données (Modèle Conceptuel de Données, puis Modèle Logique)
repose entièrement sur trois éléments : **les entités**, **leurs attributs**, et **les relations
(avec leurs cardinalités)** qui les lient.

Or, ces éléments ne se devinent pas : ils appartiennent au **métier** de la librairie, pas à moi
en tant que concepteur technique. Même en connaissant bien la librairie de l'extérieur, je ne
connais pas ses **règles de gestion internes** (ex : politique de retour, gestion des réservations,
remises fidélité...).

Sauter cette étape de recueil des besoins est l'une des causes les plus fréquentes d'échec en
conception de base de données : on obtient un modèle qui "fonctionne" techniquement mais qui
**ne correspond pas à la réalité du métier**, ce qui oblige à tout reprendre plus tard — à un coût
bien plus élevé qu'une bonne phase d'analyse initiale (principe bien connu en génie logiciel :
plus une erreur est détectée tard, plus elle coûte cher à corriger).

---

## ❓ Question 2

> **AFFIRMATION (QUESTION) :** Comment vais-je interagir avec mes clients et les parties
> prenantes du projet pendant la phase de conception de la base de données ?

### ✅ Solution

Je vais mettre en place une communication **itérative et vulgarisée**, en combinant plusieurs
méthodes :

1. **Entretiens individuels (interviews)** avec les propriétaires et employés-clés, pour comprendre
   les processus métier réels (pas supposés).
2. **Observation sur le terrain (job shadowing)** : passer du temps en librairie pour observer
   une vente, une réception de stock, etc.
3. **Ateliers collaboratifs (workshops)** avec plusieurs parties prenantes en même temps, pour
   faire émerger des règles de gestion que personne n'aurait mentionnées seul.
4. **Restitutions régulières et vulgarisées** : présenter mes schémas (MCD) sous une forme
   compréhensible par des non-techniciens (exemples concrets, pas de jargon SQL).
5. **Validation par cas d'usage concrets** : "Si un client achète 2 livres et en retourne un,
   que doit-il se passer dans le système ?"
6. **Points de suivi courts et réguliers** (plutôt qu'une seule grosse réunion), pour ajuster le
   modèle au fur et à mesure et éviter les mauvaises surprises en fin de projet.

### 💡 JUSTIFICATION

Les propriétaires et employés de la librairie ne sont **pas des experts en bases de données** :
leur langage naturel est celui de leur métier (livres, ventes, auteurs), pas celui des entités,
attributs ou cardinalités.

Une communication efficace doit donc :
- **s'adapter au vocabulaire de l'interlocuteur** (vulgarisation),
- **être itérative**, car les besoins réels émergent souvent progressivement, au fil des
  discussions et des exemples concrets (rarement dès la première réunion),
- **impliquer les utilisateurs finaux** (pas seulement les propriétaires) car ce sont eux qui
  connaissent les détails opérationnels du quotidien.

Ce fonctionnement correspond à une pratique reconnue en analyse des besoins : plus on valide
tôt et souvent avec les parties prenantes, plus on réduit le risque de construire un modèle
de données qui ne correspond pas à la réalité du terrain.