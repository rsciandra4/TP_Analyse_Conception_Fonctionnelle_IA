# Lecture du profil CSV  
**Fichier source :** `descriptif_donnee.csv`  
**Nombre total de colonnes :** 16  
**Nombre total d’enregistrements décrits :** 244  

---

## 1. Liste des tables `acces_pro_*`

| Table | Nombre estimé de lignes | Nombre de colonnes |
|:--|--:|--:|
| acces_pro_category | 6 | 14 |
| acces_pro_client | 901 | 36 |
| acces_pro_devis_sub_category | 2 312 | 16 |
| acces_pro_devis_with_prestation | 180 520 | 180+ |
| acces_pro_echeance_devis_2022 | 500 000 | 6 |
| acces_pro_sub_category | 113 | 8 |

---

## 2. Détail par table

### acces_pro_category
| Colonne | Type | Valeurs uniques | % null | Commentaire |
|:--|:--|--:|--:|:--|
| id | int64 | 6 | 0 | Clé primaire probable |
| ordre | int64 | 6 | 0 | Ordre d’affichage |
| is_view_by | bool | 2 | 0 | Flag d’affichage |
| name | object | 6 | 0 | Nom de la catégorie |
| tva | float64 | 6 | 0 | Taux de TVA |
| code_nature | object | 6 | 0 | Code interne |
| lettre | object | 6 | 0 | Code court |
| compte_comptable | object | 6 | 0 | Référence comptable |
| plafond | float64 | 6 | 0 | Plafond applicable |
| tax_credit_ceiling | float64 | 6 | 0 | Crédit d’impôt maximum |
| is_deleted | bool | 2 | 0 | Statut de suppression |
| internal_use_only | bool | 2 | 0 | Usage interne |

**Clé candidate :** `id`    

---

### acces_pro_client
- **36 colonnes** couvrant identité, adresse, contact et gestion d’état (inutile de lister les 36 colonnes).  
- **Clés candidates probables :** `id`, `reference`  
- **Champs clés secondaires :** `profil_urssaf_id`, `adresse_facturation_id`, `utilisateur_id`  
- **Indices de qualité :** très peu de nulls (principalement dans `fax`, `email_newsletter`), distributions homogènes.  

---

### acces_pro_devis_sub_category
| Colonne | Type | Observations |
|:--|:--|:--|
| id | int64 | clé candidate |
| devis_id | int64 | référence vers table devis |
| sub_category_id | int64 | lien vers `acces_pro_sub_category` |
| waste_* | object/float | détails de gestion des déchets |
| total_ht / total_ttc / tva | float64 | montants financiers |

**Relations :**  
- `devis_id` → `acces_pro_devis_with_prestation.id`  
- `sub_category_id` → `acces_pro_sub_category.id`

---

### acces_pro_devis_with_prestation
- **Table principale de facturation / devis** (≈ 180 520 lignes).  
- Contient de nombreux champs financiers et métadonnées client/adherent.  
- **Clé candidate :** `id.1`  
- **Relations probables :**
  - `client_id` → `acces_pro_client.id`
  - `categorie_id`, `category_id` → `acces_pro_category.id`
  - `sub_categorie_id`, `sub_category_id` → `acces_pro_sub_category.id`
  - `type_paiement_id` → table externe non décrite (paiement)
  - `devis_id` → auto-référence ou historique de devis  

- **Qualité :** données volumineuses avec redondance (plusieurs versions de `id`, `total_ht`, `tva`, etc.), colonnes obsolètes (`.old`, `.1`, `.2`).  
---

### acces_pro_echeance_devis_2022
| Colonne | Type | Commentaire |
|:--|:--|:--|
| id | int64 | clé primaire probable |
| date | datetime | échéance |
| montant | float64 | montant principal |
| devis_id | int64 | lien vers `acces_pro_devis_with_prestation` |
| montant_old | float64 | version historique |
| duree | float64 | durée associée |

🔗 **Relation principale :** `devis_id` → `acces_pro_devis_with_prestation.id`  

---

### acces_pro_sub_category
| Colonne | Type | Commentaire |
|:--|:--|:--|
| id | int64 | clé primaire |
| name | object | nom de la sous-catégorie |
| category_id | int64 | lien vers `acces_pro_category.id` |
| is_default | bool | statut par défaut |

**Relation :** `category_id` → `acces_pro_category.id`  

---

## 3. Synthèse globale des types

| Type | Colonnes |
|:--|:--|
| int64 | identifiants, codes, indicateurs numériques |
| float64 | montants, taux, statistiques |
| object | chaînes de texte |
| bool | états logiques / flags |

---

## 4. Déduction des clés et relations

### Clés candidates identifiées
| Table | Clé candidate |
|:--|:--|
| acces_pro_category | id |
| acces_pro_client | id, reference |
| acces_pro_devis_sub_category | id |
| acces_pro_devis_with_prestation | id.1 |
| acces_pro_echeance_devis_2022 | id |
| acces_pro_sub_category | id |

### Relations probables
| Table source | Colonne | Table cible |
|:--|:--|:--|
| acces_pro_devis_sub_category | sub_category_id | acces_pro_sub_category |
| acces_pro_devis_with_prestation | client_id | acces_pro_client |
| acces_pro_devis_with_prestation | categorie_id | acces_pro_category |
| acces_pro_devis_with_prestation | sub_categorie_id | acces_pro_sub_category |
| acces_pro_echeance_devis_2022 | devis_id | acces_pro_devis_with_prestation |
| acces_pro_sub_category | category_id | acces_pro_category |

---

## 5. Observations sur la qualité des données

- **Taux de nullité global moyen :** < 5 %  
- **Données cohérentes** entre les tables principales.  
- **Doublons potentiels** dans `acces_pro_devis_with_prestation` dus à plusieurs versions de champs (`*_old`, `*_1`, `*_2`).  
- **Structure relationnelle claire**, proche d’un modèle en étoile autour du devis (client, catégorie, sous-catégorie, échéances).  

---

## 6. Conclusion

Le jeu de données `acces_pro_*` décrit un **système complet de gestion de devis, clients et catégories**.  
Les relations sont cohérentes avec une architecture SQL bien définie.

---
