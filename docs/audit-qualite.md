# Audit qualité DCT — Groupe Marc AWAD & Killian DOIZELET

**Audité par** : Auto-audit (posture auditeur externe)  
**Date** : 13/05/2026  
**Version DCT** : v0.6 (commit de référence : avant corrections TP 1.7)

---

## Catégorie 1 — Structure

| N° | Critère | Score | Commentaire | Action recommandée |
|---|---|---|---|---|
| S1 | Page de garde présente avec tous les champs requis (titre, version, date, statut, auteurs, destinataires) | 2 | Page de garde complète, tous les champs renseignés, statut à jour | — |
| S2 | Historique des révisions présent et à jour | 2 | Tableau complet avec toutes les versions TP par TP | — |
| S3 | Glossaire présent avec au moins 15 termes définis | 2 | 21 termes définis, tous contextualisés à SupEvents | — |
| S4 | Table des matières cliquable avec statut par section | 2 | README + table dans la page de garde | — |
| S5 | Numérotation des sections cohérente (§1 à §12) | 2 | Numérotation respectée dans tous les fichiers | — |
| S6 | Arborescence conforme au template TP 1.2 | 1 | Les fichiers §7 sont dans un sous-dossier au lieu d'un seul fichier — acceptable mais à harmoniser | Renommer `07-conception-detaillee/` → vérifier la cohérence des liens internes |

**Total : 11 / 12**

---

## Catégorie 2 — Complétude

| N° | Critère | Score | Commentaire | Action recommandée |
|---|---|---|---|---|
| C1 | Section §4 Contexte rédigée du point de vue technique (pas copiée du CDC) | 2 | Traduction claire des besoins métier en enjeux techniques, tableau des objectifs implicites | — |
| C2 | Section §5 Périmètre avec au moins 3 limites techniques | 2 | 6 limites techniques ajoutées et justifiées | — |
| C3 | Diagrammes C4 Context et Containers présents | 2 | Les deux présents avec couleurs et labels | — |
| C4 | Diagrammes de séquence pour parcours nominal et échec | 2 | Les deux présents avec fragments alt et opt | — |
| C5 | Modèle ER complet avec dictionnaire de données | 2 | 7 entités, dictionnaire exhaustif avec RGPD | — |
| C6 | Tableau des endpoints REST (§8) couvrant tous les modules | 1 | Tableau complet mais l'endpoint `POST /api/v1/events/:id/media` (upload image) est absent | Ajouter l'endpoint d'upload S3 dans le tableau §8 |
| C7 | Analyse STRIDE sur 2 flux critiques | 2 | STRIDE complet sur Inscription et Webhook Stripe | — |

**Total : 13 / 14**

---

## Catégorie 3 — Cohérence

| N° | Critère | Score | Commentaire | Action recommandée |
|---|---|---|---|---|
| CO1 | Nommage des entités cohérent entre §6.4, §7 et §8 | 2 | `Ticket` utilisé partout de manière cohérente (jamais "Reservation" ou "Booking") | — |
| CO2 | Modules cités en §10 existent bien en §7 | 1 | `EventModule`, `NotificationModule`, `UserModule` cités en §10 mais seulement résumés en §7.4-7.6 | Compléter les fiches §7.4 à §7.6 en TP 1.8 |
| CO3 | ADR cités dans la matrice §10 existent dans /docs/adr/ | 2 | ADR-001, ADR-002, ADR-003 tous présents et référencés | — |
| CO4 | Codes HTTP dans §7 cohérents avec ceux annoncés en §8 | 2 | Vérification effectuée — cohérents | — |
| CO5 | Les événements RabbitMQ publiés en §7 documentés en §8 | 2 | `ticket.confirmed` et `payment.failed` documentés avec schéma JSON | — |
| CO6 | Les entités du dictionnaire §6.4 utilisées dans les payloads §8 | 2 | Tous les `uuid` référencés dans les schémas JSON correspondent aux entités du dictionnaire | — |

**Total : 11 / 12**

---

## Catégorie 4 — Lisibilité

| N° | Critère | Score | Commentaire | Action recommandée |
|---|---|---|---|---|
| L1 | Phrases courtes (< 25 mots) dans les paragraphes critiques | 1 | Quelques phrases longues identifiées dans §9.1 (STRIDE) et §7.3 | Raccourcir les phrases > 25 mots dans §9.1.1 et §7.3 |
| L2 | Paragraphes introductifs présents sur chaque diagramme | 2 | Intro + lecture présents sur tous les diagrammes | — |
| L3 | Voix active utilisée (pas "il est recommandé de" mais "nous recommandons") | 1 | Quelques tournures passives résiduelles dans §9.3 | Passer les tournures passives en voix active dans §9.3 |
| L4 | Mots-clés en gras sur les points d'attention importants | 2 | Bonne utilisation du gras dans les pièges et décisions | — |
| L5 | Aucune section avec seulement un commentaire TODO | 2 | Toutes les sections majeures sont complètes | — |
| L6 | Terminologie cohérente avec le glossaire §3 | 2 | Tous les termes du glossaire utilisés de manière cohérente | — |

**Total : 10 / 12**

---

## Catégorie 5 — Maintenabilité

| N° | Critère | Score | Commentaire | Action recommandée |
|---|---|---|---|---|
| M1 | Liens internes fonctionnels (entre sections) | 1 | Quelques liens du README vers les fichiers .md à vérifier | Tester tous les liens internes avec markdown-link-check |
| M2 | Métadonnée "dernière mise à jour" dans chaque fichier | 2 | Présente en en-tête de chaque section | — |
| M3 | CODEOWNERS présent avec assignation par section | 2 | Fichier CODEOWNERS complet | — |
| M4 | Workflow CI opérationnel (lint, liens, Mermaid) | 1 | Workflow créé mais pas encore validé en PR de test | Créer PR de test et documenter dans ci-validation.md |
| M5 | Historique des révisions mis à jour à chaque TP | 2 | Tableau complet dans §2 | — |

**Total : 8 / 10**

---

## Synthèse

**Total général : 53 / 60 (88 %)**

### Top 5 des défauts les plus impactants

1. **CO2 — Modules §7 incomplets (§7.4 à §7.6)** — Impact : un développeur assigné à EventModule, NotificationModule ou UserModule ne peut pas démarrer l'implémentation depuis la DCT seul. Priorité : élevée. Prévu en TP 1.8.

2. **M1 — Liens internes à vérifier** — Impact : un lecteur qui suit un lien mort perd confiance dans le document entier. Coût de correction : faible (vérification automatisée). Priorité : élevée, à corriger en Phase B.

3. **C6 — Endpoint upload S3 absent du tableau §8** — Impact : le développeur qui implémente l'upload de couverture d'événement n'a pas de contrat de référence. Correction rapide. Priorité : moyenne.

4. **L1/L3 — Phrases longues et voix passives dans §9** — Impact : réduction de la lisibilité sur des sections déjà denses (STRIDE). Correction mécanique. Priorité : faible.

5. **M4 — CI non encore validée en PR de test** — Impact : la valeur de la CI n'est pas prouvée tant qu'elle n'a pas rejeté au moins un défaut et été corrigée. Priorité : moyenne, à faire aujourd'hui.

### Plan de correction (Phase B — priorité et traçage)

| Priorité | Défaut | Impact | Coût | Action |
|---|---|---|---|---|
| 1 (haute/faible coût) | M1 — Liens internes | Élevé | Faible | `fix(maintenabilite): verify and fix internal links` |
| 2 (haute/faible coût) | C6 — Endpoint upload S3 | Moyen | Faible | `fix(completude): add S3 upload endpoint to §8 table` |
| 3 (moyenne/faible coût) | L1 — Phrases longues §9 | Moyen | Faible | `fix(lisibilite): shorten sentences in STRIDE section §9.1` |
| 4 (moyenne/faible coût) | L3 — Voix passives §9.3 | Faible | Faible | `fix(lisibilite): replace passive voice in §9.3 RGPD` |
| 5 (haute/moyen coût) | M4 — CI non validée | Élevé | Moyen | `chore(ci): create test PR to validate docs-quality workflow` |
| 6 (haute/élevé coût) | CO2 — §7 incomplets | Élevé | Élevé | Traiter en TP 1.8 — dette acceptée documentée |
