# 📘 Récapitulatif — Leçon 6 : Bilan de la Phase 1

## 🎯 Objectif de ce récapitulatif

Après les Exercices 1, 2 et 3, ce document propose un **temps de recul** : comparer mes
annotations avec des exemples de référence, identifier les points communs entre documents,
et surtout **assumer une divergence** que j'ai rencontrée avec le corrigé officiel — l'occasion
de justifier un choix de conception plutôt que de le suivre aveuglément.

---

## ❓ Question 1

> **AFFIRMATION (QUESTION) :** Parmi les différentes annotations, en existait-il qui étaient
> communes à plusieurs documents ?

### ✅ Solution

**Oui.** En reprenant mon tableau de synthèse de l'Exercice 3, plusieurs annotations
apparaissent dans **au moins 3 des 5 documents analysés** :

| Annotation | Documents où elle apparaît | Nombre |
|---|---|---|
| `cup` | Formulaire, Feuille Stock, Reçu, Compte rendu mensuel | 4 / 5 |
| `nom_article` / `titre` | Formulaire, Feuille Stock, Reçu, Compte rendu mensuel | 4 / 5 |
| `prix_vente` | Formulaire, Reçu, Compte rendu mensuel | 3 / 5 |
| `quantite` (sous une forme ou une autre) | Formulaire, Feuille Stock, Reçu, Compte rendu mensuel | 4 / 5 |
| `type_produit` / `description` | Formulaire, Compte rendu mensuel | 2 / 5 |
| `date` (réception, transaction ou rapport) | Formulaire, Reçu, Compte rendu mensuel | 3 / 5 |

### 💡 JUSTIFICATION

Ces répétitions ne sont **pas un hasard** : elles désignent les données **pivots** du système,
celles qui **relient** les documents entre eux. En modélisation de bases de données, une donnée
qui revient dans plusieurs sources est souvent un excellent candidat pour devenir :
- soit un **attribut clé** d'une entité centrale (le `cup` deviendra très probablement la
  **clé primaire** de l'entité `PRODUIT`),
- soit une **clé étrangère** qui matérialise une relation entre deux entités (le `cup` présent
  à la fois dans le Reçu et dans la Feuille Stock illustre déjà le lien futur entre les tables
  `PRODUIT` et `LIGNE_DE_VENTE`).

Concrètement, plus une donnée est **transversale**, plus il est urgent de bien la nommer et
d'en fixer le format dès maintenant — une erreur sur `cup` se répercuterait dans **toute la
base de données**, alors qu'une erreur sur un attribut isolé (ex. `editeur`) resterait
localisée à une seule table.

---

## ❓ Question 2

> **AFFIRMATION (QUESTION) :** Ai-je négligé certaines annotations ?

### ✅ Solution

**Oui, une divergence importante est apparue** en comparant mon travail avec les exemples
officiels fournis en fin de leçon. Voici la comparaison :

| Donnée | Mon annotation (Exercice 3) | Exemple officiel de la leçon | Écart |
|---|---|---|---|
| **CUP** | `Caractères` | `Numérique standard` | ⚠️ Divergence assumée |
| **Titre du livre** | `Caractères` | `Caractères` | ✅ Cohérent |
| **Prix de vente** | `Décimal` | `Numérique` | ➖ Nuance de vocabulaire |

**Je maintiens volontairement mon annotation `Caractères` pour le CUP**, et voici pourquoi
cette décision n'est pas une erreur, mais un choix de conception argumenté.

### 💡 JUSTIFICATION

**Sur le CUP — pourquoi je maintiens "Caractères" malgré le corrigé officiel :**

Un code produit (CUP, ISBN, code-barres) est **composé de chiffres**, mais cela ne signifie
pas qu'il doit être **stocké comme un nombre** en base de données. Trois arguments techniques
soutiennent ce choix :

1. **Risque de perte des zéros non significatifs** : un CUP comme `00343298349` stocké en
   type numérique deviendrait `343298349` — la donnée serait **corrompue silencieusement**,
   sans aucune erreur signalée par le système.
2. **Un CUP n'est jamais utilisé dans un calcul** : on ne l'additionne jamais, on ne calcule
   jamais sa moyenne. La règle de conception classique est : *si une donnée numérique ne sert
   à aucune opération arithmétique, elle doit être traitée comme du texte (une chaîne de
   caractères), même si elle ne contient que des chiffres.*
3. **Certains codes-barres internationaux contiennent des lettres ou des tirets** (comme
   l'ISBN du même formulaire : `0-449-22361-2`) — traiter tous les codes produits de la même
   façon (en texte) garantit la **cohérence** et évite une règle différente pour chaque type
   de code.

> 📌 **Ce que je retiens de cette divergence :** le corrigé officiel simplifie probablement
> la classification (CUP = suite de chiffres = "numérique" au sens intuitif), mais du point
> de vue **strict de la conception de bases de données**, le critère qui compte n'est pas
> "est-ce que ça ressemble à un nombre ?" mais **"est-ce que cette donnée sera un jour utilisée
> dans un calcul ou une comparaison d'ordre de grandeur ?"**. Cette nuance deviendra
> essentielle en Phase 3, au moment de choisir le vrai type SQL (`VARCHAR` vs `INT`).

**Sur les annotations que j'ai probablement sous-estimées :**

En comparant plus largement mon travail à l'esprit de l'exercice, deux points méritent d'être
renforcés dans les prochaines phases :
- Je n'ai pas suffisamment insisté sur les **unités de mesure implicites** (le prix est-il
  toujours en USD ? Le formulaire ne le précise pas explicitement partout).
- Je n'ai pas creusé la question du **format exact des dates** (jour/mois/année vs
  mois/jour/année) — un détail anodin en apparence, mais qui peut créer de **vraies erreurs
  de données** si les employés saisissent les dates de façon incohérente d'un document à l'autre.

Ces deux points seront ajoutés à ma liste de questions pour les parties prenantes, en
complément de celles déjà posées dans l'Exercice 2.

---

## 🎓 Conclusion de la Phase 1

Ces trois exercices m'ont permis de parcourir les toutes premières étapes fondamentales de
la conception d'une base de données :

1. **Cadrer le projet** en identifiant les informations à recueillir et la manière de
   communiquer avec les parties prenantes (Exercice 1)
2. **Analyser les documents existants** pour transformer des outils informels (papier,
   tableur, mémoire des employés) en questions concrètes et en découvertes de modélisation
   (Exercice 2)
3. **Extraire et classer systématiquement** chaque donnée utile, en anticipant déjà des
   décisions techniques (formats, identifiants, données calculées) (Exercice 3)
4. **Confronter mon travail à une référence externe**, assumer un désaccord argumenté plutôt
   que de suivre un corrigé sans réflexion critique (ce récapitulatif)

> 💡 **Pourquoi cette dernière compétence compte autant que les autres ?**
> Dans un vrai projet professionnel, il n'existe pas toujours une "bonne réponse" unique.
> Un concepteur de base de données compétent doit savoir **justifier ses choix techniques**
> avec des arguments solides, y compris face à un document de référence — tant que le
> raisonnement est cohérent et documenté.

## 🎯 Prochaine étape

Avec cette Phase 1 terminée, je dispose de tout le matériau nécessaire pour construire le
**dictionnaire de données** et identifier formellement les **entités, attributs et relations**
de la future base — ce sera l'objet de la **Phase 2 : Modélisation conceptuelle**.