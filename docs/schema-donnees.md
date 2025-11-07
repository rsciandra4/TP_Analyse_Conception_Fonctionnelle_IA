# Schéma de données — Base dédiée (Partie 4)

**Projet ESGI M2 — DDD & IA**  
*Auteurs : Romain SCIANDRA & Omar MOSTAFA*

---

## Vue d’ensemble (MCD textuel)

clients (1) ── (N) devis (1) ── (N) lignes_devis
devis (1)  ── (N) factures (1) ── (N) paiements

categories (1) ── (N) sub_categories
sub_categories (1) ── (N) lignes_devis

* **Granularité document** :

  * `devis` = en-tête (contexte commercial).
  * `factures` = en-tête comptable.
* **Granularité ligne** :

  * `lignes_devis` = détail des prestations / sous-catégories par devis.
  * `paiements` = mouvements financiers rattachés à une facture.

---

## 1. Table `clients`

**Source principale** : `acces_pro_client`
**Rôle** : référentiel des clients (B2B/B2C) utilisés par `devis` et `factures`.

| Colonne       | Type logique | Clé / Index    | Source / Remarque     |
| :------------ | :----------- | :------------- | :-------------------- |
| id            | INT          | PK             | `acces_pro_client.id` |
| reference     | VARCHAR      | UNIQUE INDEX   | `reference`           |
| civility      | VARCHAR      | INDEX          | `civility`            |
| firstname     | VARCHAR      |                | `firstname`           |
| lastname      | VARCHAR      |                | `lastname`            |
| email_address | VARCHAR      | INDEX          | `email_address`       |
| phone         | VARCHAR      |                | `phone` / `cellular`  |
| address       | VARCHAR      |                | `address`             |
| postal_code   | VARCHAR      |                | `postal_code`         |
| city          | VARCHAR      |                | `city`                |
| is_deleted    | BOOLEAN      | INDEX (filtre) | `is_delete`           |
| created_at    | DATETIME     | INDEX (audit)  | `created_at`          |
| updated_at    | DATETIME     |                | `updated_at`          |

**Justification clés & index**

* `PK(id)` : identifie de manière stable chaque client.
* `UNIQUE(reference)` : réutilise l’identifiant métier existant.
* Index sur `email_address`, `is_deleted`, `created_at` utiles pour recherche & filtrage.

---

## 2. Table `categories`

**Source** : `acces_pro_category`
**Rôle** : référentiel de catégories comptables / métier.

| Colonne          | Type logique | Clé / Index | Source             |
| :--------------- | :----------- | :---------- | :----------------- |
| id               | INT          | PK          | `id`               |
| name             | VARCHAR      | UNIQUE      | `name`             |
| code_nature      | INT          | INDEX       | `code_nature`      |
| lettre           | VARCHAR      |             | `lettre`           |
| compte_comptable | INT          |             | `compte_comptable` |
| tva              | DECIMAL      |             | `tva`              |
| is_deleted       | BOOLEAN      | INDEX       | `is_deleted`       |

**Justification**

* Table **stable**, largement réutilisée (devis, lignes_devis, analytique).
* Index sur `code_nature` pertinent pour les analyses comptables.

---

## 3. Table `sub_categories`

**Source** : `acces_pro_sub_category`
**Rôle** : affiner la nature des prestations rattachées aux lignes de devis.

| Colonne     | Type logique | Clé / Index        | Source        |
| :---------- | :----------- | :----------------- | :------------ |
| id          | INT          | PK                 | `id`          |
| category_id | INT          | FK → categories.id | `category_id` |
| name        | VARCHAR      | INDEX              | `name`        |
| ordre       | INT          |                    | `ordre`       |
| is_default  | BOOLEAN      |                    | `is_default`  |

**Justification**

* Permet le mapping propre des prestations par typologie.
* `FK(category_id)` indispensable pour cohérence.

---

## 4. Table `devis`

**Sources** : `acces_pro_devis_with_prestation` (partie en-tête), `acces_pro_client`
**Rôle** : document commercial global (niveau devis).

| Colonne       | Type logique | Clé / Index        | Source / Remarque                    |
| :------------ | :----------- | :----------------- | :----------------------------------- |
| id            | INT          | PK                 | `acces_pro_devis_with_prestation.id` |
| reference     | VARCHAR      | UNIQUE INDEX       | `reference`                          |
| client_id     | INT          | FK → clients.id    | `client_id`                          |
| date_creation | DATETIME     | INDEX              | `date_heure_creation`                |
| status        | VARCHAR      | INDEX              | `status`                             |
| total_ht      | DECIMAL      |                    | `total_ht` (en-tête)                 |
| total_ttc     | DECIMAL      |                    | `total_ttc`                          |
| tva           | DECIMAL      |                    | `tva`                                |
| categorie_id  | INT          | FK → categories.id | `categorie_id` (si présent)          |

**Granularité**

* 1 ligne par **devis**.
* Les détails par prestation/ligne sont déportés dans `lignes_devis`.

**Index clés**

* `UNIQUE(reference)` pour retrouver rapidement un devis métier.
* `INDEX(client_id, date_creation)` pour filtre client/période.

---

## 5. Table `lignes_devis`

**Sources** :

* `acces_pro_devis_with_prestation` (colonnes article / qty / pu),
* `acces_pro_devis_sub_category` (lignes par sous-catégorie).

**Rôle** : stocker les lignes détaillées rattachées à un devis.

| Colonne         | Type logique | Clé / Index            | Source                           |
| :-------------- | :----------- | :--------------------- | :------------------------------- |
| id              | INT          | PK                     | [NOUVEAU] (clé technique)        |
| devis_id        | INT          | FK → devis.id, INDEX   | `devis_id` / `id` (legacy)       |
| sub_category_id | INT          | FK → sub_categories.id | `sub_category_id`                |
| description     | VARCHAR      |                        | `designation_article` / `name`   |
| qty             | DECIMAL      |                        | `qty_article` / `waste_quantity` |
| unit_price_ht   | DECIMAL      |                        | `pu_article`                     |
| total_ht        | DECIMAL      |                        | `total_ht`                       |
| total_ttc       | DECIMAL      |                        | `total_ttc`                      |
| tva             | DECIMAL      |                        | `tva`                            |

**Granularité**

* 1 ligne = 1 prestation / sous-catégorie sur un devis.
* Séparation **document (`devis`)** / **ligne (`lignes_devis`)** :

  * facilite calculs, reporting, IA (type de service, pricing, etc.).

**Index**

* `INDEX(devis_id)` obligatoire.
* `INDEX(sub_category_id)` utile pour analyses par type de prestation.

---

## 6. Table `factures`

**Source principale** : `acces_pro_echeance_devis_2022`
**Interprétation** : ce tableau représente des montants liés à un `devis_id` et une `date`.
Dans la base dédiée, on l’utilise comme base de **factures simples** (1 devis → N factures possibles).

| Colonne  | Type logique | Clé / Index          | Source                     |
| :------- | :----------- | :------------------- | :------------------------- |
| id       | INT          | PK                   | `id`                       |
| devis_id | INT          | FK → devis.id, INDEX | `devis_id`                 |
| date     | DATE         | INDEX                | `date`                     |
| montant  | DECIMAL      |                      | `montant`                  |
| duree    | VARCHAR      |                      | `duree` (si échelonnement) |

**Granularité**

* 1 ligne = 1 facture ou 1 échéance associée à un devis.
* Permet de gérer le cas : plusieurs factures / échéances pour un même devis.

---

## 7. Table `paiements`

**Constat dataset** : pas de table `paiements` dédiée ; seules les colonnes d’échéances existent.
**Choix cible** : créer une table `paiements` pour suivre les flux réels (à détailler dans `gaps-analysis.md`).

| Colonne           | Type logique | Clé / Index             | Source / Remarque |
| :---------------- | :----------- | :---------------------- | :---------------- |
| id                | INT          | PK                      | [NOUVEAU]         |
| facture_id        | INT          | FK → factures.id, INDEX | [NOUVEAU]         |
| date_paiement     | DATE         |                         | [À collecter]     |
| montant_paye      | DECIMAL      |                         | [À collecter]     |
| mode_paiement     | VARCHAR      |                         | [À collecter]     |
| reference_externe | VARCHAR      |                         | [À collecter]     |

**Justification**

* Nécessaire pour :

  * suivi métier (relances),
  * analyses IA (comportement de paiement),
* Les champs sont marqués comme **nouveaux** → documentés comme manque dans `gaps-analysis.md`.

---

## 8. Synthèse : Clés & Index (essentiels)

* `clients` :

  * `PK(id)`
  * `UNIQUE(reference)`
  * index sur `email_address`, `created_at`, `is_deleted`
* `categories` / `sub_categories` :

  * `PK(id)`
  * `FK(sub_categories.category_id → categories.id)`
* `devis` :

  * `PK(id)`
  * `UNIQUE(reference)`
  * `FK(client_id → clients.id)`
  * index `(client_id, date_creation)`
* `lignes_devis` :

  * `PK(id)`
  * `FK(devis_id → devis.id)`
  * `FK(sub_category_id → sub_categories.id)`
* `factures` :

  * `PK(id)`
  * `FK(devis_id → devis.id)`
  * index sur `date`
* `paiements` (cible) :

  * `PK(id)`
  * `FK(facture_id → factures.id)`

Ce schéma :

* reste **aligné sur les données réelles** (`acces_pro_*`),
* simplifie le modèle en séparant clairement :

  * référentiels (`clients`, `categories`, `sub_categories`),
  * documents (`devis`, `factures`),
  * lignes (`lignes_devis`),
  * flux financiers (`paiements`),
* prépare la suite : analyse des **manques métier & IA** dans `docs/gaps-analysis.md`.

---