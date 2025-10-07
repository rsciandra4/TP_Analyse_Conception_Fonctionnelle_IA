# Données — Exploration (Partie 1)

## 1) Vue d’ensemble

Tables détectées : acces_pro_category, acces_pro_client, acces_pro_devis_sub_category, acces_pro_devis_with_prestation, acces_pro_echeance_devis_2022, acces_pro_sub_category.

Profil issu de `descriptif_donnee.csv` (dictionnaire statistique). Les pourcentages de valeurs nulles et le nombre de valeurs uniques proviennent du profil.

## 2) Détail par table

### acces_pro_category

| Colonne | Type | #Uniques | #Nulls | % Null | Résumé |
|---|---:|---:|---:|---:|---|
| id | int64 | 6 | 0 | 0.00 | Med=5 \| P5–P95=[1.25; 10.8] |
| code_nature | int64 | 6 | 0 | 0.00 | Med=90 \| P5–P95=[62.5; 182] |
| compte_comptable | int64 | 6 | 0 | 0.00 | Med=6.11e+06 \| P5–P95=[6.11e+06; 6.11e+06] |
| internal_use_only | bool | 1 | 0 | 0.00 |  |
| is_deleted | bool | 1 | 0 | 0.00 |  |
| is_reduced_tax | bool | 2 | 0 | 0.00 |  |
| is_view_by | object | 0 | 6 | 100.00 |  |
| lettre | object | 6 | 0 | 0.00 |  |
| name | object | 6 | 0 | 0.00 |  |
| ordre | int64 | 5 | 0 | 0.00 | Med=3.5 \| P5–P95=[1.25; 5] |
| plafond | int64 | 3 | 0 | 0.00 | Med=1e+03 \| P5–P95=[500; 1.75e+03] |
| tax_credit_ceiling | int64 | 4 | 0 | 0.00 | Med=4.25e+03 \| P5–P95=[562; 6e+03] |
| tva | float64 | 2 | 0 | 0.00 | Med=0.15 \| P5–P95=[0.1; 0.2] |
| tva_ile_reunion | float64 | 2 | 0 | 0.00 | Med=0.053 \| P5–P95=[0.021; 0.085] |
### acces_pro_client

| Colonne | Type | #Uniques | #Nulls | % Null | Résumé |
|---|---:|---:|---:|---:|---|
| id | int64 | 901 | 0 | 0.00 | Med=1.16e+06 \| P5–P95=[3.78e+05; 1.16e+06] |
| adresse_facturation_id | float64 | 193 | 708 | 78.58 | Med=6.33e+05 \| P5–P95=[1.85e+05; 9.18e+05] |
| agent_id | object | 0 | 901 | 100.00 |  |
| authenticator_id | float64 | 427 | 474 | 52.61 | Med=2.92e+05 \| P5–P95=[5.72e+04; 3.56e+05] |
| emailing_contact_id | float64 | 109 | 792 | 87.90 | Med=6.85e+05 \| P5–P95=[2.61e+05; 9.87e+05] |
| fusion_client_id | object | 0 | 901 | 100.00 |  |
| profil_urssaf_id | float64 | 260 | 641 | 71.14 | Med=1.36e+05 \| P5–P95=[2.21e+04; 1.86e+05] |
| user_id | int64 | 571 | 0 | 0.00 | Med=6.92e+03 \| P5–P95=[1.28e+03; 1.35e+04] |
| utilisateur_id | float64 | 8 | 891 | 98.89 | Med=104 \| P5–P95=[18.1; 408] |
| address | object | 886 | 0 | 0.00 |  |
| address_chantier | object | 98 | 791 | 87.79 |  |
| adresse_chantier_different | bool | 2 | 0 | 0.00 |  |
| blocked_by_client | float64 | 1 | 804 | 89.23 | Med=1 \| P5–P95=[1; 1] |
| cellular | float64 | 786 | 97 | 10.77 | Med=6.61e+08 \| P5–P95=[6.08e+08; 7.68e+08] |
| city | object | 794 | 1 | 0.11 |  |
| city_chantier | object | 97 | 790 | 87.68 |  |
| civility | object | 7 | 0 | 0.00 |  |
| created_at | object | 898 | 0 | 0.00 |  |
| date_fusion | object | 0 | 901 | 100.00 |  |
| deleted_at | object | 0 | 901 | 100.00 |  |
| deleted_by | object | 0 | 901 | 100.00 |  |
| email_address | object | 805 | 83 | 9.21 |  |
| email_newsletter | object | 64 | 837 | 92.90 |  |
| fax | object | 0 | 901 | 100.00 |  |
| firstname | object | 429 | 0 | 0.00 |  |
| has_account | float64 | 2 | 496 | 55.05 | Med=1 \| P5–P95=[1; 1] |
| invited_at | object | 673 | 227 | 25.19 |  |
| is_delete | bool | 1 | 0 | 0.00 |  |
| lastname | object | 844 | 0 | 0.00 |  |
| newsletter | float64 | 2 | 797 | 88.46 | Med=1 \| P5–P95=[0; 1] |
| phone | float64 | 125 | 776 | 86.13 | Med=3.84e+08 \| P5–P95=[2.32e+08; 9.65e+08] |
| postal_code | float64 | 714 | 1 | 0.11 | Med=5.92e+04 \| P5–P95=[1.23e+04; 9.16e+04] |
| postal_code_chantier | float64 | 91 | 790 | 87.68 | Med=6.26e+04 \| P5–P95=[1.21e+04; 9.51e+04] |
| reference | object | 901 | 0 | 0.00 |  |
| show_mail | bool | 1 | 0 | 0.00 |  |
| site | object | 0 | 901 | 100.00 |  |
| updated_at | object | 888 | 0 | 0.00 |  |
| valid_doublon | bool | 1 | 0 | 0.00 |  |
### acces_pro_devis_sub_category

| Colonne | Type | #Uniques | #Nulls | % Null | Résumé |
|---|---:|---:|---:|---:|---|
| id | int64 | 2312 | 0 | 0.00 | Med=2.92e+06 \| P5–P95=[2.92e+06; 2.92e+06] |
| collection_point_id | object | 0 | 2312 | 100.00 |  |
| devis_id | int64 | 1000 | 0 | 0.00 | Med=1.11e+06 \| P5–P95=[1.11e+06; 1.11e+06] |
| sub_category_id | int64 | 64 | 0 | 0.00 | Med=92 \| P5–P95=[5; 671] |
| total_ht | float64 | 988 | 0 | 0.00 | Med=134 \| P5–P95=[0; 1.79e+03] |
| total_ttc | float64 | 1024 | 0 | 0.00 | Med=161 \| P5–P95=[0; 2.15e+03] |
| tva | float64 | 3 | 0 | 0.00 | Med=0.2 \| P5–P95=[0; 0.2] |
| waste_collection_point_address | object | 223 | 1952 | 84.43 |  |
| waste_collection_point_city | object | 210 | 1956 | 84.60 |  |
| waste_collection_point_name | object | 213 | 1952 | 84.43 |  |
| waste_collection_point_postal_code | float64 | 199 | 1956 | 84.60 | Med=5.66e+04 \| P5–P95=[1.21e+04; 8.95e+04] |
| waste_collection_point_type | float64 | 6 | 1947 | 84.21 | Med=3 \| P5–P95=[1; 4] |
| waste_management | float64 | 3 | 1437 | 62.15 | Med=3 \| P5–P95=[1; 3] |
| waste_management_type | float64 | 6 | 1947 | 84.21 | Med=1 \| P5–P95=[1; 4] |
| waste_quantity | float64 | 61 | 1926 | 83.30 | Med=6 \| P5–P95=[1; 1e+03] |
| waste_type | float64 | 3 | 1926 | 83.30 | Med=1 \| P5–P95=[1; 1] |
| waste_unit | float64 | 5 | 1926 | 83.30 | Med=4 \| P5–P95=[1; 4] |
### acces_pro_devis_with_prestation

| Colonne | Type | #Uniques | #Nulls | % Null | Résumé |
|---|---:|---:|---:|---:|---|
| id | int64 | 172722 | 0 | 0.00 | Med=5.09e+05 \| P5–P95=[4.67e+04; 1.06e+06] |
| adherent_quote_request_id | object | 746 | 179745 | 99.57 |  |
| categorie_id | float64 | 2 | 38 | 0.02 | Med=2 \| P5–P95=[2; 2] |
| category_id | int64 | 1 | 0 | 0.00 | Med=2 \| P5–P95=[2; 2] |
| client_id | float64 | 97146 | 3 | 0.00 | Med=5.13e+05 \| P5–P95=[6.31e+04; 1.09e+06] |
| collection_point_id | object | 0 | 180520 | 100.00 |  |
| devis_id | int64 | 172722 | 0 | 0.00 | Med=5.09e+05 \| P5–P95=[4.67e+04; 1.06e+06] |
| monitoring_id | object | 234 | 180248 | 99.85 |  |
| old_client_id | object | 121 | 180333 | 99.90 |  |
| papier_administratif_cachet_id | object | 1068 | 155301 | 86.03 |  |
| papier_administratif_logo_id | float64 | 3398 | 104101 | 57.67 | Med=1.74e+04 \| P5–P95=[3.35e+03; 4.53e+04] |
| parent_id | float64 | 30907 | 139571 | 77.32 | Med=5.27e+05 \| P5–P95=[0; 9.86e+05] |
| reject_mofit_type_id | object | 5 | 178744 | 99.02 |  |
| sub_categorie_id | float64 | 130 | 1793 | 0.99 | Med=31 \| P5–P95=[4; 153] |
| sub_category_id | int64 | 3 | 0 | 0.00 | Med=4 \| P5–P95=[4; 30] |
| type_paiement_id | bool | 1 | 0 | 0.00 |  |
| user_id | float64 | 6260 | 1 | 0.00 | Med=3.9e+03 \| P5–P95=[747; 9.43e+03] |
| adherent_address | object | 6372 | 30849 | 17.09 |  |
| adherent_cachet | object | 1198 | 141999 | 78.66 |  |
| adherent_cellular | float64 | 4930 | 47756 | 26.45 | Med=6.66e+08 \| P5–P95=[6.09e+08; 7.76e+08] |
| adherent_city | object | 5356 | 12 | 0.01 |  |
| adherent_email_address | object | 5838 | 49080 | 27.19 |  |
| adherent_entreprise_name | object | 6225 | 1 | 0.00 |  |
| adherent_logo | object | 4315 | 40620 | 22.50 |  |
| adherent_n_tva | object | 5621 | 8468 | 4.69 |  |
| adherent_phone | float64 | 2419 | 81421 | 45.10 | Med=4.76e+08 \| P5–P95=[2.31e+08; 7.61e+08] |
| adherent_postal_code | float64 | 3083 | 10 | 0.01 | Med=5.07e+04 \| P5–P95=[1.31e+04; 8.74e+04] |
| adherent_siret_number | float64 | 6254 | 118 | 0.07 | Med=8.19e+13 \| P5–P95=[3.99e+13; 9.4e+13] |
| bak_com | object | 1 | 180519 | 100.00 |  |
| client_additional_address | object | 3800 | 174391 | 96.60 |  |
| client_additional_address_chaniter | object | 3555 | 174682 | 96.77 |  |
| client_address | object | 98350 | 255 | 0.14 |  |
| client_address_chantier | object | 68310 | 61424 | 34.03 |  |
| client_city | object | 33374 | 106 | 0.06 |  |
| client_city_chantier | object | 26320 | 61112 | 33.85 |  |
| client_civility | object | 16 | 707 | 0.39 |  |
| client_firstname | object | 12041 | 58 | 0.03 |  |
| client_lastname | object | 52079 | 23 | 0.01 |  |
| client_postal_code | float64 | 5584 | 345 | 0.19 | Med=5.62e+04 \| P5–P95=[1.41e+04; 9.04e+04] |
| client_postal_code_chantier | float64 | 5278 | 61679 | 34.17 | Med=5.05e+04 \| P5–P95=[1.31e+04; 8.82e+04] |
| comission | float64 | 23 | 0 | 0.00 | Med=1.1 \| P5–P95=[1.07; 1.12] |
| comment | object | 90049 | 56199 | 31.13 |  |
| commentaire | object | 17146 | 146388 | 81.09 |  |
| compte_encaissement | object | 0 | 180520 | 100.00 |  |
| compte_versement | object | 0 | 180520 | 100.00 |  |
| created_at | object | 4238 | 4 | 0.00 |  |
| created_by | object | 5296 | 112989 | 62.59 |  |
| date | object | 4232 | 31 | 0.02 |  |
| date_encaissement | object | 0 | 180520 | 100.00 |  |
| date_envoi_courrier | object | 0 | 180520 | 100.00 |  |
| date_heure_creation | object | 171526 | 611 | 0.34 |  |
| date_impression | object | 1056 | 174062 | 96.42 |  |
| date_intervention | object | 0 | 180520 | 100.00 |  |
| date_signature | object | 33156 | 146180 | 80.98 |  |
| date_versement | object | 0 | 180520 | 100.00 |  |
| debut_prestation | object | 2806 | 112798 | 62.49 |  |
| delais_prestation | int64 | 590 | 0 | 0.00 | Med=5.76e+04 \| P5–P95=[3.6e+03; 2.52e+05] |
| deleted_at | object | 8619 | 165617 | 91.74 |  |
| deleted_at.1 | object | 0 | 180520 | 100.00 |  |
| deleted_by | object | 754 | 173114 | 95.90 |  |
| deleted_by.1 | object | 0 | 180520 | 100.00 |  |
| designation_article | object | 0 | 180520 | 100.00 |  |
| devis_type | int64 | 4 | 0 | 0.00 | Med=3 \| P5–P95=[3; 3] |
| discount | bool | 1 | 0 | 0.00 |  |
| echeance_frequence | float64 | 2 | 136691 | 75.72 | Med=1 \| P5–P95=[1; 3] |
| etat_signature | float64 | 3 | 1 | 0.00 | Med=0 \| P5–P95=[0; 2] |
| fin_prestation | object | 2699 | 111778 | 61.92 |  |
| frequence_intevention | object | 0 | 180520 | 100.00 |  |
| frequence_intevention_par | object | 0 | 180520 | 100.00 |  |
| id.1 | int64 | 180520 | 0 | 0.00 | Med=1.16e+06 \| P5–P95=[7.9e+04; 2.78e+06] |
| id.2 | int64 | 3 | 0 | 0.00 | Med=4 \| P5–P95=[4; 30] |
| interventions | object | 7460 | 165769 | 91.83 |  |
| is_archived | bool | 2 | 0 | 0.00 |  |
| is_default | bool | 1 | 0 | 0.00 |  |
| is_delete | bool | 2 | 0 | 0.00 |  |
| is_delete.1 | bool | 1 | 0 | 0.00 |  |
| is_envoye_courrier | bool | 1 | 0 | 0.00 |  |
| is_imprime | bool | 2 | 0 | 0.00 |  |
| is_transform | bool | 2 | 0 | 0.00 |  |
| is_unep | bool | 2 | 0 | 0.00 |  |
| is_view_by | bool | 1 | 0 | 0.00 |  |
| is_visible | bool | 2 | 0 | 0.00 |  |
| masquer_duree | bool | 2 | 0 | 0.00 |  |
| name | object | 3 | 0 | 0.00 |  |
| nb_passage | object | 0 | 180520 | 100.00 |  |
| nbr_echeance | float64 | 26 | 747 | 0.41 | Med=1 \| P5–P95=[1; 12] |
| nbr_echeance_devis | float64 | 25 | 71175 | 39.43 | Med=0 \| P5–P95=[0; 12] |
| num_paiement | object | 0 | 180520 | 100.00 |  |
| num_paiement1 | object | 0 | 180520 | 100.00 |  |
| num_paiement2 | object | 0 | 180520 | 100.00 |  |
| num_versement | object | 0 | 180520 | 100.00 |  |
| number_workers | float64 | 15 | 70879 | 39.26 | Med=1 \| P5–P95=[1; 3] |
| numero_avoir | object | 7121 | 173023 | 95.85 |  |
| ordre | int64 | 2 | 0 | 0.00 | Med=2 \| P5–P95=[2; 1e+04] |
| origin_version | float64 | 3 | 66608 | 36.90 | Med=3 \| P5–P95=[3; 3] |
| prestation_type | float64 | 3 | 44 | 0.02 | Med=1 \| P5–P95=[1; 2] |
| prix_ttc | object | 0 | 180520 | 100.00 |  |
| prix_ttc_old | object | 0 | 180520 | 100.00 |  |
| pu_article | float64 | 3568 | 126983 | 70.34 | Med=0 \| P5–P95=[0; 520] |
| pu_article.1 | float64 | 21519 | 5 | 0.00 | Med=112 \| P5–P95=[33.3; 1.36e+03] |
| pu_article_ht | float64 | 25170 | 5862 | 3.25 | Med=112 \| P5–P95=[33.3; 1.36e+03] |
| pu_article_old | float64 | 3551 | 134454 | 74.48 | Med=0 \| P5–P95=[0; 620] |
| pu_ttc_article | float64 | 21908 | 0 | 0.00 | Med=134 \| P5–P95=[39.6; 1.62e+03] |
| qty_article | float64 | 201 | 115253 | 63.85 | Med=1 \| P5–P95=[0; 8] |
| qty_article.1 | float64 | 542 | 4 | 0.00 | Med=1 \| P5–P95=[1; 16] |
| qty_article_old | float64 | 201 | 124226 | 68.82 | Med=1 \| P5–P95=[0; 10] |
| quote_storage_config | object | 50855 | 127226 | 70.48 |  |
| reference | object | 172721 | 1 | 0.00 |  |
| reference_utilisateur | object | 43 | 179556 | 99.47 |  |
| refused_at | object | 1077 | 174635 | 96.74 |  |
| reject_object | object | 866 | 179470 | 99.42 |  |
| schedule | object | 1770 | 177149 | 98.13 |  |
| signature_fileid | object | 52962 | 125671 | 69.62 |  |
| signature_memberid | object | 52635 | 126005 | 69.80 |  |
| signature_procedureid | object | 52634 | 126005 | 69.80 |  |
| signature_type | object | 1 | 180201 | 99.82 |  |
| status | int64 | 6 | 0 | 0.00 | Med=2 \| P5–P95=[0; 4] |
| tax_reversed | float64 | 3 | 5889 | 3.26 | Med=0.2 \| P5–P95=[0; 0.2] |
| tax_reversed_old | object | 0 | 180520 | 100.00 |  |
| total | float64 | 46576 | 8 | 0.00 | Med=945 \| P5–P95=[90; 3.81e+03] |
| total_discount | float64 | 25112 | 5861 | 3.25 | Med=112 \| P5–P95=[33.3; 1.36e+03] |
| total_ht | float64 | 44985 | 0 | 0.00 | Med=850 \| P5–P95=[81.2; 3.34e+03] |
| total_ht.1 | float64 | 24562 | 5861 | 3.25 | Med=500 \| P5–P95=[50; 2.07e+03] |
| total_ht_old | float64 | 26027 | 118735 | 65.77 | Med=798 \| P5–P95=[75; 3.12e+03] |
| total_ht_old.1 | object | 0 | 180520 | 100.00 |  |
| total_ht_reversed | float64 | 59794 | 5856 | 3.24 | Med=780 \| P5–P95=[74.4; 3.06e+03] |
| total_ht_reversed.1 | float64 | 42935 | 5861 | 3.25 | Med=452 \| P5–P95=[45.9; 1.89e+03] |
| total_ht_reversed_old | object | 0 | 180520 | 100.00 |  |
| total_old | float64 | 18866 | 118735 | 65.77 | Med=833 \| P5–P95=[77.6; 3.2e+03] |
| total_tax | float64 | 34141 | 5861 | 3.25 | Med=100 \| P5–P95=[10; 414] |
| total_ttc | float64 | 44588 | 31 | 0.02 | Med=1.02e+03 \| P5–P95=[96.7; 4e+03] |
| total_ttc.1 | float64 | 27093 | 5861 | 3.25 | Med=600 \| P5–P95=[60; 2.48e+03] |
| total_ttc_old | float64 | 21816 | 118751 | 65.78 | Med=870 \| P5–P95=[81.3; 3.39e+03] |
| total_ttc_reversed | float64 | 57367 | 5856 | 3.24 | Med=927 \| P5–P95=[87.5; 3.65e+03] |
| total_ttc_reversed.1 | float64 | 50432 | 5861 | 3.25 | Med=540 \| P5–P95=[54.5; 2.25e+03] |
| total_ttc_reversed_old | object | 0 | 180520 | 100.00 |  |
| transformed_at | object | 1170 | 122643 | 67.94 |  |
| tva | float64 | 2 | 5891 | 3.26 | Med=0.2 \| P5–P95=[0.2; 0.2] |
| tva.1 | float64 | 2 | 5861 | 3.25 | Med=0.2 \| P5–P95=[0.2; 0.2] |
| tva_old | object | 0 | 180520 | 100.00 |  |
| type_paiement_id1 | object | 0 | 180520 | 100.00 |  |
| type_paiement_id2 | object | 0 | 180520 | 100.00 |  |
| unit_price_reversed | float64 | 14907 | 114193 | 63.26 | Med=86.4 \| P5–P95=[26.7; 1.17e+03] |
| unit_price_ttc_reversed | float64 | 14873 | 114288 | 63.31 | Med=103 \| P5–P95=[31.8; 1.4e+03] |
| update_version | float64 | 2 | 99329 | 55.02 | Med=3 \| P5–P95=[3; 3] |
| updated_at | object | 112203 | 5853 | 3.24 |  |
| updated_by | object | 5345 | 109764 | 60.80 |  |
| validated_at | object | 1136 | 166067 | 91.99 |  |
| validite | object | 5 | 2341 | 1.30 |  |
| version | bool | 1 | 0 | 0.00 |  |
| waste_collection_point_address | object | 0 | 180520 | 100.00 |  |
| waste_collection_point_city | object | 0 | 180520 | 100.00 |  |
| waste_collection_point_name | object | 0 | 180520 | 100.00 |  |
| waste_collection_point_postal_code | object | 0 | 180520 | 100.00 |  |
| waste_collection_point_type | object | 0 | 180520 | 100.00 |  |
| waste_management | object | 1 | 180516 | 100.00 |  |
| waste_management_type | object | 0 | 180520 | 100.00 |  |
| waste_quantity | object | 0 | 180520 | 100.00 |  |
| waste_type | object | 0 | 180520 | 100.00 |  |
| waste_unit | object | 0 | 180520 | 100.00 |  |
### acces_pro_echeance_devis_2022

| Colonne | Type | #Uniques | #Nulls | % Null | Résumé |
|---|---:|---:|---:|---:|---|
| id | int64 | 500000 | 0 | 0.00 | Med=9.86e+05 \| P5–P95=[6.91e+05; 1.3e+06] |
| devis_id | int64 | 92573 | 0 | 0.00 | Med=4.85e+05 \| P5–P95=[3.23e+05; 6.74e+05] |
| date | object | 1204 | 0 | 0.00 |  |
| duree | object | 1320 | 417777 | 83.56 |  |
| montant | float64 | 39241 | 0 | 0.00 | Med=224 \| P5–P95=[59.1; 907] |
| montant_old | object | 6194 | 456974 | 91.39 |  |
### acces_pro_sub_category

| Colonne | Type | #Uniques | #Nulls | % Null | Résumé |
|---|---:|---:|---:|---:|---|
| id | int64 | 113 | 0 | 0.00 | Med=476 \| P5–P95=[6.6; 821] |
| category_id | int64 | 6 | 0 | 0.00 | Med=2 \| P5–P95=[1; 11] |
| deleted_at | object | 1 | 112 | 99.12 |  |
| deleted_by | object | 1 | 112 | 99.12 |  |
| is_default | bool | 2 | 0 | 0.00 |  |
| is_delete | bool | 1 | 0 | 0.00 |  |
| is_view_by | float64 | 2 | 10 | 8.85 | Med=0 \| P5–P95=[0; 0] |
| name | object | 108 | 0 | 0.00 |  |
| ordre | int64 | 7 | 0 | 0.00 | Med=1e+04 \| P5–P95=[999; 1e+04] |

## 3) Clés & Relations (déductions)

**Clés primaires candidates :**
- `acces_pro_category.id`
- `acces_pro_client.id`
- `acces_pro_devis_sub_category.id`
- `acces_pro_devis_with_prestation.id`
- `acces_pro_echeance_devis_2022.id`
- `acces_pro_sub_category.id`

**Relations probables (FK → PK) :**
- `acces_pro_client.user_id` → `à confirmer.id`
- `acces_pro_client.agent_id` → `à confirmer.id`
- `acces_pro_client.fusion_client_id` → `à confirmer.id`
- `acces_pro_client.profil_urssaf_id` → `à confirmer.id`
- `acces_pro_client.adresse_facturation_id` → `à confirmer.id`
- `acces_pro_client.utilisateur_id` → `à confirmer.id`
- `acces_pro_client.emailing_contact_id` → `à confirmer.id`
- `acces_pro_client.authenticator_id` → `à confirmer.id`
- `acces_pro_devis_sub_category.devis_id` → `acces_pro_devis_with_prestation.id`
- `acces_pro_devis_sub_category.sub_category_id` → `acces_pro_sub_category.id`
- `acces_pro_devis_sub_category.collection_point_id` → `à confirmer.id`
- `acces_pro_devis_with_prestation.user_id` → `à confirmer.id`
- `acces_pro_devis_with_prestation.client_id` → `acces_pro_client.id`
- `acces_pro_devis_with_prestation.categorie_id` → `acces_pro_category.id`
- `acces_pro_devis_with_prestation.sub_categorie_id` → `acces_pro_sub_category.id`
- `acces_pro_devis_with_prestation.type_paiement_id` → `à confirmer.id`
- `acces_pro_devis_with_prestation.old_client_id` → `à confirmer.id`
- `acces_pro_devis_with_prestation.parent_id` → `à confirmer.id`
- `acces_pro_devis_with_prestation.monitoring_id` → `à confirmer.id`
- `acces_pro_devis_with_prestation.reject_mofit_type_id` → `à confirmer.id`
- `acces_pro_devis_with_prestation.papier_administratif_logo_id` → `à confirmer.id`
- `acces_pro_devis_with_prestation.papier_administratif_cachet_id` → `à confirmer.id`
- `acces_pro_devis_with_prestation.adherent_quote_request_id` → `à confirmer.id`
- `acces_pro_devis_with_prestation.devis_id` → `acces_pro_devis_with_prestation.id`
- `acces_pro_devis_with_prestation.sub_category_id` → `acces_pro_sub_category.id`
- `acces_pro_devis_with_prestation.collection_point_id` → `à confirmer.id`
- `acces_pro_devis_with_prestation.category_id` → `acces_pro_category.id`
- `acces_pro_echeance_devis_2022.devis_id` → `acces_pro_devis_with_prestation.id`
- `acces_pro_sub_category.category_id` → `acces_pro_category.id`

## 4) Qualité & points de vigilance

- Colonnes très **nulles** à surveiller (≥80% null) :
  - `acces_pro_category.is_view_by` : 100.00% null
  - `acces_pro_client.phone` : 86.13% null
  - `acces_pro_client.fax` : 100.00% null
  - `acces_pro_client.site` : 100.00% null
  - `acces_pro_client.address_chantier` : 87.79% null
  - `acces_pro_client.postal_code_chantier` : 87.68% null
  - `acces_pro_client.city_chantier` : 87.68% null
  - `acces_pro_client.agent_id` : 100.00% null
  - `acces_pro_client.fusion_client_id` : 100.00% null
  - `acces_pro_client.date_fusion` : 100.00% null
  - `acces_pro_client.email_newsletter` : 92.90% null
  - `acces_pro_client.newsletter` : 88.46% null
  - `acces_pro_client.deleted_at` : 100.00% null
  - `acces_pro_client.deleted_by` : 100.00% null
  - `acces_pro_client.blocked_by_client` : 89.23% null
  - `acces_pro_client.utilisateur_id` : 98.89% null
  - `acces_pro_client.emailing_contact_id` : 87.90% null
  - `acces_pro_devis_sub_category.waste_type` : 83.30% null
  - `acces_pro_devis_sub_category.waste_management_type` : 84.21% null
  - `acces_pro_devis_sub_category.waste_quantity` : 83.30% null
  - `acces_pro_devis_sub_category.waste_unit` : 83.30% null
  - `acces_pro_devis_sub_category.waste_collection_point_address` : 84.43% null
  - `acces_pro_devis_sub_category.waste_collection_point_postal_code` : 84.60% null
  - `acces_pro_devis_sub_category.waste_collection_point_city` : 84.60% null
  - `acces_pro_devis_sub_category.waste_collection_point_name` : 84.43% null
  - `acces_pro_devis_sub_category.waste_collection_point_type` : 84.21% null
  - `acces_pro_devis_sub_category.collection_point_id` : 100.00% null
  - `acces_pro_devis_with_prestation.interventions` : 91.83% null
  - `acces_pro_devis_with_prestation.schedule` : 98.13% null
  - `acces_pro_devis_with_prestation.designation_article` : 100.00% null
  - `acces_pro_devis_with_prestation.prix_ttc` : 100.00% null
  - `acces_pro_devis_with_prestation.deleted_at` : 91.74% null
  - `acces_pro_devis_with_prestation.numero_avoir` : 95.85% null
  - `acces_pro_devis_with_prestation.num_paiement` : 100.00% null
  - `acces_pro_devis_with_prestation.date_versement` : 100.00% null
  - `acces_pro_devis_with_prestation.num_versement` : 100.00% null
  - `acces_pro_devis_with_prestation.compte_versement` : 100.00% null
  - `acces_pro_devis_with_prestation.date_encaissement` : 100.00% null
  - `acces_pro_devis_with_prestation.compte_encaissement` : 100.00% null
  - `acces_pro_devis_with_prestation.num_paiement1` : 100.00% null
  - `acces_pro_devis_with_prestation.type_paiement_id1` : 100.00% null
  - `acces_pro_devis_with_prestation.num_paiement2` : 100.00% null
  - `acces_pro_devis_with_prestation.type_paiement_id2` : 100.00% null
  - `acces_pro_devis_with_prestation.commentaire` : 81.09% null
  - `acces_pro_devis_with_prestation.old_client_id` : 99.90% null
  - `acces_pro_devis_with_prestation.date_intervention` : 100.00% null
  - `acces_pro_devis_with_prestation.frequence_intevention` : 100.00% null
  - `acces_pro_devis_with_prestation.frequence_intevention_par` : 100.00% null
  - `acces_pro_devis_with_prestation.date_signature` : 80.98% null
  - `acces_pro_devis_with_prestation.date_impression` : 96.42% null
  - `acces_pro_devis_with_prestation.date_envoi_courrier` : 100.00% null
  - `acces_pro_devis_with_prestation.bak_com` : 100.00% null
  - `acces_pro_devis_with_prestation.monitoring_id` : 99.85% null
  - `acces_pro_devis_with_prestation.client_additional_address` : 96.60% null
  - `acces_pro_devis_with_prestation.client_additional_address_chaniter` : 96.77% null
  - `acces_pro_devis_with_prestation.deleted_by` : 95.90% null
  - `acces_pro_devis_with_prestation.tax_reversed_old` : 100.00% null
  - `acces_pro_devis_with_prestation.total_ht_reversed_old` : 100.00% null
  - `acces_pro_devis_with_prestation.total_ttc_reversed_old` : 100.00% null
  - `acces_pro_devis_with_prestation.prix_ttc_old` : 100.00% null
  - `acces_pro_devis_with_prestation.tva_old` : 100.00% null
  - `acces_pro_devis_with_prestation.reject_object` : 99.42% null
  - `acces_pro_devis_with_prestation.validated_at` : 91.99% null
  - `acces_pro_devis_with_prestation.refused_at` : 96.74% null
  - `acces_pro_devis_with_prestation.reject_mofit_type_id` : 99.02% null
  - `acces_pro_devis_with_prestation.signature_type` : 99.82% null
  - `acces_pro_devis_with_prestation.papier_administratif_cachet_id` : 86.03% null
  - `acces_pro_devis_with_prestation.reference_utilisateur` : 99.47% null
  - `acces_pro_devis_with_prestation.adherent_quote_request_id` : 99.57% null
  - `acces_pro_devis_with_prestation.nb_passage` : 100.00% null
  - `acces_pro_devis_with_prestation.waste_type` : 100.00% null
  - `acces_pro_devis_with_prestation.waste_management_type` : 100.00% null
  - `acces_pro_devis_with_prestation.waste_quantity` : 100.00% null
  - `acces_pro_devis_with_prestation.waste_unit` : 100.00% null
  - `acces_pro_devis_with_prestation.waste_collection_point_address` : 100.00% null
  - `acces_pro_devis_with_prestation.waste_collection_point_postal_code` : 100.00% null
  - `acces_pro_devis_with_prestation.waste_collection_point_city` : 100.00% null
  - `acces_pro_devis_with_prestation.waste_collection_point_name` : 100.00% null
  - `acces_pro_devis_with_prestation.waste_collection_point_type` : 100.00% null
  - `acces_pro_devis_with_prestation.waste_management` : 100.00% null
  - `acces_pro_devis_with_prestation.collection_point_id` : 100.00% null
  - `acces_pro_devis_with_prestation.total_ht_old.1` : 100.00% null
  - `acces_pro_devis_with_prestation.deleted_at.1` : 100.00% null
  - `acces_pro_devis_with_prestation.deleted_by.1` : 100.00% null
  - `acces_pro_echeance_devis_2022.montant_old` : 91.39% null
  - `acces_pro_echeance_devis_2022.duree` : 83.56% null
  - `acces_pro_sub_category.deleted_at` : 99.12% null
  - `acces_pro_sub_category.deleted_by` : 99.12% null
- Cohérence **HT/TVA/TTC** à contrôler (ex : `total_ht`, `tva`, `total_ttc` dans `acces_pro_devis_with_prestation`).
- Normalisation de noms : colonnes **dupliquées** / variantes (`category_id` vs `categorie_id`, `sub_category_id` vs `sub_categorie_id`).
- Traçabilité : horodatage d'**envoi de devis** non observé explicitement dans le profil (événement métier présent néanmoins).
- Échéancier : vérifier `Σ échéances` vs `total_ttc` ; champs `montant_old`, `duree` très nuls indiquent un **legacy**.
- Suppressions logiques : colonnes `is_delete`, `is_deleted`, `deleted_at`, `deleted_by` → clarifier conventions.

## 5) Mapping colonnes ↔ concepts métier

- **Client** : `acces_pro_client.*` (identité/adresses/contact).
- **Devis** : `acces_pro_devis_with_prestation.id`, `date_heure_creation`, `reference`, `status`, `total_ht`, `total_ttc`, `tva`, `accepted_at`, `refused_at`, `signature_date`, `transformed_at`, `type_paiement_id*`, `user_id`.
- **Prestations/Lignes** : `qty_article`, `pu_article`, `designation_article` (dans la même table devis_prestation).
- **Catégories** : `acces_pro_category.*` (`tva`, `tva_ile_reunion`, `is_reduced_tax`, `code_nature`, `plafond`, `tax_credit_ceiling`, `compte_comptable`).
- **Sous-catégories** : `acces_pro_sub_category.*` avec `category_id`.
- **Lien devis ↔ sous-catégories** : `acces_pro_devis_sub_category` (table de liaison).
- **Échéances** : `acces_pro_echeance_devis_2022.devis_id`, `date`, `montant`.

## 6) Invariants métier (à vérifier sur données)

- **Un devis a un client** : `client_id` non nul et référencé dans `acces_pro_client.id`.
- **Accepté XOR Refusé** : si `accepted_at` non nul alors `refused_at` nul (et inversement).
- **TTC cohérent** : `abs(total_ttc - total_ht * (1 + tva_effective)) ≤ ε`.
- **Échéances** : `Σ montants échéances ≤ total_ttc` ; dates d'échéance ≥ date de création du devis.
- **Catégories** : `sub_category.category_id` doit référencer une `category` existante.

## 7) Alignement avec le Chapitre 1 (référentiel pédagogique)

- La structure reflète le **scénario Devis & Facturation** présenté au Chapitre 1 et prépare la suite DDD (contexts & agrégats).
- Les objets **Client, Devis, Facture, Paiement** correspondent aux entités suggérées dans le chapitre (exemples de tables).
- Le document servira de base pour le **Glossaire v1** (≥20 termes) et pour l’**Event Storming** (événements/commandes/acteurs).