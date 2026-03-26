# ICN Creativ Day — 28 mars 2026

App web pour le séminaire de transformation ICN Business School.
187 participants, 22 équipes, 5 villages.

## URL (ne jamais changer)

**https://helloelo-fr.github.io/icn-sprint-2026/**

## 3 modes d'accès

| Mode | URL | Usage |
|------|-----|-------|
| **Participant** | `https://helloelo-fr.github.io/icn-sprint-2026/` | Pour les 187 participants sur smartphone |
| **Admin** | `https://helloelo-fr.github.io/icn-sprint-2026/?admin=1` | Régie sondages : lancer/stopper questions, RAZ, export Excel |
| **Présentation** | `https://helloelo-fr.github.io/icn-sprint-2026/?presenter=1` | Grand écran : affiche les résultats en live |

## Sondages — Comment ça marche

### Avant le jour J
1. Ouvrir le mode **Admin** (`?admin=1`)
2. Cliquer **RAZ tout** pour partir de zéro

### Le jour J
1. Ouvrir **Admin** sur ton téléphone/ordi
2. Ouvrir **Présentation** sur l'ordi branché au vidéoprojecteur
3. Cliquer **Lancer** sur Q1 → la question apparaît sur tous les téléphones
4. Les réponses arrivent en temps réel sur l'écran de présentation
5. Cliquer **Stop** quand suffisamment de réponses
6. Cliquer **Lancer** sur Q2, puis Q3
7. **Exporter Excel** pour récupérer toutes les réponses

### Les 3 questions
- **Q1** (texte libre) : Qu'est-ce qui t'empêche de faire du bon travail à l'ICN ?
- **Q2** (nuage de mots) : Qu'est-ce qui freine le développement de l'ICN selon toi ?
- **Q3** (Change Journey Map) : Où en es-tu dans le voyage du changement ?

## Chatbot IA

Bouton rose "Où suis-je ?" en bas à droite.
- Tape un **nom** → trouve l'équipe, la salle, le village
- Pose une **question** → répond via Gemini 2.5 Flash (programme, méthode, logistique)
- Bilingue FR/EN selon le toggle

## Architecture technique

- **Frontend** : HTML/CSS/JS pur, fichier unique `index.html`, aucune dépendance npm
- **Backend temps réel** : Firebase Realtime Database (projet `ai4leaders-3e524`, région Europe West 1)
- **Chatbot LLM** : Google Gemini 2.5 Flash via API (clé restreinte par domaine)
- **Hébergement** : GitHub Pages (repo `HelloElo-Fr/icn-sprint-2026`)

## Localisation des fichiers

| Fichier | Emplacement |
|---------|-------------|
| Source HTML | `/Users/elodiehughes/Documents/Claude/Projects/ICN/ICN_Sprint_App.html` |
| Repo GitHub | `/tmp/icn-sprint-2026/` (copie de déploiement) |
| QR Code | `/Users/elodiehughes/Documents/Claude/Projects/ICN/QR_ICN_Creativ_Day_Orange.png` |

## Déploiement

```bash
cd /tmp/icn-sprint-2026
cp /Users/elodiehughes/Documents/Claude/Projects/ICN/ICN_Sprint_App.html index.html
git add index.html
git commit -m "Description du changement"
git push origin main
```

L'URL ne change jamais. GitHub Pages met à jour en 1-2 min.

## Sécurité

- Clé API Gemini restreinte au domaine `helloelo-fr.github.io` uniquement
- Clé API restreinte à l'API Generative Language uniquement
- Firebase : écriture seule pour les participants, lecture temps réel pour le présentateur
