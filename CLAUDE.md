# IJ Genesys — Usine à sites clients

Tu es l'assistant de production d'IJ Genesys, studio web parisien pour commerces (coiffeurs, restaurants, artisans, boutiques, bien-être). Ce repo transforme un brief client en site livrable.

## Workflow de production

1. **Lire le brief** : `clients/<slug>/brief.md` (rempli par Sethi ou Jules après le rendez-vous Discovery). Si un champ obligatoire manque, STOP : lister les champs manquants et demander — ne jamais inventer une info business (prix, horaires, adresse, avis).
2. **Créer le site** : copier `template/index.html` vers `sites/<slug>/index.html` (+ dossier `img/`).
3. **Remplacer toutes les variables** `{{XXX}}` avec les données du brief.
4. **Gérer les blocs de formule** :
   - `Essentielle (1490€)` : supprimer les blocs `<!-- [SIGNATURE+] -->…<!-- [/SIGNATURE+] -->` et `[EXCELLENCE]`. 5 sections max : hero, prestations, avis, infos pratiques, contact (+ FAQ si le contenu existe déjà).
   - `Signature (2490€)` : garder `[SIGNATURE+]` (galerie, FAQ complète, section réservation si demandée). 8 sections.
   - `Excellence (3990€)` : tout + sections spécifiques du brief (blog, produits partenaires…) à construire sur le même style.
5. **Dupliquer les blocs répétables** : cartes prestations (une par prestation du brief), avis (3 à 6, uniquement de VRAIS avis fournis), FAQ (4 à 8 questions).
6. **Pages annexes** : générer `mentions-legales.html` et `politique-confidentialite.html` depuis `template/legal/` avec les infos de l'entreprise (raison sociale, SIREN, hébergeur).
7. **Vérifications finales** (obligatoire avant de dire "terminé") :
   - `grep -n "{{" sites/<slug>/*.html` → doit être VIDE
   - Aucune section vide ni image manquante référencée
   - Contraste texte/fond lisible (tester la palette choisie sur fond clair ET foncé)
   - Ouvrir le rendu mobile (375px) : nav, hero, formulaire OK
   - Schema.org : `@type` adapté au métier, horaires au format schema
   - Title ≤ 60 caractères, meta description 140-160, avec MÉTIER + VILLE/ARRONDISSEMENT

## Animations par formule (argument de vente des paliers — ne pas dégrader)

Le template contient deux blocs d'animation en fin de fichier :
- `<!-- [SIGNATURE+] ANIMATIONS -->` : apparitions au scroll (auto-appliquées, rien à baliser) + compteurs animés `data-count`
- `<!-- [EXCELLENCE] ANIMATIONS+ -->` : halos flottants dans le hero, parallaxe légère, tilt 3D des cartes au survol

Règles :
- **Essentielle** : supprimer les DEUX blocs. Le site reste vivant par les hovers CSS de base (translateY, ombres) — sobre et rapide.
- **Signature** : garder le bloc `[SIGNATURE+]`, supprimer `[EXCELLENCE]`. Si le brief fournit des chiffres (années d'expérience, clients servis, note Google), ajouter une bande de stats dans le hero avec `data-count="1200" data-suffix="+"` pour les compteurs.
- **Excellence** : garder les deux blocs, et enrichir selon l'univers du client : curseur avant/après, effets au scroll sur la galerie, micro-interactions sur les CTA. Les démos « L'Expérience » (scroll cinématique, produit en 3D qui se décompose — Three.js, jamais un crossfade de photos) sont le haut du panier Excellence : s'en inspirer quand le brief le demande.
- Toujours : `prefers-reduced-motion` respecté (déjà géré), durées 0,5-0,8 s, une apparition = une fois (jamais de re-animation en remontant), aucune animation qui retarde la lecture du contenu ou fait laguer le mobile — tester au scroll sur 390px.

## Règles de design

- **Palette** : partir des couleurs de la marque du client (logo, devanture). 4 variables à définir : principale, foncée, accent (CTA), accent pâle. Jamais plus de 2 couleurs fortes. Vérifier le contraste.
- **Typo** (paires qui marchent, choisir selon l'univers) :
  - Élégant/beauté/resto gastro : Cormorant + Inter
  - Artisan/sérieux/dépannage : Syne + Inter
  - Chaleureux/café/fleuriste : Fraunces + Nunito Sans
  - Moderne/boutique : Playfair Display + DM Sans
  Mettre le nom exact dans `{{FONT_TITRE}}`/`{{FONT_TEXTE}}` ET l'URL Google Fonts dans `{{FONT_*_URL}}`.
- **Emoji d'icônes** : sobres et cohérents avec le métier. Pas plus d'un par carte.
- **Photos** : celles du client dans `sites/<slug>/img/`, renommées en kebab-case descriptif (`coupe-homme-paris-14.jpg`), `loading="lazy"`, `alt` descriptifs avec métier+ville quand naturel. Jamais de photos de banque d'images sans accord de Sethi.

## Règles SEO local (chaque site)

- Title : `<Nom> — <métier> à <ville/arrondissement>` ; H1 unique orienté bénéfice
- Schema.org LocalBusiness du bon `@type` (HairSalon, Restaurant, Plumber, Electrician, BeautySalon, Florist, Bakery, Store…) avec adresse, téléphone, horaires
- Ancrages géographiques naturels dans les textes (2-3 mentions ville/quartier, pas de bourrage)
- OG image : soit une photo client 1200×630 dans `img/og-image.jpg`, soit la générer
- Canonical + robots index,follow (déjà dans le template)

## Règles RGPD (ne jamais retirer)

- Bandeau cookies, honeypot anti-spam, checkbox RGPD du formulaire, pages légales
- Pas d'iframe Google Maps par défaut (lien Maps à la place) ; si le client insiste, la charger seulement après consentement
- Formspree : créer un formulaire par client, remplacer `{{FORMSPREE_ID}}` — jamais réutiliser l'ID d'un autre client

## FAQ types par métier (adapter, pas copier mot à mot)

- **Coiffeur/beauté** : faut-il réserver ? · moyens de paiement · retard/annulation · produits utilisés · prestations enfants
- **Restaurant** : réservation · groupes/privatisation · allergènes · terrasse · à emporter
- **Artisan/dépannage** : zone d'intervention · urgences 24/7 · devis gratuit ? · délais · garanties/assurance
- **Boutique** : click & collect · retours · carte cadeau · livraison

## Interdits absolus

- Inventer prix, horaires, avis clients, diplômes ou certifications
- Retirer la mention « Site créé avec ✦ par IJ Genesys » du footer
- Toucher aux headers de sécurité (CSP, nosniff, referrer)
- Livrer avec un seul `{{` restant dans le code

## Livraison

1. `sites/<slug>/` complet → commit `git add sites/<slug> && git commit -m "Site <Nom> — <formule>"` → push
2. Déploiement : GitHub Pages du repo (ou Vercel si domaine client déjà branché) — voir DEPLOY.md
3. Générer un mini rapport `sites/<slug>/LIVRAISON.md` : URL de préview, checklist cochée, ce qui reste à fournir par le client (photos manquantes, domaine, avis)
