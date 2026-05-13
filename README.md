# SupEvents — Document de Conception Technique

> 🎓 Plateforme de gestion d'événements étudiants  
> **Version DCT :** 0.7 | **Statut :** 🟡 Brouillon | **Dernière mise à jour :** 13/05/2026

---

## Sommaire cliquable

| § | Section | Fichier | Statut |
|---|---|---|---|
| §1 | Page de garde | [docs/01-page-de-garde.md](docs/01-page-de-garde.md) | ✅ |
| §2 | Historique des révisions | [docs/02-historique-revisions.md](docs/02-historique-revisions.md) | ✅ |
| §3 | Glossaire | [docs/03-glossaire.md](docs/03-glossaire.md) | ✅ |
| §4 | Contexte et objectifs | [docs/04-contexte-objectifs.md](docs/04-contexte-objectifs.md) | ✅ |
| §5 | Périmètre et limites | [docs/05-perimetre-limites.md](docs/05-perimetre-limites.md) | ✅ |
| §6.1 | Vue logique (C4 Context, Containers, Composants) | [docs/06-architecture-generale/06.1-vue-logique.md](docs/06-architecture-generale/06.1-vue-logique.md) | ✅ |
| §6.2 | Vue des processus (séquences) | [docs/06-architecture-generale/06.2-vue-processus.md](docs/06-architecture-generale/06.2-vue-processus.md) | ✅ |
| §6.3 | Vue de déploiement | [docs/06-architecture-generale/06.3-vue-deploiement.md](docs/06-architecture-generale/06.3-vue-deploiement.md) | 🟡 |
| §6.4 | Vue des données (ER, dictionnaire, RGPD) | [docs/06-architecture-generale/06.4-vue-donnees.md](docs/06-architecture-generale/06.4-vue-donnees.md) | ✅ |
| §7 | Conception détaillée par module | [docs/07-conception-detaillee/07-modules.md](docs/07-conception-detaillee/07-modules.md) | 🟡 |
| §8 | Interfaces & contrats d'API | [docs/08-interfaces-api.md](docs/08-interfaces-api.md) | ✅ |
| §9 | Exigences transverses (STRIDE, Performance, RGPD) | [docs/09-exigences-transverses.md](docs/09-exigences-transverses.md) | ✅ |
| §10 | Matrice de traçabilité | [docs/10-matrice-tracabilite.md](docs/10-matrice-tracabilite.md) | ✅ |
| §11 | Architecture Decision Records | [docs/adr/README.md](docs/adr/README.md) | ✅ |
| §12 | Annexes | [docs/12-annexes.md](docs/12-annexes.md) | 🟡 |

### ADR — Décisions d'architecture

| ADR | Titre | Statut |
|---|---|---|
| [ADR-001](docs/adr/ADR-001.md) | RabbitMQ retenu comme broker de messages | ✅ Accepté |
| [ADR-002](docs/adr/ADR-002.md) | Verrou pessimiste pour la gestion de la concurrence | ✅ Accepté |
| [ADR-003](docs/adr/ADR-003.md) | Idempotence webhooks Stripe via Redis | ✅ Accepté |

---

## Qualité documentaire

| Ressource | Fichier |
|---|---|
| Grille d'audit qualité (TP 1.7) | [docs/audit-qualite.md](docs/audit-qualite.md) |
| Validation du pipeline CI | [docs/ci-validation.md](docs/ci-validation.md) |

**Score audit qualité :** 53 / 60 (88 %)  
**Pipeline CI :** ✅ Opérationnel (lint + liens + Mermaid)

---

## Conventions Git

**Branches :** `docs/section-XX-description`  
**Commits :** `docs(§XX): description courte au présent`  
**CODEOWNERS :** [.github/CODEOWNERS](.github/CODEOWNERS)  

---

## Auteurs

| Auteur | GitHub | Responsabilités |
|---|---|---|
| Marc AWAD | @marc-awad | Architecture, données, contexte, traçabilité |
| Killian DOIZELET | @kikidrain | Paiement, sécurité, API, CI/CD |
