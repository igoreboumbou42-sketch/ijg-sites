# Déploiement d'un site client

## Préview (systématique)
Le repo est en GitHub Pages : chaque site est visible à
`https://<compte>.github.io/<repo>/sites/<slug>/` dès le push.
→ C'est ce lien qu'on envoie au client pour validation des maquettes.

## Mise en ligne définitive (domaine client)
Option A — GitHub Pages + domaine (simple, gratuit) :
1. Créer un repo dédié `<slug>-site`, y copier `sites/<slug>/*`
2. Settings → Pages → branche main
3. Acheter le domaine (OVH/Gandi) → DNS : CNAME www → <compte>.github.io + A records GitHub Pages
4. Settings → Pages → Custom domain → cocher Enforce HTTPS

Option B — Vercel (si formulaire évolué ou besoin serveur plus tard) :
1. `vercel deploy sites/<slug>` ou repo dédié importé dans Vercel
2. Domaine ajouté dans le dashboard Vercel

## Après mise en ligne (checklist SEO — 10 min)
- [ ] Google Search Console : ajouter la propriété + demander l'indexation
- [ ] Créer/mettre à jour la fiche Google Business Profile du client (lien site)
- [ ] Tester le formulaire en réel (un envoi de bout en bout)
- [ ] Tester sur iPhone + Android
- [ ] Envoyer LIVRAISON.md au client avec la garantie 30 jours
