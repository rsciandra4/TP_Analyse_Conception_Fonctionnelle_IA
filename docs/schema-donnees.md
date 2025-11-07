# Schéma de données — Base dédiée - Partie 4
*Auteurs : Romain SCIANDRA & Omar MOSTAFA*  

---
## Vue d’ensemble (MCD textuel)

```
clients (1) ── (N) devis (1) ── (N) factures (1) ── (N) paiements
                 │
                 └── (N) prestations
```

**Granularité :**

* `devis` = document principal (niveau en-tête)
* `prestations` = référentiel des types et sous-types de services proposés
* `factures` et `paiements` = suivi comptable et financier associé

---

## 1. Table `clients`

**Source principale :** `acces_pro_client`
**Rôle :** référentiel central des clients (B2B ou B2C).

| Colonne       | Type logique      | Clé / Index  | Source / Remarque                        |
| :------------ | :---------------- | :----------- | :--------------------------------------- |
| id            | INT               | PK           | `id`                                     |
| reference     | VARCHAR           | UNIQUE INDEX | `reference`                              |
| civility      | VARCHAR           |              | `civility`                               |
| firstname     | VARCHAR           |              | `firstname`                              |
| lastname      | VARCHAR           |              | `lastname`                               |
| email_address | VARCHAR           | INDEX        | `email_address`                          |
| phone         | VARCHAR           |              | `phone`                                  |
| address       | VARCHAR           |              | `address`                                |
| postal_code   | VARCHAR           |              | `postal_code`                            |
| city          | VARCHAR           |              | `city`                                   |
| type_client   | ENUM('B2B','B2C') | INDEX        | [NOUVEAU] permet de distinguer les types |
| is_deleted    | BOOLEAN           | INDEX        | `is_delete`                              |
| created_at    | DATETIME          | INDEX        | `created_at`                             |
| updated_at    | DATETIME          |              | `updated_at`                             |

**Justification :**

* La clé `id` identifie chaque client de manière unique.
* L’attribut `type_client` est introduit pour la segmentation (métier et IA).
* Index sur `email_address` et `created_at` pour faciliter les recherches et le reporting.

---

## 2. Table `prestations`

**Fusion de :** `acces_pro_category` + `acces_pro_sub_category`
**Rôle :** référentiel unique des prestations disponibles, avec leur **type** et **sous-type**, ainsi que les données de tarification et de TVA.

| Colonne              | Type logique  | Clé / Index | Source / Remarque                               |
| :------------------- | :------------ | :---------- | :---------------------------------------------- |
| id                   | INT           | PK          | `id_category` / `id_sub_category` fusionnés     |
| type_prestation      | VARCHAR       | INDEX       | Anciennement `category.name`                    |
| sous_type_prestation | VARCHAR       | INDEX       | Anciennement `sub_category.name`                |
| description          | TEXT          |             | `description_category` ou `designation_article` |
| code_nature          | VARCHAR       | INDEX       | `code_nature`                                   |
| taux_tva             | DECIMAL(4,2)  |             | `tva`                                           |
| compte_comptable     | VARCHAR       |             | `compte_comptable`                              |
| prix_unitaire_ht     | DECIMAL(10,2) |             | `pu_article` (si présent dans le CSV)           |
| unite_mesure         | VARCHAR       |             | `waste_unit` (hérité de prestations)            |
| is_default           | BOOLEAN       |             | `is_default`                                    |
| is_deleted           | BOOLEAN       | INDEX       | `is_deleted`                                    |

**Justification :**

* Le regroupement évite la redondance entre catégories et sous-catégories.
* Chaque ligne représente une **prestation unique** (ex. : type = "Nettoyage", sous-type = "Vitre").
* Les champs `prix_unitaire_ht`, `taux_tva` et `compte_comptable` permettent une cohérence comptable et analytique.

---

## 3. Table `devis`

**Source :** `acces_pro_devis_with_prestation`
**Rôle :** gérer le document commercial global (en-tête et lien vers les prestations proposées).

| Colonne         | Type logique                                               | Clé / Index         | Source / Remarque                             |
| :-------------- | :--------------------------------------------------------- | :------------------ | :-------------------------------------------- |
| id              | INT                                                        | PK                  | `id`                                          |
| reference       | VARCHAR                                                    | UNIQUE INDEX        | `reference`                                   |
| client_id       | INT                                                        | FK → clients.id     | `client_id`                                   |
| prestation_id   | INT                                                        | FK → prestations.id | [NOUVEAU] lien direct vers type de prestation |
| date_creation   | DATETIME                                                   | INDEX               | `date_heure_creation`                         |
| status          | ENUM('Brouillon','Envoyé','Accepté','Refusé','Transformé') | INDEX               | `status`                                      |
| montant_ht      | DECIMAL(10,2)                                              |                     | `total_ht`                                    |
| tva             | DECIMAL(4,2)                                               |                     | `tva`                                         |
| montant_ttc     | DECIMAL(10,2)                                              |                     | `total_ttc`                                   |
| commentaire     | TEXT                                                       |                     | `commentaire`                                 |
| date_signature  | DATE                                                       |                     | [NOUVEAU] (date d’acceptation du devis)       |
| date_expiration | DATE                                                       |                     | [NOUVEAU] (validité du devis)                 |

**Granularité :**

* 1 ligne = 1 devis.
* Le lien vers `prestations` remplace les anciennes lignes de devis (`lignes_devis` supprimée).

**Index & contraintes :**

* `UNIQUE(reference)` : identifiant métier du devis.
* `FK(client_id)` et `FK(prestation_id)` assurent l’intégrité métier.

---

## 4. Table `factures`

**Source :** `acces_pro_echeance_devis_2022`
**Rôle :** document comptable émis à partir d’un devis accepté.

| Colonne           | Type logique                                | Clé / Index   | Source / Remarque                   |
| :---------------- | :------------------------------------------ | :------------ | :---------------------------------- |
| id                | INT                                         | PK            | `id`                                |
| devis_id          | INT                                         | FK → devis.id | `devis_id`                          |
| date_facture      | DATE                                        | INDEX         | `date`                              |
| montant_ht        | DECIMAL(10,2)                               |               | `montant`                           |
| tva               | DECIMAL(4,2)                                |               | `tva`                               |
| montant_ttc       | DECIMAL(10,2)                               |               | calculé                             |
| statut_facture    | ENUM('Émise','Payée','En retard','Annulée') | INDEX         | dérivé de `statut`                  |
| duree_echeance    | VARCHAR                                     |               | `duree`                             |
| date_versement    | DATE                                        |               | `date_versement`                    |
| date_encaissement | DATE                                        |               | `date_encaissement`                 |
| remise_ht         | DECIMAL(10,2)                               |               | [NOUVEAU] (réductions commerciales) |

**Justification :**

* Le lien avec `devis` est direct via la clé `devis_id`.
* Les champs `remise_ht` et `statut_facture` renforcent la traçabilité commerciale.

---

## 5. Table `paiements`

**Nouveau modèle dédié** (aucune table native dans `acces_pro_*`)
**Rôle :** tracer les règlements associés aux factures.

| Colonne           | Type logique                           | Clé / Index      | Source / Remarque                      |
| :---------------- | :------------------------------------- | :--------------- | :------------------------------------- |
| id                | INT                                    | PK               | [NOUVEAU]                              |
| facture_id        | INT                                    | FK → factures.id | [NOUVEAU]                              |
| date_paiement     | DATE                                   | INDEX            | [À collecter]                          |
| montant_paye      | DECIMAL(10,2)                          |                  | [À collecter]                          |
| mode_paiement     | VARCHAR                                |                  | [NOUVEAU] (CB, virement, chèque, etc.) |
| statut_paiement   | ENUM('En attente','Partiel','Complet') | INDEX            | [NOUVEAU]                              |
| reference_externe | VARCHAR                                |                  | [NOUVEAU] (id transaction externe)     |

**Justification :**

* Complète les factures pour la gestion comptable.
* Prépare les analyses IA sur comportement de paiement (retard, mode, fréquence).

---
