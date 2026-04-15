# Sécurité — `icn-sprint-2026`

Ce document récapitule l'audit du 15 avril 2026 et la procédure à suivre pour
assainir le repo après la diffusion de données personnelles.

## TL;DR — actions urgentes

1. **Révoquer la clé Gemini** `AIzaSyDdpdQUBhFYXXHzQtRaEbgQtfd_d7ssi4o` dans
   Google Cloud Console (présente dans l'historique Git public).
2. **Purger l'historique Git** des données nominatives des 187 participants
   (procédure ci-dessous).
3. **Vérifier** que le Cloudflare Worker `icn-chatbot-proxy.elodie-61f.workers.dev`
   filtre par Origin/Referer et applique un rate-limit (sinon : relais ouvert
   sur votre quota Anthropic).
4. **Vérifier** les règles Firebase de `ai4leaders-3e524` (RTDB europe-west1) :
   sans Firebase Auth, les modes `?admin=1` / `?presenter=1` / `?fac=...` ne
   protègent rien.

## Données concernées par la purge

Les éléments suivants contenaient ou contiennent encore les nom/prénom/campus
des 187 participants ICN :

- `ICN_Sprint_Salles_28mars.xlsx` — supprimé du HEAD, **encore dans
  l'historique**.
- `index.html` — constantes `TEAMS` et `FAC_MAP` présentes dans tous les
  commits jusqu'à `331471b` (« Replace app with THE END page only »).

Le commit `331471b` n'a fait que masquer le contenu côté client : la version
complète reste accessible via `git show 3dae52c:index.html` ou en chargeant
le site avec `?admin=1` / `?presenter=1` / `?fac=...` tant qu'elle est
déployée.

## Procédure de purge de l'historique

> ⚠️ Opération destructive. Prévenir les éventuels collaborateurs avant le
> force-push : leurs clones devront être ré-clonés.

```bash
# 1. Sauvegarder l'état actuel
git clone --mirror https://github.com/HelloElo-Fr/icn-sprint-2026.git \
  icn-sprint-2026-backup.git

# 2. Cloner un mirror propre pour la réécriture
git clone --mirror https://github.com/HelloElo-Fr/icn-sprint-2026.git
cd icn-sprint-2026.git

# 3. Installer git-filter-repo (https://github.com/newren/git-filter-repo)
#    pip install git-filter-repo

# 4. Supprimer le fichier xlsx de toute l'histoire
git filter-repo --invert-paths --path ICN_Sprint_Salles_28mars.xlsx

# 5. Pour purger les noms dans index.html, le plus simple est de réécrire
#    chaque commit qui contient TEAMS / FAC_MAP avec une version anonymisée.
#    Option radicale : squash de tout l'historique en un seul commit propre :
#
#    cd ../icn-sprint-2026
#    git checkout --orphan clean-main
#    git add -A
#    git commit -m "Initial commit (history purged 2026-04)"
#    git branch -D main
#    git branch -m main
#    git push -f origin main

# 6. Force-push du mirror nettoyé
git push --force --all
git push --force --tags

# 7. Demander à GitHub d'invalider les caches/forks :
#    https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository
#    Ouvrir un ticket au support GitHub pour purger les références cache.

# 8. Révoquer tout secret qui aurait existé dans l'ancien historique
#    (clé Gemini, tokens, etc.) — l'historique reste disponible chez quiconque
#    a cloné le repo avant la purge.
```

## Correctifs applicatifs à appliquer avant tout futur événement

1. **XSS wordcloud** — dans `renderWordCloud`, échapper la variable `w`
   (le code utilise déjà `escHtml` dans `renderOpenText`, l'appliquer
   également ici).
2. **Sortie LLM** — dans `askLLM`/`sendChat`, échapper le texte renvoyé par
   l'API avant la transformation Markdown→HTML, ou n'autoriser qu'une
   liste blanche de balises.
3. **Auth admin/fac/presenter** — passer derrière Firebase Auth (compte
   dédié) et règles serveur `auth.token.admin == true`. Le filtre par
   query-string n'offre aucune sécurité.
4. **CSP** — ajouter dans `index.html` :
   ```html
   <meta http-equiv="Content-Security-Policy" content="
     default-src 'self';
     script-src 'self' https://www.gstatic.com/firebasejs/;
     connect-src 'self' https://*.firebaseio.com https://*.firebasedatabase.app
                 https://icn-chatbot-proxy.elodie-61f.workers.dev;
     style-src 'self' 'unsafe-inline';
     img-src 'self' data:;
   ">
   ```
5. **SRI** — ajouter `integrity="sha384-..."` aux scripts Firebase chargés
   depuis `gstatic.com`.
6. **Worker Cloudflare** — vérifier que le proxy refuse les requêtes dont
   `Origin` n'est pas `https://helloelo-fr.github.io`, et ajouter un
   rate-limit (par IP).

## Signaler une vulnérabilité

Contacter Elodie Hughes en privé. Ne pas ouvrir d'issue publique pour les
problèmes touchant aux données personnelles ou aux secrets.
