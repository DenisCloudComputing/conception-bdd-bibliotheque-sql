# 📘 Exercice 3 — Identification des informations importantes

## 🎯 Objectif

À partir des documents analysés dans l'Exercice 2, je dois maintenant **extraire précisément**
chaque donnée utile, en trois temps :

1. **Repérer** les champs pertinents pour le suivi du stock, les ventes et le planning
2. **Annoter** chaque champ (nom court, réutilisable dans une base de données)
3. **Classer le format** de chaque donnée : `Caractères` (texte), `Numérique standard` (entier) ou `Décimal`

> 💡 **Pourquoi classer les formats dès maintenant ?**
> Anticiper le format d'une donnée évite les erreurs classiques de conception : stocker un
> prix en texte (impossible de faire des calculs ou des tris corrects), ou un code produit
> en nombre entier (perte des zéros au début, comme `00123` qui devient `123`). Ce réflexe
> prépare directement le choix des **types de données SQL** (`VARCHAR`, `INT`, `DECIMAL`...)
> qu'on utilisera en Phase 4.

---

## 🧭 Légende des formats utilisés

| Format | Définition | Exemples |
|---|---|---|
| **Caractères** | Texte libre ou codifié, jamais utilisé pour calculer | `Nikki Wolf`, `AnyCompany, Inc.` |
| **Numérique standard** | Nombre entier, utilisable pour compter ou identifier | `144` (quantité), `2021` (année) |
| **Décimal** | Nombre avec partie fractionnaire, presque toujours pour l'argent | `7,99` (prix), `5,14 %` (taux) |

---

## 📦 Document 1 — Formulaire d'intégration au stock

| Donnée annotée | Exemple observé | Format | Justification |
|---|---|---|---|
| `date_reception` | 13/7/2022 | Caractères* | Une date est stockée avec un type `DATE` dédié en SQL, mais à ce stade on la note comme donnée textuelle brute (*voir note ci-dessous) |
| `nom_auteur_artiste` | Márcia Olivera | Caractères | Texte libre, jamais utilisé dans un calcul |
| `titre` | Les algorithmes probabilistes | Caractères | Texte libre |
| `annee_publication` | 2021 | Numérique standard | Un nombre entier, mais **jamais additionné ou moyenné** — c'est une info descriptive |
| `editeur` | AnyCompany, Inc. | Caractères | Texte libre |
| `type_produit` | Livre relié | Caractères | Catégorie fermée (liste de choix), donc du texte codifié |
| `cup` | 99361150799 | Caractères | ⚠️ Bien que composé de chiffres, un code produit **ne doit jamais être stocké comme un nombre** (risque de perdre des zéros non significatifs, et il ne sert jamais à un calcul) |
| `isbn` | 0-449-22361-2 | Caractères | Contient des tirets → obligatoirement du texte |
| `prix_vente` | 7,99 USD | Décimal | Valeur monétaire, utilisée dans des calculs (totaux, bénéfices) |
| `quantite_recue` | 144 | Numérique standard | Nombre entier, utilisé pour des calculs (additions de stock) |

> 📌 **Note sur les dates :** dans ce document de travail préparatoire, je note les dates comme
> "Caractères" par simplification, car l'objectif ici est juste de **repérer** les données. Le
> type SQL précis (`DATE`, `DATETIME`) sera tranché en Phase 3 (Modélisation logique).

---

## 📊 Document 2 — Feuille de calcul Stock

| Donnée annotée | Exemple observé | Format | Justification |
|---|---|---|---|
| `cup` | 45458987668 | Caractères | Même raisonnement que ci-dessus : code identifiant, jamais calculé |
| `nom_article` | Le b.a.-ba de l'électronique grand public | Caractères | Texte libre |
| `qte_dernier_inventaire` | 6 | Numérique standard | Nombre entier utilisé dans un calcul (`disponible = inventaire − ventes`) |
| `qte_ventes_depuis` | 1 | Numérique standard | Idem, entier utilisé dans un calcul |
| `qte_disponible` | 5 | Numérique standard | ⚠️ Donnée **calculée** (voir Exercice 2) — à confirmer si elle doit être stockée ou recalculée à la demande |

---

## 🧾 Document 3 — Reçu client

| Donnée annotée | Exemple observé | Format | Justification |
|---|---|---|---|
| `nom_magasin` | Votre librairie locale | Caractères | Texte fixe, propriété du magasin |
| `adresse_magasin` | 1234 Amazon Way, Seattle, WA, 98101 | Caractères | Texte |
| `date_transaction` | 1/8/2020 | Caractères* | Voir note sur les dates plus haut |
| `heure_transaction` | 16h04 | Caractères* | Idem, sera typée précisément en Phase 3 |
| `numero_caisse` | 001 | Caractères | ⚠️ Le `0` en tête serait perdu si stocké en nombre → texte |
| `numero_transaction` | 54826 | Numérique standard | Sert d'identifiant unique, pas de zéro significatif ici, mais **jamais utilisé dans un calcul** |
| `nom_vendeur` | Stiles | Caractères | Texte libre |
| `cup` (par ligne) | 343298349 | Caractères | Identifiant produit |
| `nom_article` (par ligne) | Niki Wolf | Caractères | Texte libre |
| `quantite` (par ligne) | 1 | Numérique standard | Entier, utilisé pour calculer le sous-total |
| `remise` (par ligne) | *(vide dans l'exemple)* | Décimal | Valeur monétaire potentielle |
| `prix_unitaire` (par ligne) | 28,50 | Décimal | Valeur monétaire |
| `sous_total` | 45,99 USD | Décimal | Valeur monétaire calculée |
| `taux_taxe` | 5,14 % | Décimal | Pourcentage, format décimal |
| `montant_taxe` | 2,36 USD | Décimal | Valeur monétaire calculée |
| `total` | 48,35 USD | Décimal | Valeur monétaire calculée |

---

## 📈 Document 4 — Compte rendu des ventes mensuelles

| Donnée annotée | Exemple observé | Format | Justification |
|---|---|---|---|
| `date_rapport` | 29 juillet 2020 | Caractères* | Voir note sur les dates |
| `cup` | 45458987668 | Caractères | Identifiant produit |
| `nom_article` | Le b.a.-ba de l'électronique grand public | Caractères | Texte libre |
| `description_type` | Livre relié | Caractères | Catégorie de produit |
| `quantite_vendue` | 3 | Numérique standard | Entier, utilisé pour des totaux |
| `prix_achat` | 10,00 | Décimal | Valeur monétaire — donnée **nouvelle**, absente du formulaire d'intégration (voir Exercice 2) |
| `prix_vente` | 23,99 | Décimal | Valeur monétaire |
| `benefice_net` | 13,99 | Décimal | Valeur monétaire calculée (`prix_vente − prix_achat`) |

---

## 📅 Document 5 — Calendrier de bureau (planning des événements)

> ⚠️ Ce document est décrit verbalement (pas de champs visibles sur un formulaire), donc les
> annotations ci-dessous s'appuient sur les **besoins exprimés** par la responsable.

| Donnée annotée | Exemple / besoin exprimé | Format | Justification |
|---|---|---|---|
| `date_evenement` | (à définir lors de la réservation) | Caractères* | Voir note sur les dates |
| `heure_debut` / `heure_fin` | (à définir) | Caractères* | Idem |
| `type_evenement` | Dédicace d'auteur / Venue de musicien | Caractères | Catégorie fermée |
| `nom_intervenant` | Nom de l'auteur ou de l'artiste | Caractères | Texte libre |
| `titre_evenement` | (ex : "Rencontre avec Márcia Olivera") | Caractères | Texte libre |
| `description` | Détails de l'événement pour le site web | Caractères | Texte libre, potentiellement long |
| `produits_associes` | Livre de l'auteur / CD de l'artiste (référence au `cup`) | Caractères | Nécessaire pour le rappel de commande de stock |
| `delai_rappel_commande` | Nombre de jours avant l'événement | Numérique standard | Entier, utilisé pour calculer une date de rappel |
| `statut_evenement` | Prévu / Confirmé / Annulé | Caractères | Catégorie fermée |

---

## 🗂️ Synthèse par exigence commune

### 1️⃣ Informations nécessaires au **suivi du stock**

| Donnée | Documents sources |
|---|---|
| `cup` | Formulaire, Feuille Stock, Reçu, Compte rendu |
| `nom_article` | Formulaire, Feuille Stock, Reçu, Compte rendu |
| `type_produit` | Formulaire, Compte rendu |
| `nom_auteur_artiste` | Formulaire |
| `titre`, `annee_publication`, `editeur`, `isbn` | Formulaire |
| `prix_vente` / `prix_achat` | Formulaire (vente), Compte rendu (achat) |
| `qte_dernier_inventaire`, `qte_ventes_depuis`, `qte_disponible` | Feuille Stock |
| `quantite_recue` | Formulaire |

### 2️⃣ Informations nécessaires au **compte rendu des ventes**

| Donnée | Documents sources |
|---|---|
| `date_transaction`, `heure_transaction` | Reçu |
| `numero_transaction`, `numero_caisse` | Reçu |
| `nom_vendeur` | Reçu |
| `cup`, `nom_article`, `quantite` (par ligne vendue) | Reçu |
| `prix_unitaire`, `remise`, `sous_total`, `taux_taxe`, `total` | Reçu |
| `prix_achat`, `benefice_net` | Compte rendu mensuel |

### 3️⃣ Informations nécessaires au **planning des événements**

| Donnée | Documents sources |
|---|---|
| `date_evenement`, `heure_debut`, `heure_fin` | Calendrier |
| `type_evenement`, `statut_evenement` | Calendrier |
| `nom_intervenant` | Calendrier |
| `titre_evenement`, `description` | Calendrier |
| `produits_associes` | Calendrier (lien vers le stock) |
| `delai_rappel_commande` | Calendrier (lien vers le stock) |

---

## 💡 JUSTIFICATION globale de la démarche

Cette **annotation systématique** transforme des documents papier ou tableurs hétérogènes en
une **liste homogène et exploitable** de données candidates. C'est un prérequis indispensable
avant de construire le **dictionnaire de données** (Phase 2) : on ne peut pas identifier les
entités et leurs attributs tant qu'on n'a pas listé, un par un, tous les éléments d'information
en jeu.

La classification par **format** (Caractères / Numérique / Décimal), bien que sommaire à ce
stade, anticipe déjà des décisions techniques importantes :
- Elle évite de traiter des **identifiants comme des nombres** (perte de zéros, code CUP/ISBN).
- Elle distingue les **valeurs monétaires** (décimal) des **quantités** (entier), évitant
  des erreurs d'arrondi ou de calcul plus tard.
- Elle met en lumière les données **calculées** (`qte_disponible`, `benefice_net`) qu'il
  faudra décider de **stocker ou de recalculer à la demande** — un choix de conception
  classique en base de données (compromis entre performance et cohérence des données).

## 🎯 Prochaine étape

Ces trois tableaux de synthèse constituent la base directe du **dictionnaire de données**
que je construirai en Phase 2 (Modélisation conceptuelle), où chaque donnée sera regroupée
par **entité** (PRODUIT, VENTE, ÉVÉNEMENT, AUTEUR, EMPLOYÉ...) avant de tracer le MCD.