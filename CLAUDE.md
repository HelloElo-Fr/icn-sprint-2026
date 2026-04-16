# Mémoire Claude — Projet ICN Sprint 2026

> Ce fichier sert de mémoire partagée entre Elodie et Claude. À lire en tout
> début de session pour savoir où on en est.

## 👤 Contexte humain

- **Utilisatrice** : Elodie Hughes (`HelloElo-Fr`) — **non-dev**. Besoin
  d'explications simples, en français, sans jargon. Pas à l'aise avec le
  Terminal mais sait copier-coller des commandes.
- **Projet** : app web pour le séminaire ICN Creativ' Day (28 mars 2026).
  L'événement est passé.
- **Sauvegarde privée** sur son Mac : `~/Documents/ICN_Backup_2026-04-16/`
  (contient le bundle git complet + xlsx + app complète).

## 🎯 Objectif en cours : sécuriser le repo sans rien perdre

Audit fait le 15 avril 2026. Détails techniques dans `SECURITY.md`.
Findings principaux :
1. Données nominatives des 187 participants exposées (xlsx + code)
2. Clé Gemini fuitée dans l'historique
3. Mode admin sans auth
4. Quelques XSS dans le code

## ✅ Avancement — checklist

- [x] Audit de sécurité complet (commit `da3015b` sur `claude/security-audit-ePszj`)
- [x] Fichier xlsx supprimé du HEAD (encore dans l'historique)
- [x] `.gitignore` + `SECURITY.md` ajoutés
- [x] **Backup complet sur Mac** (`~/Documents/ICN_Backup_2026-04-16/`)
- [x] **Clé Gemini révoquée** dans Google Cloud (fait le 16 avril 2026)
- [ ] **Créer le repo privé** `HelloElo-Fr/icn-sprint-2026-private` (en cours)
- [ ] **Exporter les réponses Firebase** (projet `ai4leaders-3e524`)
- [ ] **Durcir les règles Firebase** (`.read=false`, `.write=false`)
- [ ] **Purger l'historique** du repo public (à faire par Claude une fois
      les 3 étapes ci-dessus validées)

## 🚦 Prochaine action

**Créer le repo privé** : Elodie doit aller sur https://github.com/new,
créer `icn-sprint-2026-private` en **Private**, sans init. Puis dans le
Terminal sur son Mac :

```bash
cd ~/Documents/ICN_Backup_2026-04-16/icn-sprint-2026
git remote add private https://github.com/HelloElo-Fr/icn-sprint-2026-private.git
git push private --all && git push private --tags
```

## 🧰 Pour Claude — rappels techniques

- Toujours travailler sur la branche `claude/security-audit-ePszj` (pas sur `main`).
- Ne **jamais** committer de données nominatives ni de secrets.
- La clé Gemini `AIzaSyDdpdQUBhFYXXHzQtRaEbgQtfd_d7ssi4o` est **révoquée**
  (safe de la laisser en historique puisqu'elle est morte, mais on purgera
  quand même par propreté).
- Ne **jamais** lancer `git push --force` sur `main` sans confirmation
  explicite « purge OK » d'Elodie.
- Le backup Mac (`~/Documents/ICN_Backup_2026-04-16/`) est le filet de
  sécurité : tant qu'il est intact, toutes les actions publiques sont
  réversibles.

## 🧭 Commandes utiles que connaît déjà Elodie

Elle sait utiliser :
- Ouvrir le Terminal
- Copier-coller des commandes une par une
- Comprendre le retour de `ls -lh`
- `git clone`, `cd`, `git push` (avec login interactif)
