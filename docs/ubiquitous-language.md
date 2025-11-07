*Auteurs : Romain SCIANDRA & Omar MOSTAFA*  
---
# Glossaire (Ubiquitous Language – V2)

<br>

| **Champ**                   | **Définition métier**                                                                  | **Exemple concret**                              | **Bounded Context (BC)** |
| --------------------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------ | ------------------------- |
| **Professionnel**           | Entreprise qui utilise l'application pour gérer ses clients et les prestations         | `Entreprise Paysagiste`                          | Générique                 |
| **Client**                  | Particulier ou entreprise qui demande une prestation.                                  | `Particulier : Jean Dupont`                      | BC Client                 |
| **Firstname client**        | Prénom du client tel qu’indiqué dans le dossier client ou sur le devis/facture.        | `Jean`                                           | BC Client                 |
| **Lastname client**         | Nom du client tel qu’indiqué dans le dossier client ou sur le devis/facture.           | `Dupont`                                         | BC Client                 |
| **Adresse client**          | Adresse postale complète du client, utilisée pour la correspondance et la facturation. | `12 rue des Lilas, 75014 Paris`                  | BC Client                 |
| **Phone client**            | Numéro de téléphone principal du client pour contact commercial ou SAV.                | `06 45 23 12 98`                                 | BC Client                 |
| **Email client**            | Adresse email du client pour l’envoi de devis, factures et relances.                   | `jean.dupont@gmail.com`                          | BC Client                 |
| **Client adresse**          | Adresse du client pour l'envoi de devis, factures et relances.                         | `24 avenue de la Gare, 69003 Lyon`               | BC Client  |
| **Client adresse chantier** | Adresse spécifique du chantier, qui peut être différente de l’adresse de facturation.  | `24 avenue de la Gare, 69003 Lyon`               | BC Prestation             |
| **Demande prestation**      | Un client fait une demande de prestation à l'entreprise afin de réaliser un service.   | `Venir récupérer déchets verts dans mon jardin`  | BC Prestation             |
| **Commercial**              | Commercial de l'entreprise qui gère la relation avec le client et la création du devis.| `—`                                              | BC Devis       |
| **Devis**                   | Document envoyé à un client pour lui transmettre le coût d’une prestation.             | `DEV-2025-0321`                                  | BC Devis                  |
| **Devis envoyé**            | Indique si le devis a été transmis au client (statut Oui/Non).                         | `Oui`                                            | BC Devis                  |
| **Devis accepté**           | Indique si le devis a été accepté par le client (statut Oui/Non).                      | `Oui`                                            | BC Devis                  |
| **Devis modifié**           | Indique si le devis a été modifié par le commercial.                                   | `Non`                                            | BC Devis                  |
| **Date signature devis**    | Date à laquelle le client a signé le devis.                                            | `15/03/2025`                                     | BC Devis                  |
| **Commission**              | Pourcentage ou montant attribué à un commercial pour la vente.                         | `5 %`                                            | BC Facturation |
| **Montant HT**              | Montant total hors taxes du devis ou de la facture.                                    | `2 500,00 €`                                     | BC Devis  |
| **Taux TVA**                | Taux de taxe sur la valeur ajoutée appliqué au montant HT.                             | `20 %`                                           | BC Facturation            |
| **Montant TTC**             | Montant total toutes taxes comprises, après application de la TVA.                     | `3 000,00 €`                                     | BC Facturation            |
| **Prestation**              | Description du service ou travail réalisé pour le client.                              | `Récupération déchets verts`                     | BC Prestation             |
| **Date prestation**         | Date prévue ou réalisée de la prestation.                                              | `22/04/2025`                                     | BC Prestation             |
| **Délai prestation**        | Durée prévue entre la signature et la réalisation de la prestation.                    | `30 jours`                                       | BC Prestation             |
| **Waste type**              | Type de déchet récupéré lors d’une prestation.                                         | `Gravats`                                        | BC Prestation             |
| **Waste quantity**          | Quantité estimée ou réelle de déchets récupérés.                                       | `250 kg`                                         | BC Prestation             |
| **Date versement**          | Date réelle de versement d’un acompte ou paiement partiel.                             | `10/04/2025`                                     | BC Facturation            |
| **Date encaissement**       | Date à laquelle le paiement a effectivement été reçu sur le compte.                    | `12/04/2025`                                     | BC Facturation            |
| **Num paiement**            | Numéro de référence du paiement (virement, chèque, transaction).                       | `PAY-8542`                                       | BC Facturation            |
| **Type paiement**           | Mode de paiement utilisé par le client.                                                | `Virement bancaire`                              | BC Facturation            |
| **Num versement**           | Identifiant d’un acompte ou échéance dans un plan de paiement.                         | `VRS-02`                                         | BC Facturation            |
| **Commentaire**             | Champ libre permettant d’ajouter une remarque interne ou une note client.              | `Client souhaite un contact avant intervention.` | Générique                 |
| **Devis transformé**        | Indicateur indiquant si un devis a été transformé en commande / facture.               | `Oui`                                            | BC Devis  |

