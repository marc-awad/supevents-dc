# §4 — Contexte et objectifs

## Contexte technique

SupEvents naît d'un constat simple : les événements étudiants sont aujourd'hui organisés via des outils génériques (Google Forms, Eventbrite, virements bancaires informels) qui n'offrent ni intégration avec les systèmes de l'école ni gestion fiable des places et paiements. La plateforme doit combler ce vide en exposant un parcours d'inscription fluide, sécurisé et traçable.

Du point de vue du concepteur technique, les enjeux sont clairs : le système doit exposer un **parcours d'inscription en moins de 3 étapes avec authentification SSO** (pas de création de compte supplémentaire), supporter des **pics de charge prévisibles** (ouverture des inscriptions en début de semestre avec plusieurs centaines d'étudiants simultanés), et garantir la **cohérence transactionnelle** entre réservation de place et capture du paiement — opérations qui doivent être atomiques pour ne jamais débiter sans confirmer, ni confirmer sans débiter.

L'intégration Stripe impose une conformité **PCI-DSS** déléguée : aucune donnée de carte ne doit transiter par nos serveurs. L'intégration du SSO école impose de respecter le protocole **OIDC/OAuth2** et la fédération des identités. La présence d'étudiants en situation de handicap dans la population cible impose une conformité **WCAG 2.1 AA** sur le frontend.

La plateforme est construite **from scratch** sans migration de données existantes, ce qui libère des contraintes de compatibilité ascendante mais impose d'autant plus de rigueur sur les choix initiaux — ils seront difficiles à corriger une fois la production lancée.

---

## Objectifs techniques implicites

Les exigences suivantes ne sont pas formulées noir sur blanc dans le CDC mais découlent directement des besoins exprimés. Elles ont le même rang de priorité que les exigences explicites.

| Objectif technique | Origine dans le CDC | Niveau de priorité |
|---|---|---|
| Scalabilité horizontale du backend | "500 utilisateurs simultanés" | Élevé |
| Conformité PCI-DSS (déléguée à Stripe) | "Paiement en ligne par carte bancaire" | Élevé |
| Conformité WCAG 2.1 AA | "Accessible aux étudiants handicapés" | Élevé |
| Idempotence des opérations de paiement | "Aucune double facturation tolérée" | Élevé |
| Cohérence transactionnelle réservation-paiement | "L'inscription n'est effective qu'après paiement" | Élevé |
| Traçabilité des accès aux données personnelles | "Conformité RGPD" | Élevé |
| Découplage asynchrone pour les notifications | "Emails de confirmation envoyés après paiement" | Moyen |
| Disponibilité 99,5 % mensuelle | "Plateforme disponible pendant les périodes d'inscription" | Élevé |
| Authentification fédérée sans création de compte | "Les étudiants utilisent leurs identifiants ENT" | Élevé |
| Génération et validation de billets infalsifiables | "Contrôle d'accès à l'entrée des événements" | Moyen |
| Internationalisation (i18n) prête à l'emploi | "Étudiants internationaux dans la population cible" | Faible |
| Audit log des actions administrateur | "Traçabilité des décisions de modération" | Moyen |
