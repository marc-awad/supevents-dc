# Review TP 1.3 — Revue croisée des diagrammes d'architecture

**Groupe relu :** Marc AWAD & Killian DOIZELET  
**Groupe relecteur :** Même groupe (auto-revue)  
**Date :** 30/04/2026

---

## Grille de revue

| Point de contrôle | Statut | Commentaire |
|---|---|---|
| Les diagrammes se rendent sans erreur Mermaid | ✅ OK | Tous les diagrammes ont été vérifiés sur mermaid.live avant intégration |
| Le diagramme Context est cohérent avec le CDC (pas d'acteur inventé, pas de système tiers oublié) | ✅ OK | 3 acteurs + 4 systèmes tiers présents, correspondant exactement au CDC |
| Le diagramme Containers indique technologie ET protocole/port sur chaque lien | ✅ OK | Tous les liens portent protocole ET port (HTTPS:443, AMQP:5672, TCP:5432, Redis:6379) |
| Les diagrammes de séquence couvrent le parcours complet sans raccourci ni "…" | ✅ OK | 9 étapes documentées dans le parcours nominal, alt/opt utilisés |
| La terminologie est cohérente avec le glossaire rédigé en TP 1.2 | ✅ OK | "Ticket", "PaymentModule", "SSO" — tous présents dans le glossaire §3 |

---

## Commentaires détaillés

**Point positif :** L'utilisation des fragments `alt` et `opt` dans le diagramme de séquence du parcours nominal est correcte — le fragment `opt` pour la liste d'attente est bien conditionnel (seulement si `remaining_spots = 0`), et le fragment `alt` distingue clairement succès et échec du paiement.

**Point à améliorer :** Dans le diagramme C4 Containers, le flux entre `Frontend` et `API Gateway` indique `HTTPS :443` mais la communication interne entre `API Gateway` et `Backend` devrait préciser `HTTP :3000` (communication interne non chiffrée dans le cluster). **Correction appliquée dans la version finale.**

**Point à améliorer :** Le diagramme de composants B.1 ne montre pas les dépendances de `UserModule` vers `AuthModule`. Après réflexion, cette dépendance existe (`AuthModule` appelle `UserModule` pour l'upsert au premier login). **Lien ajouté dans la version finale.**
