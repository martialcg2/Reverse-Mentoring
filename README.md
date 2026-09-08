# Frontend Cross-Mentoring Digital Banking

Interface **React (Vite)** câblée à l'API FastAPI. Écran de connexion réel (JWT), et **toutes les données viennent du serveur** — le scoring des compétences est calculé côté backend, jamais côté client.

## Prérequis

Le [backend](../cdc-backend) doit être déployé (ou lancé en local sur `http://localhost:8000`).

## Lancer en local

```bash
npm install
cp .env.example .env          # puis renseignez VITE_API_URL si besoin
npm run dev                   # http://localhost:5173
```

Par défaut, l'app appelle `http://localhost:8000`. Lancez le backend en parallèle, puis
connectez-vous avec un compte de démo (mot de passe `demo1234`) :

| Rôle | Email |
|---|---|
| RH / Admin | `rh@cdc.cg` |
| Mentor | `nadia@cdc.cg` |
| Mentorée | `sophie@cdc.cg` |

## Ce que fait chaque espace (données réelles du backend)

- **Mentoré** — profil et radar depuis `/me/competencies` ; le **diagnostic est posté** à `/me/diagnostic` (le serveur calcule les 8 niveaux) ; passeport.
- **Mentor** — `/mentor/mentees`, puis `/mentor/mentees/{id}/competencies` (le serveur **refuse 403** si le mentor n'est pas le binôme) ; playbook par niveau ; séance via `/mentor/sessions`.
- **RH** — tableau de bord, suivi, cartographie (agrégats, blocage sous n=5), matching à valider, **CRUD participants**, reporting téléchargeable. Aucune route ne renvoie de score individuel à la RH.

## Déploiement gratuit (Vercel ou Netlify)

1. Poussez ce dossier sur GitHub.
2. **Vercel** : *New Project* → importez le dépôt. Framework détecté : Vite. Ajoutez la variable d'environnement `VITE_API_URL` = l'URL publique de votre API (ex. `https://cross-mentoring-api.onrender.com`). Déployez.
   - *Netlify* : identique — build command `npm run build`, publish `dist`, variable `VITE_API_URL`.
3. **Côté backend**, autorisez l'origine du frontend : variable `CORS_ORIGINS` = l'URL Vercel/Netlify (ex. `https://mon-app.vercel.app`).

C'est tout : backend (Render + Neon) + frontend (Vercel) = plateforme en ligne, gratuite.

## Configuration

| Variable | Rôle | Défaut |
|---|---|---|
| `VITE_API_URL` | URL de l'API FastAPI | `http://localhost:8000` |

## Structure

```
src/
  main.jsx  App.jsx        # auth, shell, routage par rôle
  api.js                    # client HTTP + jeton JWT
  common.jsx                # constantes métier + composants UI (radar, cartes…)
  screens/
    Login.jsx
    Mentee.jsx  questions.js
    Mentor.jsx
    RH.jsx
```

## Note production

Le login local (email/mot de passe) est destiné à la démo / au pilote. En production,
il cède la place au **SSO d'entreprise + MFA** (le modèle de rôles ne change pas).
