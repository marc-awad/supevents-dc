# §3 — Glossaire

Ce glossaire rend le document autonome. Tout lecteur — développeur intégrant l'équipe, auditeur externe, commanditaire — doit pouvoir comprendre n'importe quel terme technique utilisé dans cette DCT sans avoir à chercher ailleurs.

Les définitions sont contextualisées à SupEvents, pas copiées de Wikipedia.

---

| Terme | Définition | Première occurrence |
|-------|-----------|---------------------|
| **SSO** | Single Sign-On — dans SupEvents, mécanisme permettant à un étudiant ou organisateur de s'authentifier une seule fois via le portail de l'école (identifiants ENT) pour accéder à la plateforme, sans créer un second compte. | §4 |
| **OIDC** | OpenID Connect — protocole d'authentification basé sur OAuth 2.0, utilisé par le SSO de l'école pour transmettre à SupEvents l'identité de l'utilisateur (sub, email, rôle) sous forme de JWT signé. | §4 |
| **RBAC** | Role-Based Access Control — dans SupEvents, les droits d'accès sont définis par rôle : `etudiant` (consulte et s'inscrit), `organisateur` (crée et gère ses événements), `admin` (supervise toute la plateforme). Un utilisateur ne peut avoir qu'un seul rôle actif. | §4 |
| **RGPD** | Règlement Général sur la Protection des Données — cadre légal européen imposant à SupEvents de limiter la collecte de données personnelles, d'obtenir le consentement, de garantir le droit d'accès, de rectification et à l'oubli des étudiants. | §4 |
| **PCI-DSS** | Payment Card Industry Data Security Standard — norme de sécurité pour les paiements par carte bancaire. SupEvents délègue intégralement la conformité PCI-DSS à Stripe : aucune donnée de carte (numéro, CVV, date d'expiration) ne transite ni n'est stockée dans nos systèmes. | §4 |
| **Webhook** | Dans SupEvents, notification HTTP envoyée par Stripe en `POST` vers notre endpoint `/api/v1/payments/webhook` pour confirmer ou signaler l'échec d'un paiement. Authentifié par signature HMAC-SHA256 via l'en-tête `Stripe-Signature`, pas par JWT. | §6.2 |
| **Broker de messages** | Composant intermédiaire (RabbitMQ dans SupEvents) qui reçoit des événements publiés par un module producteur et les distribue aux modules consommateurs abonnés, permettant un découplage total entre les services (ex : PaymentModule publie `payment.captured`, NotificationModule le consomme pour envoyer l'email de confirmation). | §6.1 |
| **API REST** | Interface de programmation exposant les ressources SupEvents (événements, tickets, utilisateurs) via des endpoints HTTP standardisés, préfixés `/api/v1/`, avec des codes de retour sémantiques (200, 201, 400, 401, 403, 404, 409, 422, 500). | §8 |
| **Early-bird (billet)** | Type de billet proposé à tarif réduit pendant une période limitée avant la date de l'événement, géré dans SupEvents par une contrainte de date de fin sur l'entité `Ticket` et une jauge dédiée distincte de la jauge principale. | §6.4 |
| **CRUD** | Create, Read, Update, Delete — les quatre opérations de base sur les ressources SupEvents. Tous les modules exposent au minimum ces opérations sur leurs entités principales via l'API REST. | §8 |
| **SLA** | Service Level Agreement — dans SupEvents, accord sur le niveau de disponibilité de la plateforme : 99,5 % de disponibilité mensuelle, soit un budget d'indisponibilité maximum de 3h 39min par mois. | §9 |
| **P95** | 95e percentile de latence — seuil en dessous duquel 95 % des requêtes doivent aboutir. L'exigence ENF-01 impose un P95 < 500 ms pour les endpoints critiques (inscription, catalogue) sous charge nominale de 500 utilisateurs simultanés. | §9 |
| **WCAG** | Web Content Accessibility Guidelines — standards d'accessibilité web. SupEvents vise la conformité WCAG 2.1 niveau AA, imposant notamment des contrastes suffisants, une navigation au clavier complète et des labels ARIA sur tous les formulaires. | §9 |
| **i18n** | Internationalisation — capacité de SupEvents à afficher son interface en plusieurs langues. La v1 cible le français uniquement, mais l'architecture doit permettre l'ajout de langues sans refactoring majeur (textes externalisés dans des fichiers de traduction). | §5 |
| **CI/CD** | Continuous Integration / Continuous Deployment — dans SupEvents, pipeline GitHub Actions qui valide automatiquement chaque pull request (lint, tests, rendu Mermaid, vérification des liens) avant tout merge sur `main`. | §12 |
| **JWT** | JSON Web Token — token signé émis par le module AuthModule après validation OIDC, contenant les claims de l'utilisateur (sub, email, role, exp). Transmis dans l'en-tête `Authorization: Bearer <token>` sur toutes les requêtes API authentifiées. | §7 |
| **PaymentIntent** | Objet Stripe représentant une intention de paiement pour une inscription SupEvents. Créé par PaymentModule, il encapsule le montant, la devise et l'identifiant client Stripe. Son statut (`requires_confirmation`, `succeeded`, `canceled`) pilote la machine à états du ticket. | §7 |
| **QR Code** | Code-barres 2D généré par TicketModule lors de la confirmation d'un ticket. Contient une signature HMAC de l'identifiant du ticket, permettant au validateur à l'entrée de l'événement de vérifier l'authenticité sans consulter la base de données. | §7 |
| **Dead Letter Queue (DLQ)** | File RabbitMQ de quarantaine dans laquelle les messages non traités après N tentatives sont redirigés. Dans SupEvents, NotificationModule envoie dans la DLQ les emails qui ont échoué 3 fois afin de déclencher une alerte et un traitement manuel. | §7 |
| **SPA** | Single Page Application — architecture frontend de SupEvents, implémentée en React. Le navigateur charge l'application une seule fois ; les navigations suivantes sont gérées côté client via React Router sans rechargement de page, améliorant la fluidité sur mobile. | §6.1 |
| **idempotence** | Propriété d'une opération qui produit le même résultat qu'on l'exécute une ou plusieurs fois. Dans SupEvents, les webhooks Stripe et les créations de tickets sont idempotents : un replay du même événement ne crée pas de doublon ni ne débite deux fois. | §7 |
