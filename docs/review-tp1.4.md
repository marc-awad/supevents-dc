# Review TP 1.4 — Revue de cohérence §6.4 ↔ §8

**Groupe relu :** Marc AWAD & Killian DOIZELET  
**Groupe relecteur :** Auto-revue  
**Date :** 05/05/2026

---

## Checklist de revue

| Point de contrôle | Statut | Commentaire |
|---|---|---|
| Toutes les entités référencées dans les payloads d'événements existent dans le dictionnaire | ✅ OK | `ticket_id`, `user_id`, `event_id` dans les schémas JSON — tous présents dans §6.4 |
| Tous les `id` côté API sont du même type que les `id` côté base (UUID) | ✅ OK | Tous les schémas JSON utilisent `"format": "uuid"`, cohérent avec `UUID` en base |
| Chaque contrainte UNIQUE en base a un code 409 documenté côté API | ✅ OK | `TICKET_001` (409) pour la contrainte UNIQUE(user_id, event_id) |
| Les champs marqués sensibles RGPD ne fuitent pas dans les payloads d'événements | ✅ OK | `email`, `first_name`, `last_name` absents des schémas JSON `ticket.confirmed` et `payment.failed` |
| Le webhook Stripe est bien authentifié par HMAC, pas par JWT | ✅ OK | Colonne Auth = "HMAC (Stripe-Signature)" dans le tableau §8 |
| Chaque événement asynchrone a un consommateur identifié | ✅ OK | `ticket.confirmed` → NotificationModule ; `payment.failed` → NotificationModule |

---

## Commentaires

**Commentaire 1 (Baptiste) :** L'événement `event.cancelled` est mentionné dans §8 (tableau événements) mais son schéma JSON n'est pas documenté en détail — contrairement à `ticket.confirmed` et `payment.failed`. À corriger en TP 1.8 pour être complet.

**Commentaire 2 (Alice) :** Dans le dictionnaire §6.4, l'entité `Notification` a le champ `body` marqué RGPD=Oui. Il faudrait vérifier que ce champ n'est pas inclus dans les événements RabbitMQ publiés (qui pourraient être loggués). **Vérification effectuée : le corps de l'email est généré par NotificationModule depuis un template, il n'est pas dans le payload de l'événement RabbitMQ. OK.**
