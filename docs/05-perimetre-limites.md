# §5 — Périmètre et limites

## Périmètre fonctionnel

Ce tableau distingue ce que la DCT couvre de ce qui en est explicitement exclu. Toute ambiguïté doit être tranchée ici — pas dans les réunions d'équipe six semaines après le lancement.

| Dans le périmètre | Hors périmètre |
|---|---|
| Création et gestion d'événements étudiants (conférences, soirées, ateliers, hackathons) | Application mobile native iOS / Android |
| Catalogue public d'événements consultable sans authentification | Système de gestion de billetterie physique (impression de billets papier) |
| Inscription avec authentification SSO école (OIDC) | Gestion financière et comptabilité des associations organisatrices |
| Paiement en ligne par carte bancaire via Stripe | Remboursements automatiques (déclenché manuellement par l'admin pour la v1) |
| Gestion des jauges et des listes d'attente | Streaming vidéo ou retransmission en direct des événements |
| Génération et validation de billets avec QR code | Gestion de plan de salle et attribution de places numérotées |
| Notifications email transactionnelles (confirmation, annulation) | Notifications push mobiles |
| Tableau de bord organisateur (participants, export CSV) | Intégration avec des outils tiers de gestion de projet (Notion, Trello) |
| Interface d'administration (validation organisateurs, supervision) | Module CRM ou gestion de relation client avancée |
| Gestion des droits d'accès par rôle (RBAC) | Authentification par réseau social (Google, LinkedIn) |
| Conformité RGPD (droit d'accès, oubli, rectification) | Support multi-établissements (une seule école pour la v1) |
| Accessibilité WCAG 2.1 AA | Accessibilité WCAG AAA |

---

## Limites de conception technique

Ces limites ont été posées par l'équipe comme décisions d'architecture. Elles ne figurent pas dans le CDC mais sont nécessaires pour cadrer l'implémentation et éviter les débordements de scope.

| Limite technique | Justification |
|---|---|
| **Migration de données depuis un système existant : hors périmètre** | La plateforme est créée from scratch ; aucune base de données d'événements préexistante n'a été identifiée. Toute migration éventuelle fera l'objet d'un projet séparé. |
| **Support IE11 et navigateurs obsolètes : hors périmètre** | La cible utilisateurs (étudiants 18-25 ans) utilise des navigateurs modernes à > 99 %. Supporter IE11 représenterait un coût de développement disproportionné pour un bénéfice négligeable. Navigateurs supportés : Chrome 110+, Firefox 110+, Safari 16+, Edge 110+. |
| **Multi-tenancy (plusieurs écoles) : hors périmètre v1** | L'architecture prévoit une seule école pour la v1. L'ajout du multi-tenant nécessiterait un ADR dédié et une refonte du modèle de données (ajout d'un `tenant_id` sur toutes les entités). Documenté en dette acceptée dans ADR-002. |
| **Paiements en devises étrangères : hors périmètre v1** | Tous les paiements sont en EUR. Stripe supporte nativement le multi-devises, mais la v1 simplifie intentionnellement en limitant à EUR pour éviter les complexités de change et de TVA internationale. |
| **Remboursements automatisés : hors périmètre v1** | Les remboursements sont déclenchés manuellement par un administrateur depuis le tableau de bord Stripe. L'automatisation (remboursement à l'annulation d'un événement) est prévue en v2, documentée en dette dans ADR-001. |
| **API publique ouverte à des tiers : hors périmètre** | L'API REST est strictement interne (SPA frontend + éventuels clients internes). Aucune ouverture à des partenaires tiers n'est prévue. Si ce besoin émerge, un programme OAuth2 client_credentials devra être ajouté. |
