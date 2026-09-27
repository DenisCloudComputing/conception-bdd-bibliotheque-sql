# 📘 Exercice 2 — Analyse des ressources existantes : quelles questions poser ?

## 🎯 Contexte

Lors de la réunion de lancement, les propriétaires et responsables de la librairie m'ont
fourni les **5 ressources** qu'ils utilisent actuellement au quotidien :

1. **Formulaire d'intégration au stock** (papier)
2. **Feuille de calcul Stock** (partagée)
3. **Reçu client** (exemple de ticket de caisse)
4. **Compte rendu des ventes mensuelles** (produit manuellement)
5. **Calendrier de bureau** (planning des événements)

## 🧠 Méthode utilisée : l'analyse documentaire

Pour chaque ressource, j'applique systématiquement la même démarche en 3 temps :
1. **Observer** : que révèle réellement ce document (y compris ce qu'on ne m'a pas dit) ?
2. **Questionner** : quelles questions dois-je poser pour lever les ambiguïtés ?
3. **Justifier** : pourquoi ces questions sont-elles déterminantes pour la modélisation ?

> 💡 L'analyse des documents existants est une technique fondamentale du recueil des besoins :
> les documents actuels révèlent les **données réelles du métier**, y compris celles que les
> personnes oublient de mentionner en entretien.

---

## 📦 Ressource 1 — Formulaire d'intégration au stock

> **AFFIRMATION (QUESTION) :** Quelles questions puis-je poser aux responsables de la
> librairie concernant le formulaire papier utilisé pour ajouter des produits au stock ?

### 🔍 Ce que j'observe dans le document

| Champ observé | Remarque d'analyste |
|---------------|---------------------|
| Date (13/7/2022) | Chaque entrée en stock est datée |
| Auteur(s) ou artiste(s) | Le **pluriel** indique qu'un produit peut avoir **plusieurs** auteurs |
| Titre, Année de publication, Éditeur | Attributs descriptifs du produit |
| Type de produit | Seulement 4 choix : livre relié, livre de poche, Blu-ray, vinyle |
| CUP (99361150799) | Code présent pour **tous** les produits |
| ISBN (0-449-22361-2) | Code renseigné **uniquement pour les livres** |
| Prix de vente (7,99 USD) | Le **prix d'achat n'apparaît pas** sur ce formulaire ! |
| Quantité reçue (144) | Notion de livraison/réception |

### ✅ Solution — Mes questions aux responsables

**Sur l'identification des produits :**
1. Le CUP est-il **unique** pour chaque produit ? Est-ce lui votre identifiant principal ?
2. Quelle est la différence entre le CUP et l'ISBN ? Faut-il conserver les deux pour les livres ?

**Sur les attributs :**
3. Un produit peut-il avoir **plusieurs auteurs ou artistes** ? Si oui, comment les notez-vous aujourd'hui ?
4. Les 4 types de produits sont-ils les **seuls** vendus ? (⚠️ le reçu client montre une *boisson* !)
5. Le prix de vente d'un produit peut-il **changer au cours du temps** ? Faut-il garder un historique ?

**Sur l'approvisionnement :**
6. Où est enregistré le **prix d'achat** ? (absent du formulaire, mais présent dans le compte rendu des ventes !)
7. Faut-il enregistrer le **fournisseur** de chaque livraison ?
8. Que se passe-t-il quand vous recevez une **nouvelle livraison d'un produit déjà en stock** ?

**Sur le processus :**
9. Qui remplit ce formulaire ? Faut-il tracer **quel employé** a fait chaque entrée en stock ?
10. Le formulaire papier est-il ressaisi tel quel dans la feuille de calcul ? Par qui, et à quel moment ?

### 💡 JUSTIFICATION

- **CUP vs ISBN :** la coexistence de deux codes est un piège classique. Il faut déterminer
  lequel servira d'**identifiant unique** (future clé primaire). Le CUP semble universel
  (tous produits), l'ISBN n'existe que pour les livres → le CUP est le meilleur candidat.
- **"Auteur(s)" au pluriel** : c'est l'indice d'une future **relation plusieurs-à-plusieurs**
  entre PRODUIT et AUTEUR (un livre a plusieurs auteurs, un auteur a écrit plusieurs livres).
  Découverte majeure pour le modèle conceptuel.
- **Le prix d'achat manquant** révèle une donnée gérée ailleurs — ou pas gérée du tout.
  Sans cette question, le futur modèle aurait un **trou** impossible à combler plus tard.
- **La boisson du reçu** contredit la liste fermée des 4 types : la liste des catégories
  n'est donc pas figée. Il faut la confirmer, sinon le modèle sera trop rigide.

---

## 📊 Ressource 2 — Feuille de calcul Stock

> **AFFIRMATION (QUESTION) :** Quelles questions supplémentaires puis-je poser par rapport
> à la feuille de calcul Stock ?

### 🔍 Ce que j'observe dans le document

- Colonnes : `CUP | Nom de l'article | Dernier inventaire | Ventes depuis | Disponible`
- La colonne **Disponible semble calculée** : `Dernier inventaire − Ventes depuis`
  (ex. : 6 − 1 = 5 ✅)
- **Aucun prix** n'apparaît dans cette feuille
- Les **réapprovisionnements entre deux inventaires** ne semblent pas pris en compte
- Plusieurs articles semblent être des volumes d'une même série (Crystal Vol. 1 et 2, CUP consécutifs)

### ✅ Solution — Mes questions aux responsables

**Sur la fiabilité du stock :**
1. À quelle **fréquence** faites-vous l'inventaire physique du magasin ?
2. Que faites-vous quand le stock réel **diffère du calcul** (casse, vol, erreur de saisie) ?

**Sur le réapprovisionnement :**
3. Comment les **livraisons reçues entre deux inventaires** sont-elles reflétées dans la feuille ?
4. Existe-t-il un **seuil d'alerte** en dessous duquel vous recommandez un produit ? Qui décide ?

**Sur l'usage de la feuille :**
5. Pourquoi le **prix** n'apparaît-il pas ici ? Où le consultez-vous quand un client le demande ?
6. **Plusieurs employés** modifient-ils cette feuille en même temps ? Avez-vous déjà eu des conflits ou des pertes de données ?

### 💡 JUSTIFICATION

- **"Disponible" est une donnée calculée (dérivée)** : en base de données, on évite de
  stocker ce qui peut être recalculé à partir d'autres données, pour ne pas créer
  d'**incohérences** (si deux colonnes se contredisent, laquelle croire ?).
- La formule actuelle (`inventaire − ventes`) **ignore les livraisons reçues** entre deux
  inventaires → le stock théorique devient **faux** au fil du temps. La future base devra
  donc enregistrer des **mouvements de stock datés** (entrées ET sorties), pas seulement
  un compteur.
- La feuille partagée modifiée par plusieurs personnes est une source classique d'**écrasements
  de données** → c'est un argument concret qui justifie le passage à une vraie base centralisée.

---

## 🧾 Ressource 3 — Reçu client

> **AFFIRMATION (QUESTION) :** Quelles questions supplémentaires puis-je poser par rapport
> au reçu électronique client ?

### 🔍 Ce que j'observe dans le document

- **En-tête de transaction** : magasin, date ET heure, n° de caisse (001), n° de transaction (54826), **nom du vendeur** (Stiles)
- **Lignes d'articles** : CUP, nom, quantité, **remise** (colonne vide ici), prix
- ⚠️ Une ligne est une **« Boisson moyenne »** → ni un livre, ni un disque !
- **Pied** : sous-total, taxes (5,14 %), total
- Les CUP affichés semblent **tronqués** (9 chiffres au lieu de 11 sur les autres documents)

### ✅ Solution — Mes questions aux responsables

**Sur la transaction :**
1. Le numéro de transaction est-il **unique et séquentiel** ? Faut-il tracer la caisse utilisée ?
2. Faut-il enregistrer **le vendeur** de chaque vente ? (commissions, suivi de performance ?)

**Sur le détail de la vente :**
3. Comment fonctionnent les **remises** : par article, par transaction, par client fidèle ?
4. Un client peut-il acheter **plusieurs exemplaires** du même article dans une même transaction ?

**Sur le catalogue :**
5. Vous vendez des **boissons** ! Quels **autres types de produits** vendez-vous qui n'apparaissent pas dans le formulaire d'intégration au stock ?

**Sur les règles financières :**
6. Le taux de taxe (5,14 %) est-il **identique pour tous les produits** ? Peut-il évoluer ?
7. Quels **moyens de paiement** acceptez-vous ? Faut-il les enregistrer ?

**Sur le cycle de vie :**
8. Comment gérez-vous les **retours et remboursements** ? Sont-ils rattachés à la vente d'origine ?

### 💡 JUSTIFICATION

- Un reçu illustre la structure la plus classique du commerce : un **en-tête** (la vente)
  qui contient plusieurs **lignes** (les articles vendus). C'est la base des futures tables
  `VENTE` et `LIGNE_DE_VENTE`, reliées à `PRODUIT`.
- Le **vendeur identifié** nommément implique une entité `EMPLOYÉ` dans le modèle — un
  besoin qui n'apparaissait dans aucun autre document.
- La **boisson** prouve que le catalogue dépasse les 4 types du formulaire d'intégration :
  sans ce reçu, j'aurais modélisé un catalogue trop restrictif.
- La colonne **Remise vide mais existante** montre une fonctionnalité prévue mais peu
  utilisée : il faut prévoir l'attribut sans pour autant le rendre obligatoire.
- La **taxe** ne doit jamais être « codée en dur » : c'est un paramètre qui peut changer
  (loi, type de produit) → donnée à stocker, pas une constante.

---

## 📈 Ressource 4 — Compte rendu des ventes mensuelles

> **AFFIRMATION (QUESTION) :** Quelles questions supplémentaires puis-je poser par rapport
> au compte rendu des ventes mensuelles ?

### 🔍 Ce que j'observe dans le document

- Colonnes : `CUP | Nom | Description (type) | Quantité | Prix d'achat | Prix de vente | Bénéfice net`
- Le **prix d'achat apparaît ici pour la première fois** (absent du formulaire d'intégration !)
- Le rapport est construit **manuellement** à partir de **3 outils différents** : une feuille
  de calcul, un traitement de texte, et les journaux de l'**ancien système de point de vente**
- Seulement 6 lignes visibles → extrait ? Sélection des meilleures ventes ?

### ✅ Solution — Mes questions aux responsables

**Sur les données :**
1. Où le **prix d'achat** est-il enregistré aujourd'hui ? Peut-il **varier d'une livraison à l'autre** pour un même produit ?
2. Le **bénéfice net** = prix de vente − prix d'achat ? Ou tient-il aussi compte des taxes et des remises ?

**Sur le contenu du rapport :**
3. Ce rapport liste-t-il **toutes les ventes du mois** ou une sélection ? Selon quel critère ?
4. Quels **autres rapports** vous seraient utiles (par catégorie, par auteur, par période, par vendeur) ?

**Sur le processus :**
5. **Combien de temps** prend la génération manuelle de ce rapport chaque mois ?
6. Qui le reçoit, et **quelles décisions** sont prises à partir de lui ?

**Sur l'historique :**
7. Faut-il **migrer les données de l'ancien système** de point de vente dans la nouvelle base ? Sur combien d'années ?

### 💡 JUSTIFICATION

- Ce rapport révèle le **besoin décisionnel réel** des propriétaires : la **rentabilité par
  produit**. La base devra donc stocker le prix d'achat ET le prix de vente — or le prix
  d'achat n'existait dans aucun autre document. Sans cette ressource, le modèle aurait été
  incomplet.
- Le **bénéfice net est une donnée calculée** : il pourra être produit automatiquement par
  une requête SQL (ou une vue), sans être stocké.
- Le processus **manuel et multi-outils** représente un coût (temps) et un risque (erreurs
  de recopie). C'est un argument fort pour l'**automatisation des rapports** via SQL — un
  bénéfice concret du projet, mesurable en heures gagnées.
- La question de la **migration des données historiques** a un impact direct sur le
  périmètre, le planning et le coût du projet : elle doit être tranchée tôt.

---

## 📅 Ressource 5 — Calendrier de bureau (planning des événements)

> **AFFIRMATION (QUESTION) :** Quelles questions supplémentaires puis-je poser par rapport
> au calendrier des séances de dédicaces, venues de musiciens et autres événements ?

### 🔍 Ce que j'observe dans le document / les propos de la responsable

- Le calendrier gère **deux types d'événements** : dédicaces d'auteurs et venues de musiciens
- Besoin exprimé n°1 : **publier le calendrier sur le site web** du magasin
- Besoin exprimé n°2 : recevoir un **rappel pour commander** les livres de l'auteur ou les
  CD de l'artiste **avant l'événement**, afin d'avoir du stock

### ✅ Solution — Mes questions aux responsables

**Sur la description d'un événement :**
1. Quelles informations décrivent un événement (titre, date, heure de début/fin, description, emplacement dans le magasin) ?
2. Un événement peut-il accueillir **plusieurs auteurs ou artistes** à la fois ?
3. Faut-il gérer un **statut** (prévu, confirmé, reporté, annulé) — utile pour la publication web ?

**Sur le lien avec le catalogue et le stock :**
4. Comment **reliez-vous un événement aux produits concernés** (les livres de l'auteur, les CD de l'artiste) ? Un événement peut-il concerner plusieurs produits ?
5. **Combien de jours à l'avance** le rappel de commande doit-il arriver ? **Qui** doit le recevoir ?

**Sur le public :**
6. Les événements sont-ils **gratuits ou payants** ? Y a-t-il une **capacité maximale** ? Les clients doivent-ils **s'inscrire** ?

**Sur la gestion :**
7. Qui est **autorisé à créer ou modifier** un événement ?
8. Faut-il conserver l'**historique des événements passés** ? Souhaitez-vous mesurer la **fréquentation** ?

### 💡 JUSTIFICATION

- L'événement est une **nouvelle entité** qui n'apparaît dans aucun autre document, et elle
  est reliée **à la fois** aux auteurs/artistes **et** aux produits → des relations
  supplémentaires à modéliser, insoupçonnées avant cet échange.
- Le **rappel de commande** crée un lien fonctionnel entre le **planning** et le **stock** :
  ce croisement est impossible avec un calendrier papier, mais devient naturel avec une base
  centralisée. C'est l'un des bénéfices majeurs du projet.
- La **publication web** impose des données **structurées et à jour** (titre, date, statut,
  description) : on ne peut pas publier proprement un calendrier griffonné.
- La question des **inscriptions clients** teste le périmètre : si la réponse est oui, il
  faudra des entités `CLIENT` et `INSCRIPTION` ; sinon, on évite de **sur-dimensionner**
  le modèle. Poser la question ne coûte rien ; l'oublier peut coûter une refonte.

---

## 🎯 Synthèse — Ce que l'analyse documentaire a révélé

| 🔎 Découverte | 📄 Ressource source | 🧱 Impact sur la modélisation |
|---|---|---|
| La librairie vend des **boissons** | Reçu client | Catégories de produits **extensibles** (pas une liste figée) |
| Le **prix d'achat** existe mais n'est nulle part enregistré formellement | Compte rendu des ventes | Attribut à ajouter (produit ou livraison) |
| Un produit peut avoir **plusieurs auteurs** | Formulaire d'intégration | Relation **plusieurs-à-plusieurs** PRODUIT ↔ AUTEUR |
| Chaque vente est faite par un **vendeur identifié** | Reçu client | Entité **EMPLOYÉ** à créer |
| Le stock « Disponible » est **calculé** et ignore les livraisons | Feuille Stock | Gérer des **mouvements de stock datés**, pas un simple compteur |
| Le planning doit **déclencher des commandes** de stock | Calendrier | Lien **ÉVÉNEMENT ↔ PRODUIT** à modéliser |
| Les rapports sont faits **manuellement avec 3 outils** | Compte rendu | Rapports **automatisables** par requêtes SQL |
| Un **ancien système** contient l'historique des ventes | Compte rendu | Question de **migration de données** à trancher |

## ✅ Conclusion

Cet exercice démontre que chaque document métier recèle des **indices de modélisation**
qu'un simple entretien n'aurait pas révélés (la boisson, le prix d'achat, le pluriel de
"Auteur(s)"...). La prochaine étape consiste à formaliser ces découvertes : c'est l'objet
de l'**Exercice 3 — Identification des informations importantes**.