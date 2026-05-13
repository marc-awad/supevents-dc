# §8 — Interfaces & contrats d'API

*Dernière mise à jour : 13/05/2026*

## Tableau synoptique des endpoints REST

Tous les chemins sont préfixés `/api/v1/`. La colonne `Auth` indique le mécanisme d'authentification requis. La colonne `Dépendances aval` liste les services internes ou externes appelés lors du traitement de la requête.

### Authentification

| Méthode | Chemin | Description | Auth | Codes retour | Dépendances aval |
|---|---|---|---|---|---|
| `GET` | `/api/v1/auth/login` | Redirige vers le SSO école | Public | 302 | SSO École |
| `GET` | `/api/v1/auth/callback` | Échange code OIDC → JWT applicatif | Public | 200, 400, 401 | SSO École, UserService, Redis |
| `POST` | `/api/v1/auth/refresh` | Renouvelle le JWT avec refresh token | Public | 200, 401 | Redis |
| `POST` | `/api/v1/auth/logout` | Révoque la session | JWT | 204, 401 | Redis |
| `GET` | `/api/v1/auth/me` | Profil de l'utilisateur connecté | JWT | 200, 401 | UserService |

### Catalogue d'événements

| Méthode | Chemin | Description | Auth | Codes retour | Dépendances aval |
|---|---|---|---|---|---|
| `GET` | `/api/v1/events` | Liste les événements publiés (paginée, filtrable) | Public | 200 | EventService, Redis (cache) |
| `GET` | `/api/v1/events/:id` | Détail d'un événement | Public | 200, 404 | EventService |
| `GET` | `/api/v1/events/search` | Recherche full-text par titre/description | Public | 200 | EventService |
| `POST` | `/api/v1/events` | Crée un événement (statut `draft`) | JWT + role:organisateur | 201, 400, 403 | EventService, S3 |
| `PUT` | `/api/v1/events/:id` | Met à jour un événement | JWT + role:organisateur | 200, 400, 403, 404 | EventService |
| `PATCH` | `/api/v1/events/:id/publish` | Publie un événement (draft → published) | JWT + role:organisateur | 200, 403, 409 | EventService |
| `PATCH` | `/api/v1/events/:id/cancel` | Annule un événement | JWT + role:organisateur | 200, 403, 409 | EventService, RabbitMQ |
| `DELETE` | `/api/v1/events/:id` | Supprime un événement (draft uniquement) | JWT + role:organisateur | 204, 403, 409 | EventService |

### Inscriptions (Tickets)

| Méthode | Chemin | Description | Auth | Codes retour | Dépendances aval |
|---|---|---|---|---|---|
| `POST` | `/api/v1/tickets` | Crée une réservation | JWT | 201, 400, 409, 422 | TicketService, EventService, PostgreSQL |
| `GET` | `/api/v1/tickets/me` | Liste les tickets de l'utilisateur connecté | JWT | 200 | TicketService |
| `GET` | `/api/v1/tickets/:id` | Détail d'un ticket | JWT | 200, 403, 404 | TicketService |
| `DELETE` | `/api/v1/tickets/:id` | Annule un ticket | JWT | 204, 403, 409 | TicketService, RabbitMQ |
| `POST` | `/api/v1/tickets/:id/validate` | Valide le QR code à l'entrée | JWT + role:organisateur | 200, 400, 409 | TicketService |

### Paiement

| Méthode | Chemin | Description | Auth | Codes retour | Dépendances aval |
|---|---|---|---|---|---|
| `POST` | `/api/v1/payments/initiate` | Initie le paiement (crée PaymentIntent) | JWT | 201, 400, 422 | PaymentService, Stripe |
| `POST` | `/api/v1/payments/webhook` | Reçoit les confirmations Stripe | **HMAC** (Stripe-Signature) | 200, 400 | PaymentService, TicketService, RabbitMQ |
| `GET` | `/api/v1/payments/me` | Liste les paiements de l'utilisateur | JWT | 200 | PaymentService |
| `GET` | `/api/v1/payments/:id` | Détail d'un paiement | JWT | 200, 403, 404 | PaymentService |

### Tableau de bord organisateur

| Méthode | Chemin | Description | Auth | Codes retour | Dépendances aval |
|---|---|---|---|---|---|
| `GET` | `/api/v1/organizer/events/:id/participants` | Liste des participants à un événement | JWT + role:organisateur | 200, 403, 404 | TicketService |
| `GET` | `/api/v1/organizer/events/:id/export` | Export CSV des participants | JWT + role:organisateur | 200, 403, 404 | TicketService |
| `GET` | `/api/v1/organizer/events/:id/stats` | KPIs de l'événement (inscrits, CA) | JWT + role:organisateur | 200, 403, 404 | EventService, PaymentService |

### Administration

| Méthode | Chemin | Description | Auth | Codes retour | Dépendances aval |
|---|---|---|---|---|---|
| `GET` | `/api/v1/admin/users` | Liste tous les utilisateurs | JWT + role:admin | 200, 403 | UserService |
| `PATCH` | `/api/v1/admin/users/:id/role` | Change le rôle d'un utilisateur | JWT + role:admin | 200, 403, 404 | UserService |
| `DELETE` | `/api/v1/admin/users/:id` | Supprime un compte (droit à l'oubli) | JWT + role:admin | 204, 403 | UserService |
| `GET` | `/api/v1/admin/events` | Liste tous les événements (y compris draft) | JWT + role:admin | 200, 403 | EventService |

---

## Conventions transverses

### Format des erreurs (RFC 7807 Problem Details)

Toutes les erreurs de l'API suivent le format RFC 7807 pour une cohérence maximale côté client :

```json
{
  "type": "https://supevents.school/errors/ticket-already-exists",
  "title": "Ticket already exists",
  "status": 409,
  "detail": "Un ticket actif existe déjà pour cet événement.",
  "instance": "/api/v1/tickets",
  "code": "TICKET_001",
  "timestamp": "2026-05-13T10:30:00Z",
  "traceId": "abc-123-def"
}
```

### Stratégie de versioning

Le versioning est dans le chemin (`/api/v1/`). La v2 sera ajoutée parallèlement à la v1 (pas de suppression immédiate). Les endpoints v1 restent opérationnels 6 mois après la sortie d'une v2. Un en-tête `Sunset` sera ajouté sur les routes dépréciées.

### Rate limiting et quotas

| Profil | Limite | Fenêtre |
|---|---|---|
| Utilisateur non authentifié | 60 req/min | Par IP |
| Utilisateur authentifié | 300 req/min | Par user_id |
| Organisateur (endpoints sensibles) | 100 req/min | Par user_id |
| Webhook Stripe | Illimité | — (source de confiance après vérification HMAC) |

---

## Événements asynchrones

### `ticket.confirmed`

| Champ | Valeur |
|---|---|
| **Nom** | `ticket.confirmed` |
| **Producteur** | `PaymentModule` — publié lors de la réception du webhook `payment_intent.succeeded` de Stripe |
| **Topic / exchange** | `tickets.events.v1` (durable, fanout) |
| **Consommateurs connus** | `NotificationModule` (envoie l'email de confirmation avec QR code) |
| **Garantie de livraison** | `at-least-once` — idempotence côté consommateur via `ticket_id` |
| **Stratégie de retry** | Exponentielle : 10s, 30s, 90s. Après 3 échecs → Dead Letter Queue `tickets.dlq` |

**Schéma JSON (draft-07) :**

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "TicketConfirmedEvent",
  "type": "object",
  "required": ["event_id_domain", "ticket_id", "user_id", "event_id", "qr_code_hash", "event_title", "start_date", "confirmed_at"],
  "properties": {
    "event_id_domain": {
      "type": "string",
      "format": "uuid",
      "description": "Identifiant unique de l'événement de domaine (pour idempotence)"
    },
    "ticket_id": {
      "type": "string",
      "format": "uuid",
      "description": "Identifiant du ticket confirmé"
    },
    "user_id": {
      "type": "string",
      "format": "uuid",
      "description": "Identifiant de l'étudiant"
    },
    "event_id": {
      "type": "string",
      "format": "uuid",
      "description": "Identifiant de l'événement SupEvents"
    },
    "event_title": {
      "type": "string",
      "description": "Titre de l'événement pour l'email"
    },
    "start_date": {
      "type": "string",
      "format": "date-time",
      "description": "Date et heure de début de l'événement (ISO 8601)"
    },
    "qr_code_hash": {
      "type": "string",
      "description": "Contenu du QR code (HMAC du ticket_id)"
    },
    "amount_cents": {
      "type": "integer",
      "minimum": 0,
      "description": "Montant payé en centimes (0 si gratuit)"
    },
    "confirmed_at": {
      "type": "string",
      "format": "date-time",
      "description": "Horodatage de la confirmation (ISO 8601)"
    }
  }
}
```

---

### `payment.failed`

| Champ | Valeur |
|---|---|
| **Nom** | `payment.failed` |
| **Producteur** | `PaymentModule` — publié lors de la réception d'un webhook d'échec Stripe ou lors de la détection d'un timeout par le job de réconciliation |
| **Topic / exchange** | `payments.events.v1` (durable, fanout) |
| **Consommateurs connus** | `NotificationModule` (envoie l'email d'échec avec lien de nouvelle tentative) |
| **Garantie de livraison** | `at-least-once` — idempotence côté consommateur via `stripe_event_id` |
| **Stratégie de retry** | Exponentielle : 10s, 30s, 90s. Après 3 échecs → Dead Letter Queue `payments.dlq` |

**Schéma JSON (draft-07) :**

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "PaymentFailedEvent",
  "type": "object",
  "required": ["event_id_domain", "ticket_id", "user_id", "event_id", "failure_reason", "failed_at"],
  "properties": {
    "event_id_domain": {
      "type": "string",
      "format": "uuid",
      "description": "Identifiant unique de l'événement de domaine"
    },
    "ticket_id": {
      "type": "string",
      "format": "uuid"
    },
    "user_id": {
      "type": "string",
      "format": "uuid"
    },
    "event_id": {
      "type": "string",
      "format": "uuid"
    },
    "event_title": {
      "type": "string"
    },
    "failure_reason": {
      "type": "string",
      "enum": ["card_declined", "insufficient_funds", "expired_card", "timeout", "stripe_unavailable", "ticket_already_cancelled"],
      "description": "Code d'erreur Stripe normalisé"
    },
    "stripe_event_id": {
      "type": "string",
      "description": "ID de l'événement Stripe (pour idempotence côté consommateur)"
    },
    "failed_at": {
      "type": "string",
      "format": "date-time"
    }
  }
}
```
