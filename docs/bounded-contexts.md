# Bounded Contexts — Partie 2  
**Projet ESGI M2 — DDD : Conception du Domaine Métier**  
*Auteurs : Romain SCIANDRA & Omar MOSTAFA*  

---

## Vue d’ensemble du domaine  

```

[Clients B2B - Professionnels]
│
├──> [Contexte Devis]
│         └──> [Contexte Prestations]
│
├──> [Contexte Facturation & Paiement]
│
├──> [Contexte Référentiel]
│
└──> [Contexte Utilisateurs & Accès]

```

Les **clients professionnels (B2B)** utilisent la plateforme pour créer, envoyer et suivre des devis, tandis que leurs propres **clients finaux (B2C)** reçoivent, acceptent ou refusent ces devis et bénéficient des prestations réalisées.

---

## Contexte **Devis**

### Rôle et description  
Gère **l’ensemble du cycle de vie des devis** établis par les **professionnels (B2B)** à destination de leurs clients finaux.  
C’est le **point d’entrée** du flux commercial : tout commence par la demande ou la création d’un devis.

### Représentation visuelle  

```

# Contexte Devis :
-> Émission du devis
    |
    └> Informations client (B2C)
    |
    └> Informations prestation (B2B)
        |
        └> Parametrage professionel
-> Modification de devis
    |
    └> Informations refus client
    |
    └> Informations reduction prestation
        |
        └> Parametrage professionel  
                
```

### Responsabilités principales  
- Créer, modifier et dupliquer un devis.  
- Gérer les statuts (`DevisCréé`, `DevisEnvoyé`, `DevisAccepté`, `DevisRefusé`).  
- Conserver les informations sur les clients finaux et les professionnels.  
- Déclencher les prestations associées à un devis accepté.  
- Calculer les montants HT, TVA, TTC.  

### Événements métier  
- `DevisCréé`  
- `DevisEnvoyé`  
- `DevisAccepté`  
- `DevisRefusé`

### Tables associées  
- `acces_pro_devis_with_prestation`  
- `acces_pro_devis_sub_category`

### Classification  
**Core Domain**

---

## Contexte **Prestations**

### Rôle et description  
Ce contexte décrit la **réalisation opérationnelle** des prestations issues d’un devis accepté.  
Il connecte la logique métier (types de service, calendrier, exécution) avec la facturation.

### Représentation visuelle  

```

# Contexte Prestations :
-> Reception d'une demande de prestation
-> Planification prestation
        |
        └> Informations client (nom client, adresse de lieu de prestation, disponibilité client)
        |
        └> Informations devis
        |
        └> Informations professionnel (disponibilité employé)
-> Réalisation prestation
        |
        └> Informations client (nom client, adresse de lieu de prestation)
        |
        └> Informations devis

```

### Responsabilités principales  
- Planifier les prestations à partir des devis acceptés.  
- Suivre leur état : *planifiée*, *en cours*, *réalisée*.  
- Gérer les types de prestations, les déchets ou matériaux concernés.  
- Émettre les événements :  
  - `PrestationDemandée`, `PrestationRéalisée`.
- Réaliser les prestations qui ont été planifiées

### Tables associées  
- `acces_pro_devis_with_prestation`  
- `acces_pro_devis_sub_category`
- `acces_pro_client` 

### Classification  
**Core Domain**

---

## Contexte **Clients**

### Rôle et description  
Ce contexte gère les clients qui ont fait (minimum) une demande de prestation 

### Représentation visuelle  

```

# Contexte Clients :
-> création d'un client
        |
        └> Informations prestation (première demande de prestation)
-> Modification d'un client
        |
        └> Informations client (nom client, adresse de lieu de prestation, disponibilité client)
-> Supprission d'un client
        |
        └> Informations prestation (date de dernière demande de prestation)

```

### Responsabilités principales  
- Centraliser les données clients (identité, coordonnées, préférences).  
- Fournir les informations nécessaires à la génération de devis et factures.  

### Tables associées  
- `acces_pro_client`

### Classification  
**Supporting Domain**

---

## Contexte **Facturation & Paiement**

### Rôle et description  
Ce contexte prend le relais une fois la prestation réalisée pour **générer la facture**, suivre les paiements et gérer les échéances.

### Représentation visuelle  

```

# Contexte Facturation :

-> Émission facture
        |
        └> Informations sur l'etat de la prestation (*réalisée*)
        |
        └> Informations client (nom client, moyen de paiement)
        |
        └> Informations devis (montant prestation)
```

### Responsabilités principales  
- Générer la facture à partir du devis accepté.  
- Calculer TVA, montants HT/TTC, remises.  
- Suivre les paiements (`date_versement`, `type_paiement`, `num_paiement`).  
- Gérer les statuts : *émise*, *payée*, *en retard*.  

### Événements métier  
- `FactureÉmise`  
- `PaiementReçu`  

### Tables associées  
- `acces_pro_echeance_devis_2022`
- `acces_pro_devis_with_prestation`

### Classification  
**Supporting Domain**

---

## 🧩 Synthèse des Contextes

| Contexte | Type | But principal | Interagit avec |
|:--|:--|:--|:--|
| Devis | Core | Gérer la création et le cycle de vie des devis. | Clients, Prestations, Facturation |
| Prestations | Core | Planifier et exécuter les prestations issues des devis. | Devis, Facturation |
| Clients | Supporting | Gérer les professionnels (B2B) et les clients finaux (B2C). | Devis, Facturation |
| Facturation | Supporting | Générer et suivre les factures et paiements. | Devis, Prestations |

---
