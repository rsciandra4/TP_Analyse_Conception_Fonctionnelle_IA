# Invariants — Partie 3  
*Auteurs : Romain SCIANDRA & Omar MOSTAFA*  

---

## 📄 1. Agrégat `Devis`

### Racine d’agrégat : `Devis`

| **Invariant** | **Contexte** | **Décision métier** | **Où est-il vérifié ?** |
|:--|:--|:--|:--|
| 1. La somme des prestations doit être égale au `total_ht` du devis | Lors du calcul du montant total | Recalculer ou refuser la validation | création devis |
| 2. Un devis non accepté ne peut pas être facturé | Avant émission de facture | Refuser la génération de facture | création facture |
| 3. Un devis expiré (`dateExpiration < aujourd’hui`) ne peut pas être accepté | Lors de la validation du devis | Refuser l’action `accepter()` | date d'expiration devis |

---

## 2. Agrégat `Prestation`

### Racine d’agrégat : `Prestation`

| **Invariant** | **Contexte** | **Décision métier** | **Où est-il vérifié ?** |
|:--|:--|:--|:--|
| 1. Une prestation ne peut être planifiée que si le devis est accepté | Lors de la planification | Refuser l’action `planifier()` | Avant la prestation |
| 2. La `date_prestation` doit être postérieure à la `date_signature` du devis | Lors de la planification | Rejeter si la date est antérieure | Avant la prestation |
| 3. Une prestation terminée (`statut = réalisée`) ne peut plus être modifiée | Lors d’une mise à jour | Bloquer la modification | Avant facturation |

---

## 🧾 3. Agrégat `Facture`

### Racine d’agrégat : `Facture`

| **Invariant** | **Contexte** | **Décision métier** | **Où est-il vérifié ?** |
|:--|:--|:--|:--|
| 1. Un numéro de facture est forcément unique | Lors de la facturation | Ne pas envoyer la facture | création de la facture  |
| 2. Une facture `PAYÉE` ne peut plus être modifiée | Lors d’une tentative de modification | Bloquer l’édition, faire un avoir si necessaire | apres paiement |
| 3. Montant de facture >= 0 | Lors de la facturation | Ne pas créer la facture | création de facture |
---
