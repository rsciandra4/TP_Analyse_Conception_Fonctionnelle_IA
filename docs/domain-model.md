# Domain Model — Partie 3  
**Projet ESGI M2 — DDD : Conception du Domaine Métier**  
*Auteurs : Romain SCIANDRA & Omar MOSTAFA*  

---

## 🎯 Objectif  
Décrire les **entités** et **objets valeur** du domaine métier à partir des tables `acces_pro_*` présentes dans le fichier `descriptif_donnee.csv`.  
Ce document correspond au livrable 1 de la Partie 3 : **Modélisation tactique** (entités / objets valeur).

---

## 🧱 1. Entités  

| **Nom de l’entité** | **Attributs clés** | **Identité** |
|:--|:--|:--|
| **Client** | `firstname`, `lastname`, `email_address`, `phone`, `address`, `postal_code`, `city`, `created_at`, `updated_at` | `id` **(table acces_pro_client)** |
| **Devis** | `reference`, `date`, `status`, `total_ht`, `total_ttc`, `tva`, `client_id`, `categorie_id`, `sub_categorie_id`, `created_at`, `updated_at` | `id`**(table acces_pro_devis_with_prestation)** |
| **Prestation** | `description`, `type_prestation`, `date_prestation`, `waste_type`, `waste_quantity`, `waste_unit`, `devis_id`, `client_id` | `id` **(table acces_pro_devis_with_prestation)** |
| **Facture** | `date`, `montant`, `devis_id`, `duree`, `num_paiement`, `date_versement`, `date_encaissement`, `statut` | `id` **(table acces_pro_client)** |

---

## 💎 2. Objets Valeur  

| **Nom de l’objet valeur** | **Attributs internes** | **Règles locales (invariants internes)** |
|:--|:--|:--|
| **Adresse** | `address`, `postal_code`, `city` | - `postal_code` : 5 chiffres <br> - `city` non vide |
| **Montant** | `total_ht`, `tva`, `total_ttc` | - `total_ht > 0` <br> - `total_ttc = total_ht × (1 + tva)` |
| **Quantité** | `waste_quantity`, `waste_unit` | - `waste_quantity ≥ 0` |
| **Période** | `date_devis`, `date_prestation`, `date_facture` | - `date_devis <= date_prestation <= date_facture` |

---