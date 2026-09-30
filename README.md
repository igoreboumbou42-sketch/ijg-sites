# IJ Genesys — Usine à sites

`template/` = squelette client (variables {{XXX}}, blocs par formule)
`brief/BRIEF-CLIENT.md` = questionnaire Discovery à copier dans `clients/<slug>/brief.md`
`exemples/brief-exemple.md` = brief fictif (Atelier Louise) pour tester la chaîne
`CLAUDE.md` = instructions lues automatiquement par Claude Code
`DEPLOY.md` = mise en ligne + checklist SEO

## Utilisation (nouveau client)
1. Discovery → remplir le brief dans `clients/<slug>/brief.md` + photos dans `clients/<slug>/photos/`
2. Ouvrir Claude Code à la racine du repo :
   « Produis le site du client <slug> à partir de son brief »
3. Vérifier la préview, ajuster, livrer (DEPLOY.md)

## Premier test
« Produis le site de test à partir de exemples/brief-exemple.md » — permet de valider
toute la chaîne sans client réel.
