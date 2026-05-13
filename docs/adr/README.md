# Architecture Decision Records — Index

Ce répertoire contient l'ensemble des décisions d'architecture structurantes prises pour le projet SupEvents. Chaque ADR est un document immutable : une fois accepté, il n'est pas modifié — il est déprécié ou remplacé par un nouvel ADR.

---

## Registre des ADR

| N° | Titre | Statut | Date | Auteurs |
|---|---|---|---|---|
| [ADR-001](ADR-001.md) | RabbitMQ est retenu comme broker de messages pour le découplage asynchrone | ✅ Accepté | 07/05/2026 | Marc AWAD, Killian DOIZELET |
| [ADR-002](ADR-002.md) | Le verrou pessimiste (SELECT FOR UPDATE) est retenu pour la gestion de la concurrence sur la jauge des places | ✅ Accepté | 07/05/2026 | Marc AWAD, Killian DOIZELET |
| [ADR-003](ADR-003.md) | L'idempotence des webhooks Stripe est garantie par déduplication via Redis sur l'identifiant d'événement Stripe | ✅ Accepté | 07/05/2026 | Killian DOIZELET |

---

## Comment lire un ADR

Chaque ADR suit le format Nygard avec 5 sections :

1. **Contexte** — Le problème à résoudre et les contraintes en présence.
2. **Décision** — Ce qui a été choisi, en une à trois phrases.
3. **Alternatives envisagées** — Les options réellement évaluées et les raisons de leur rejet.
4. **Conséquences** — Bénéfices, coûts et dette acceptée.

---

## Template

Voir [template.md](template.md) pour créer un nouvel ADR.
