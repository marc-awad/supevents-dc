# §10 — Matrice de traçabilité

*Dernière mise à jour : 13/05/2026*

## Introduction — Méthodologie

Cette matrice croise les 21 exigences du cahier des charges SupEvents (13 exigences fonctionnelles EF + 8 exigences non-fonctionnelles ENF) avec les éléments produits dans cette DCT. Son objectif est de permettre à tout auditeur ou commanditaire de vérifier en moins de 30 minutes que toutes les exigences du CDC sont couvertes par une décision de conception documentée.

La colonne `Module(s)` liste les modules de §7 qui implémentent l'exigence. La colonne `Section(s) DCT` renvoie aux sections qui contribuent à la couverture. La colonne `ADR` liste les décisions d'architecture associées.

---

## §10.1 — Matrice principale

| Réf. | Intitulé (court) | Module(s) | Section(s) DCT | ADR |
|---|---|---|---|---|
| **EF-01** | Création d'événements | EventModule | §6.4 (Event), §7.4, §8 | — |
| **EF-02** | Catalogue public | EventModule | §6.1, §8, §6.4 (Event) | — |
| **EF-03** | Inscription à un événement | TicketModule, EventModule | §6.2, §7.2, §8, §6.4 (Ticket) | ADR-002 |
| **EF-04** | Paiement en ligne | PaymentModule | §6.2, §7.3, §8, §6.4 (Payment) | ADR-001, ADR-003 |
| **EF-05** | Confirmation par email | NotificationModule | §6.2, §7.5, §8 (ticket.confirmed) | ADR-001 |
| **EF-06** | Génération QR code billet | TicketModule | §6.4 (qr_code_hash), §7.2, §8 | — |
| **EF-07** | Validation QR code entrée | TicketModule | §7.2, §8 (POST /tickets/:id/validate) | — |
| **EF-08** | Tableau de bord organisateur | EventModule, TicketModule | §8 (organizer endpoints) | — |
| **EF-09** | Gestion liste d'attente | TicketModule, NotificationModule | §6.4 (WaitingListEntry), §7.2, §8 | — |
| **EF-10** | Authentification SSO | AuthModule | §6.1, §7.1, §8 (auth endpoints) | — |
| **EF-11** | Gestion des rôles (RBAC) | AuthModule, UserModule | §7.1, §7.6, §8 | — |
| **EF-12** | Administration plateforme | UserModule | §8 (admin endpoints), §7.6 | — |
| **EF-13** | Export participants (CSV) | TicketModule | §8 (GET /organizer/events/:id/export) | — |
| **ENF-01** | Performance P95 < 500 ms | EventModule, TicketModule | §9.2 (ENF-P1, ENF-P3), §6.4 (index) | ADR-002 |
| **ENF-02** | Disponibilité 99,5 % | Infrastructure | §9.2 (ENF-P2), §6.3 | — |
| **ENF-03** | Sécurité paiements (PCI-DSS) | PaymentModule | §9.1.1 (STRIDE), §7.3, §6.4 (rappel CDC) | ADR-003 |
| **ENF-04** | Conformité RGPD | UserModule, AuthModule | §9.3, §6.4 (RGPD), §7.6 | — |
| **ENF-05** | Accessibilité WCAG 2.1 AA | Frontend (SPA) | §4 (objectif implicite), §5 | ⚠️ Non couvert — à traiter en TP 1.8 |
| **ENF-06** | Scalabilité 500 users | Infrastructure, TicketModule | §9.2 (ENF-P3), §6.3, §7.2 | ADR-002 |
| **ENF-07** | Authentification sécurisée | AuthModule | §9.1.2 (STRIDE), §7.1 | — |
| **ENF-08** | i18n (architecture prête) | Frontend (SPA) | §5 (limite technique) | ⚠️ Non couvert — à traiter en TP 1.8 |

---

## §10.2 — Synthèse

- **Couverture EF : 13 / 13** — toutes les exigences fonctionnelles sont couvertes par au moins un module documenté et au moins une section DCT.
- **Couverture ENF : 6 / 8** — six exigences non-fonctionnelles sont couvertes. Deux sont en attente.

### Exigences non couvertes à ce stade

| Réf. | Intitulé | Traitement prévu |
|---|---|---|
| ENF-05 | Accessibilité WCAG 2.1 AA | À documenter en TP 1.8 — nécessite une section §6.1 frontend détaillée et un audit WCAG automatisé dans le pipeline CI (outil : axe-core ou Lighthouse). |
| ENF-08 | Architecture i18n | À documenter en TP 1.8 — choix de la librairie (react-i18next), structure des fichiers de traduction, processus de contribution. |

### Vérification de complétude par module

| Module | Nombre d'exigences couvertes |
|---|---|
| AuthModule | EF-10, EF-11, ENF-07 → 3 exigences |
| EventModule | EF-01, EF-02, EF-03 (partiel), EF-08 → 4 exigences |
| TicketModule | EF-03, EF-06, EF-07, EF-08, EF-09, EF-13, ENF-01, ENF-06 → 8 exigences |
| PaymentModule | EF-04, ENF-03 → 2 exigences |
| NotificationModule | EF-05, EF-09 (partiel) → 2 exigences |
| UserModule | EF-11, EF-12, ENF-04 → 3 exigences |

Aucun module n'apparaît sur 0 exigence (pas de module superflu). TicketModule apparaît sur 8 exigences, ce qui est élevé mais justifié : il est le module central de l'inscription, portant la machine à états du ticket, la gestion de la jauge, la génération du QR code et l'export. Sa responsabilité unique reste cohérente.
