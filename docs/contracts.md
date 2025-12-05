# Contrats d’échange de données — Partie 5
*Auteurs : Romain SCIANDRA & Omar MOSTAFA*

---

## 1. Contrat d’événement — `FactureÉmise`

### 1.1. Description métier

- **Événement :** `FactureÉmise`
- **Quand ?**  
  Lorsqu’une facture est créée dans le contexte Facturation, à partir d’un devis accepté.
- **Produit par :**  
  Contexte **Facturation** (table `factures`).
- **Consommé par :**  
  - Contexte **Paiements** (initialisation du cycle de paiement),  
  - Contexte **Reporting / IA** (features pour prédire le délai de paiement, encours, risque, etc.).

### 1.2. JSON Schema de l’événement `FactureÉmise`

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "FactureÉmise",
  "type": "object",
  "required": [
    "event_name",
    "event_timestamp",
    "facture_id",
    "devis_id",
    "client_id",
    "date_facture",
    "montant_ht",
    "tva",
    "montant_ttc",
    "statut_facture"
  ],
  "properties": {
    "event_name": {
      "type": "string",
      "enum": ["FactureÉmise"],
      "description": "Nom de l'événement métier."
    },
    "event_timestamp": {
      "type": "string",
      "format": "date-time",
      "description": "Horodatage de l'émission de l'événement."
    },

    "facture_id": {
      "type": "integer",
      "description": "Identifiant technique de la facture (factures.id)."
    },
    "devis_id": {
      "type": "integer",
      "description": "Identifiant du devis associé (factures.devis_id → devis.id)."
    },
    "client_id": {
      "type": "integer",
      "description": "Identifiant du client (devis.client_id → clients.id)."
    },

    "date_facture": {
      "type": "string",
      "format": "date",
      "description": "Date de la facture (factures.date_facture)."
    },
    "montant_ht": {
      "type": "number",
      "description": "Montant hors taxes de la facture (factures.montant_ht)."
    },
    "tva": {
      "type": "number",
      "description": "Taux de TVA appliqué à la facture (factures.tva)."
    },
    "montant_ttc": {
      "type": "number",
      "description": "Montant toutes taxes comprises de la facture (factures.montant_ttc)."
    },
    "statut_facture": {
      "type": "string",
      "enum": ["Émise", "Payée", "En retard", "Annulée"],
      "description": "Statut de la facture (factures.statut_facture)."
    },

    "type_client": {
      "type": "string",
      "enum": ["B2B", "B2C"],
      "description": "Type de client (clients.type_client)."
    },
    "type_prestation": {
      "type": "string",
      "description": "Type de prestation principale (prestations.type_prestation, via devis.prestation_id)."
    },
    "sous_type_prestation": {
      "type": "string",
      "description": "Sous-type de prestation (prestations.sous_type_prestation)."
    },

    "correlation_id": {
      "type": "string",
      "description": "Identifiant de corrélation optionnel pour chaîner événements, devis, facture, paiements."
    }
  }
}
```

---

## 2. Contrat d’export tabulaire — Dataset IA « Délai de paiement »

### 2.1. Objectif de l’export

* **But :** construire une table d’apprentissage pour **prédire le délai de paiement d’une facture**.
* **Grain de la table :**
  -> **1 ligne = 1 facture** pour laquelle on dispose d’au moins un paiement enregistré.
* **Variable cible (label) :**
  `delai_paiement_jours` = nombre de jours entre `date_facture` (table `factures`) et `date_paiement` (table `paiements`).

Tables utilisées (schéma dédié) :

* `factures` (id, devis_id, date_facture, montant_ht, tva, montant_ttc, statut_facture, …)
* `paiements` (facture_id, date_paiement, montant_paye, statut_paiement, …)
* `devis` (client_id, prestation_id, date_creation, montant_ht, statut, …)
* `clients` (type_client, created_at, …)
* `prestations` (type_prestation, sous_type_prestation, prix_unitaire_ht, taux_tva, …)

---

### 2.2. Dictionnaire de données de l’export

#### 2.2.1. Identifiants & temporalité

| Colonne              | Type | Rôle              | Origine                                        | Description                               |
| -------------------- | ---- | ----------------- | ---------------------------------------------- | ----------------------------------------- |
| facture_id           | INT  | ID                | `factures.id`                                  | Identifiant unique de la facture.         |
| devis_id             | INT  | ID                | `factures.devis_id`                            | Référence au devis source.                |
| client_id            | INT  | ID                | `devis.client_id`                              | Client associé à la facture.              |
| date_facture         | DATE | Feature           | `factures.date_facture`                        | Date d’émission de la facture.            |
| date_paiement        | DATE | Target auxiliaire | `paiements.date_paiement` (paiement complet)   | Date effective de paiement.               |
| delai_paiement_jours | INT  | **Target**        | calcul dérivé (`date_paiement - date_facture`) | Délai en jours entre facture et paiement. |

> Remarque : on ne garde que les factures où `statut_paiement = 'Complet'` pour que `delai_paiement_jours` soit bien défini.

---

#### 2.2.2. Features client & contrat

| Colonne                 | Type   | Rôle            | Origine                             | Description                                   |
| ----------------------- | ------ | --------------- | ----------------------------------- | --------------------------------------------- |
| type_client             | STRING | Feature         | `clients.type_client`               | Type de client : B2B ou B2C.                  |
| anciennete_client_jours | INT    | Feature dérivée | `date_facture - clients.created_at` | Ancienneté du client au moment de la facture. |

---

#### 2.2.3. Features devis & prestation

| Colonne                     | Type    | Rôle    | Origine                            | Description                                 |
| --------------------------- | ------- | ------- | ---------------------------------- | ------------------------------------------- |
| statut_devis                | STRING  | Feature | `devis.status`                     | Statut final du devis (Accepté, Refusé, …). |
| montant_devis_ht            | DECIMAL | Feature | `devis.montant_ht`                 | Montant HT du devis.                        |
| type_prestation             | STRING  | Feature | `prestations.type_prestation`      | Type de prestation (ex : Nettoyage).        |
| sous_type_prestation        | STRING  | Feature | `prestations.sous_type_prestation` | Sous-type de prestation (ex : Vitres).      |
| prix_unitaire_prestation_ht | DECIMAL | Feature | `prestations.prix_unitaire_ht`     | Prix unitaire HT de la prestation.          |

---

#### 2.2.4. Features facture & paiement

| Colonne             | Type    | Rôle    | Origine                     | Description                                |
| ------------------- | ------- | ------- | --------------------------- | ------------------------------------------ |
| montant_facture_ht  | DECIMAL | Feature | `factures.montant_ht`       | Montant HT de la facture.                  |
| montant_facture_ttc | DECIMAL | Feature | `factures.montant_ttc`      | Montant TTC de la facture.                 |
| tva                 | DECIMAL | Feature | `factures.tva`              | Taux de TVA appliqué.                      |
| statut_facture      | STRING  | Feature | `factures.statut_facture`   | Statut (Émise, Payée, En retard, Annulée). |
| montant_paye        | DECIMAL | Feature | `paiements.montant_paye`    | Montant total payé sur la facture.         |
| statut_paiement     | STRING  | Feature | `paiements.statut_paiement` | En attente / Partiel / Complet.            |

---

### 2.3. Exemple de « schema » logique de la table d’export

*(forme lisible pour échange Data / IA)*

```json
{
  "table": "dataset_delai_paiement",
  "grain": "1 row = 1 facture avec paiement complet",
  "columns": [
    { "name": "facture_id",           "type": "integer", "role": "id" },
    { "name": "devis_id",             "type": "integer", "role": "id" },
    { "name": "client_id",            "type": "integer", "role": "id" },

    { "name": "date_facture",         "type": "date",    "role": "feature" },
    { "name": "date_paiement",        "type": "date",    "role": "auxiliary" },
    { "name": "delai_paiement_jours", "type": "integer", "role": "target" },

    { "name": "type_client",          "type": "string",  "role": "feature" },
    { "name": "anciennete_client_jours", "type": "integer", "role": "feature" },

    { "name": "statut_devis",         "type": "string",  "role": "feature" },
    { "name": "montant_devis_ht",     "type": "number",  "role": "feature" },

    { "name": "type_prestation",      "type": "string",  "role": "feature" },
    { "name": "sous_type_prestation", "type": "string",  "role": "feature" },
    { "name": "prix_unitaire_prestation_ht", "type": "number", "role": "feature" },

    { "name": "montant_facture_ht",   "type": "number",  "role": "feature" },
    { "name": "montant_facture_ttc",  "type": "number",  "role": "feature" },
    { "name": "tva",                  "type": "number",  "role": "feature" },
    { "name": "statut_facture",       "type": "string",  "role": "feature" },

    { "name": "montant_paye",         "type": "number",  "role": "feature" },
    { "name": "statut_paiement",      "type": "string",  "role": "feature" }
  ]
}
```

---

## 3. Résumé

* L’événement **`FactureÉmise`** est contracté via un **JSON Schema strict**, pour l’intégration entre Facturation, Paiement et IA.
* L’**export tabulaire** `dataset_delai_paiement` définit clairement :

  * le **grain** (1 ligne = 1 facture),
  * la **cible** ML (`delai_paiement_jours`),
  * et les **colonnes** (id, dates, montants, type_client, type/sous-type de prestation, statut…).

Ces contrats pourront être directement repris comme **spécification de pipeline** (ETL/ELT) entre la base dédiée et les notebooks / jobs d’entraînement de modèles.

---
