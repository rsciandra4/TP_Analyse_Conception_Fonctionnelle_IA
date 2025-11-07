# Gaps Analysis — Partie 4  
**Projet ESGI M2 — DDD & IA**  
*Auteurs : Romain SCIANDRA & Omar MOSTAFA*  

---
## 1. État de la base existante

Les tables `acces_pro_*` contiennent les structures de base nécessaires :  
- **Clients**, **Devis**, **Prestations**, **Factures**, **Catégories**.  
Cependant, ces données sont **incomplètes** pour une exploitation décisionnelle, prédictive ou d’automatisation des processus.

---

## 2. Données manquantes par domaine

| **Domaine** | **Donnée manquante / insuffisante** | **Justification métier / IA** | **Proposition d’enrichissement** |
|:--|:--|:--|:--|
| **Client** | `type_client` (B2B / B2C) | Impossible de distinguer les professionnels des clients finaux ; empêche toute segmentation marketing. | Ajouter un champ `type_client` (ENUM : 'Professionnel', 'Particulier'). |
| | `canal_acquisition` | Identifier l’origine du client (site web, appel, recommandation) permet de mesurer les performances commerciales. | Ajouter une table de référence `canaux_acquisition`. |
| | `note_satisfaction` | Nécessaire pour la fidélisation et le scoring prédictif (probabilité de réachat). | Collecte via formulaire post-prestation. |
| | `segment` (IA) | Manque de variable catégorielle pour classification ou clustering IA. | Ajout automatique via algorithme de segmentation. |
| **Devis** | `motif_refus` | Impossible d’analyser les causes de refus. | Champ textuel / table des motifs normalisés. |
| | `delai_acceptation` | Mesure du temps de transformation devis→commande utile au scoring de conversion. | Calculé automatiquement (date_acceptation - date_envoi). |
| | `canal_demande` | Identifier la provenance d’un devis (email, plateforme, téléphone). | Ajouter champ `canal_demande` avec table de référence. |
| **Prestation** | `temps_reel_execution` | Pas de suivi précis du temps réel de réalisation. | Ajout champ `duree_reelle` + logs opérationnels. |
| | `qualite_realisation` | Manque d’indicateur de qualité (réclamations, retour client). | Créer table `feedbacks` reliée à `prestation_id`. |
| | `materiaux_utilises` | Donnée importante pour la traçabilité environnementale et IA (analyse des coûts matière). | Ajouter table `materiaux` liée à `prestation_id`. |
| **Facture** | `mode_paiement` | Actuellement absent du dataset ; essentiel pour l’automatisation comptable. | Ajout champ `mode_paiement` (CB, virement, chèque). |
| | `date_paiement_reelle` | Manque d’historisation de paiement effectif. | Ajout dans table `paiements`. |
| | `remise_commerciale` | Nécessaire pour mesurer les marges et politiques tarifaires. | Ajouter champ `remise_ht`. |
| **Paiement** | `retard_jours` | Manque d’indicateur pour la gestion du risque de paiement. | Calcul automatique via `date_paiement - date_echeance`. |
| | `statut_paiement` | Distinction manquante entre “en attente”, “payé partiel”, “payé total”. | Champ `statut_paiement` (ENUM). |

---

## 3. Données à normaliser ou consolider

| **Problème identifié** | **Impact** | **Action recommandée** |
|:--|:--|:--|
| Multiplicité de noms incohérents (`firstname_client`, `prenom_client`, etc.) | Complexifie l’intégration IA / SQL | Créer un dictionnaire de données consolidé avec noms unifiés (`clients.firstname`, `clients.lastname`, etc.). |
| Valeurs libres dans `status` (devis, factures) | Risque de doublons / orthographes différentes | Imposer une liste de statuts contrôlée : `Brouillon`, `Envoyé`, `Accepté`, `Refusé`, `Payé`, `Annulé`. |
| Colonnes de dates non typées (`date`, `date_creation`, `created_at`) | Problèmes de cohérence temporelle | Homogénéiser en type `DATETIME` + convention ISO 8601. |
| TVA incohérente entre devis / catégories | Biais dans calculs financiers | Centraliser les taux dans `categories` + contrainte FK. |

---

## 4. Données à créer pour l’IA

| **Donnée / Variable dérivée** | **Usage IA envisagé** | **Mode de création / calcul** |
|:--|:--|:--|
| `taux_conversion_devis` | Prédire la probabilité qu’un devis soit accepté. | Calculé par client : nb_devis_acceptés / nb_total_devis. |
| `score_client` | Détection clients à fort potentiel. | Calcul IA supervisée (historique + montants). |
| `delai_paiement_moyen` | Indicateur comportemental pour scoring risque. | Moyenne `(date_paiement - date_facture)` par client. |
| `valeur_vie_client (CLV)` | Prédire la valeur totale attendue d’un client. | Modèle IA basé sur achats passés. |
| `indice_fidélité` | Mesurer la récurrence des commandes. | Nombre de devis acceptés / période. |
| `categorie_rentable` | Identifier les prestations les plus profitables. | Calcul : marge moyenne par `sub_category_id`. |

---

## 5. Plan d’enrichissement progressif

| **Étape** | **Action principale** | **Responsable / outil** | **Finalité** |
|:--|:--|:--|:--|
| 1. | Unification du dictionnaire de données (`clients`, `devis`, `factures`) | Équipe Data / SQL | Structuration base dédiée |
| 2. | Ajout des champs manquants critiques (motif_refus, canal_acquisition, mode_paiement) | Développeurs / DevOps | Cohérence métier |
| 3. | Collecte des variables IA (satisfaction, délai, segment) | CRM / Application front | Analyse prédictive |
| 4. | Construction d’un entrepôt analytique | Data Engineering | Agrégation & reporting |
| 5. | Création de jeux de données enrichis pour IA | Data Science | Modélisation & Machine Learning |

---