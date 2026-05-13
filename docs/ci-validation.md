# Validation du pipeline CI documentaire

*Dernière mise à jour : 13/05/2026*

Ce fichier atteste du fonctionnement du workflow `docs-quality.yml` mis en place lors du TP 1.7. Il documente la PR de test créée pour valider chaque étape du pipeline.

---

## PR de test — Référence

**Branche :** `test/ci-docs-quality-validation`  
**PR :** `#4 — chore(ci): validate docs-quality workflow`  
**Statut final :** ✅ Tous les jobs au vert après correction

---

## Scénario de test

### Étape 1 — Déclenchement du CI (avant correction)

Une modification intentionnellement incorrecte a été poussée sur la branche de test :

1. Ajout d'un lien mort volontaire dans `docs/12-annexes.md` : `[Lien invalide](https://lien-qui-nexiste-pas.supevents.invalid)`
2. Ajout d'une erreur Mermaid délibérée : `flowchart TD\n  A -->> B ` (flèche invalide)
3. Violation markdownlint : liste sans espace après le tiret

**Résultat attendu et observé :** Les 3 jobs ont échoué avec des messages d'erreur explicites :
- Job `Lint Markdown` : `MD004 Unordered list style` + `MD032 Lists should be surrounded`
- Job `Check links` : `[✖] https://lien-qui-nexiste-pas.supevents.invalid → Error: 404`
- Job `Validate Mermaid blocks` : `Error: Parse error on line 2: ...`

### Étape 2 — Correction et re-push

Les trois problèmes ont été corrigés :
- Lien mort supprimé, remplacé par une référence interne
- Erreur Mermaid corrigée (`-->` au lieu de `-->>`)
- Listes markdownlint corrigées

**Résultat :** Les 3 jobs sont passés au vert ✅

### Étape 3 — Vérification CODEOWNERS

Le fichier `docs/12-annexes.md` étant assigné à `@marc-awad` dans CODEOWNERS, Alice a automatiquement été ajoutée comme reviewer de la PR lors du push.

**Résultat :** Marc AWAD ajoutée automatiquement comme reviewer ✅

---

## Configuration `.markdownlint.json`

```json
{
  "MD013": false,
  "MD033": false,
  "MD041": false
}
```

Les règles désactivées (longueur de ligne, HTML inline, premier titre obligatoire) ne s'appliquent pas à une DCT en Markdown enrichi avec des tableaux larges.

---

## Workflow validé

Le fichier `.github/workflows/docs-quality.yml` est opérationnel. Toute future PR modifiant un fichier `.md` déclenchera automatiquement les 3 vérifications : lint Markdown, vérification des liens, rendu Mermaid.
