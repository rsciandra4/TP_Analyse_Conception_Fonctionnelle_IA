# Quality Rules — Partie 5  
*Auteurs : Romain SCIANDRA & Omar MOSTAFA*

---

## Consigne
Définir **3 règles de qualité des données** essentielles pour garantir :  
- la fiabilité des exports métier,  
- la cohérence des pipelines analytiques,  
- la performance des modèles IA (prédiction du délai de paiement),  
- l’observabilité data (contrôles journaliers).

---

# 1. Règle de qualité n°1 — **Unicité et validité des clients**

Garantir que chaque client existe une seule fois, avec des coordonnées valides.  
Critique pour le reporting, la facturation et les agrégats d’IA.

### Contrôles
- Doublons sur `(email_address)`  
- Doublons sur `(reference)`  
- Valeurs manquantes dans :
  - `email_address`
  - `firstname`, `lastname`
  - `type_client` (B2B/B2C)

### Métriques à suivre (journalier)
- `nb_clients_doublons_email`
- `nb_clients_reference_dup`
- `pct_clients_missing_contact`

### Actions en cas de dérive
- Signalement automatique aux équipes métier.  
- Proposition de fusion de fiches client (merge).  
- Blocage de la création de devis pour fiches invalides.

---

# 2. Règle de qualité n°2 — Cohérence devis → factures → paiements

Assurer que les documents commerciaux suivent un enchaînement cohérent, sans anomalies qui casseraient un pipeline ou un modèle IA.

### Contrôles
- `factures.devis_id` doit exister dans `devis.id`  
- `paiements.facture_id` doit exister dans `factures.id`  
- Contrôles de cohérence temporelle :
  - `date_facture >= devis.date_creation`
  - `date_paiement >= date_facture`

- Contrôle de cohérence financière :
  - `montant_paye <= montant_facture_ttc`
  - Si `statut_paiement = 'Complet'` → `montant_paye == montant_facture_ttc`

### Métriques à suivre
- `nb_factures_orphelines` (factures sans devis)  
- `nb_paiements_orphelins` (paiements sans facture)  
- `nb_dates_incoherentes_devis_facture`  
- `nb_dates_incoherentes_facture_paiement`  
- `nb_factures_montant_incoherent`

### Actions en cas de dérive
- Blocage de l’intégration des factures incohérentes.  
- Alerte Data Quality envoyée à la comptabilité / support.  
- Correction automatique dans un job ETL (mapping devis → facture).

---

# 3. Règle de qualité n°3 — Valeurs critiques manquantes dans factures & paiements

Empêcher la création de trous dans :
- les KPI finance,
- les prévisions IA,
- le rapprochement comptable.

### Colonnes considérées comme critiques
- Factures :
  - `factures.date_facture`
  - `factures.montant_ttc`
  - `factures.devis_id`
  - `factures.statut_facture`
- Paiements :
  - `paiements.date_paiement`
  - `paiements.montant_paye`
  - `paiements.statut_paiement`

### Métriques à suivre
- `pct_factures_missing_core_fields`
- `pct_paiements_missing_core_fields`

### Actions en cas de dérive
- Rejet automatique des lignes invalides dans les exports analytiques.  
- Alerte Slack/Email si le taux dépasse un seuil (ex : 2%).  
- Mise en file d’attente pour correction manuelle ou script de remplissage automatique (si valeurs inférables).

---
