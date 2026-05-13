# §12 — Annexes

*Dernière mise à jour : 13/05/2026*

## A.1 — Backlog technique (dette documentée)

Les éléments ci-dessous sont des améliorations identifiées mais reportées, documentées conformément à la pratique ADR (section "Dette acceptée"). Leur traitement est planifié mais non engagé pour la v1.

| ID | Description | Source | Priorité | Sprint cible |
|---|---|---|---|---|
| DEBT-01 | Remboursements automatiques lors de l'annulation d'un événement | ADR-001 | Moyenne | v2 |
| DEBT-02 | Alerte automatique sur taille de la DLQ RabbitMQ | ADR-001 | Haute | TP 1.8 |
| DEBT-03 | Configuration Redis AOF persistence en production | ADR-003 | Haute | TP 1.8 |
| DEBT-04 | Multi-tenancy (support de plusieurs écoles) | §5 (limites) | Faible | v3 |
| DEBT-05 | Fiches §7 complètes pour EventModule, NotificationModule, UserModule | §10 matrice | Haute | TP 1.8 |
| DEBT-06 | Section WCAG 2.1 AA — audit et conformité frontend | §10 (ENF-05) | Haute | TP 1.8 |
| DEBT-07 | Stratégie i18n — choix librairie et structure des traductions | §10 (ENF-08) | Moyenne | TP 1.8 |

---

## A.2 — Références

| Ressource | URL / Référence |
|---|---|
| Stripe API Documentation | https://stripe.com/docs/api |
| Stripe Webhooks — Idempotency | https://stripe.com/docs/webhooks/best-practices |
| OpenID Connect Core 1.0 | https://openid.net/specs/openid-connect-core-1_0.html |
| RFC 7807 — Problem Details for HTTP APIs | https://datatracker.ietf.org/doc/html/rfc7807 |
| WCAG 2.1 Guidelines | https://www.w3.org/TR/WCAG21/ |
| RabbitMQ — Publisher Confirms | https://www.rabbitmq.com/confirms.html |
| PostgreSQL — Explicit Locking | https://www.postgresql.org/docs/current/explicit-locking.html |
| RGPD — CNIL Guide développeurs | https://www.cnil.fr/fr/guide-rgpd-du-developpeur |
| JSON Schema Draft-07 | https://json-schema.org/draft-07/schema |

---

## A.3 — Décisions hors-périmètre à surveiller

Les points suivants ont été identifiés comme potentiellement impactants pour l'architecture mais ont été écartés du périmètre v1. Ils doivent faire l'objet d'un ADR si le contexte change :

- **Progressive Web App (PWA)** : si une expérience mobile améliorée est demandée avant le développement d'une app native, une PWA peut être ajoutée sans refactoring majeur du backend.
- **API Gateway avancée** : si des besoins d'API management (portail développeur, clés API, analytics) émergent, Kong peut être configuré en mode gestionnaire d'API plutôt que simple proxy.
- **Search engine** : si le catalogue d'événements dépasse 10 000 entrées, l'ajout d'Elasticsearch ou d'une extension PostgreSQL full-text (pg_trgm) sera nécessaire.
