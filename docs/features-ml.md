# Features ML — Partie 5  
*Auteurs : Romain SCIANDRA & Omar MOSTAFA*

---

## Consigne  
Identifier les **10 meilleures features calculables aujourd’hui** à partir du schéma dédié (`clients`, `prestations`, `devis`, `factures`, `paiements`) ainsi que **5 features à collecter** pour améliorer les futurs modèles IA (notamment la prédiction du délai de paiement).

---

# 1. Top 10 features calculables aujourd’hui  

Pour chaque feature :  
- **Nom**  
- **Définition**  
- **Formule / source exacte**  
- **Intérêt pour l’IA**  

---

### 1. Montant de la facture (TTC)
- **Définition :** montant total facturé au client.  
- **Source :** `factures.montant_ttc`  
- **Intérêt IA :** les grosses factures sont souvent payées plus tard → corrélation forte avec le délai.

---

### 2. Montant HT + TVA
- **Définition :** montant hors taxes et taux de TVA appliqué.  
- **Source :** `factures.montant_ht`, `factures.tva`  
- **Intérêt IA :** certaines prestations fortement taxées indiquent segments spécifiques de clients → impacts possibles sur les comportements de paiement.

---

### 3. Type de prestation
- **Définition :** catégorie principale de la prestation demandée.  
- **Source :** `prestations.type_prestation` (via `devis.prestation_id`)  
- **Intérêt IA :** certains types (nettoyage, débarras, intervention rapide…) ont des comportements de paiement historiques différents.

---

### 4. Sous-type de prestation
- **Définition :** sous-catégorie plus fine (ex : Vitres, Jardinage, Déchets verts).  
- **Source :** `prestations.sous_type_prestation`  
- **Intérêt IA :** niveau plus précis pour discriminer les délais de paiement selon la nature opérationnelle de la prestation.

---

### 5. Prix unitaire HT de la prestation
- **Définition :** prix catalogue de la prestation associée au devis.  
- **Source :** `prestations.prix_unitaire_ht`  
- **Intérêt IA :** peut capturer la complexité ou spécialisation du service → influence sur les conditions et délais de paiement.

---

### 6. Type de client (B2B ou B2C)
- **Définition :** classification du client.  
- **Source :** `clients.type_client`  
- **Intérêt IA :**  
  - Les **clients B2B** paient souvent à échéance (30–60 jours).  
  - Les **clients B2C** paient souvent immédiatement → feature critique.

---

### 7. Ancienneté du client (en jours)
- **Définition :** nombre de jours entre la création du client et la date de facture.  
- **Formule :** `DATEDIFF(factures.date_facture, clients.created_at)`  
- **Source :** `clients.created_at`, `factures.date_facture`  
- **Intérêt IA :** les clients fidèles paient en général plus rapidement.

---

### 8. Jours entre création du devis et génération de la facture
- **Définition :** rapidité du cycle commercial.  
- **Formule :** `DATEDIFF(factures.date_facture, devis.date_creation)`  
- **Source :** `devis.date_creation`, `factures.date_facture`  
- **Intérêt IA :** un processus long signale des frictions → risque de paiement plus lent.

---

### 9. Statut final de la facture
- **Définition :** état administratif : Émise, Payée, En retard, Annulée.  
- **Source :** `factures.statut_facture`  
- **Intérêt IA :** feature permettant d’exclure les factures hors périmètre ou d'analyser les patterns de retard.

---

### 10. Nombre de paiements associés à la facture
- **Définition :** nombre d’enregistrements dans `paiements` pour une facture donnée.  
- **Formule :** `COUNT(paiements WHERE paiements.facture_id = factures.id)`  
- **Source :** table `paiements`  
- **Intérêt IA :**  
  - Une facture réglée en plusieurs fois indique souvent une tension financière.  
  - Forte corrélation avec des délais plus longs.

---

# 2. Top 5 features à collecter pour améliorer le modèle IA  

---

### 1. Motif de retard ou d’impayé
- **Pourquoi utile :** permet de distinguer les retards "techniques" (administratifs) des retards "risque client".  
- **Collecte :** entrée manuelle lors d’un retard, ou sélection dans une liste standardisée.

---

### 2. Mode de paiement détaillé
- **Pourquoi utile :** les paiements par chèque ou virement ont des délais mécaniques plus longs.  
- **Collecte :** champ obligatoire dans `paiements` via l’interface de saisie.

---

### 3. Score de satisfaction / retour prestation
- **Pourquoi utile :** un client mécontent paye souvent plus tard.  
- **Collecte :** formulaire de satisfaction post-prestation.

---

### 4. Canal d’acquisition du client
- **Pourquoi utile :** les clients acquis via recommandation ou partenaire → comportements de paiement différents.  
- **Collecte :** tracking CRM (source : site web, appel, recommandation, etc.).

---

### 5. Délai réel de réalisation de la prestation
- **Pourquoi utile :** mesure la complexité réelle → impact possible sur les retards de paiement.  
- **Collecte :** temps horodaté début/fin dans l’application terrain.

---
