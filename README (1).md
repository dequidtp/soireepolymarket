# La Bourse des Potes

Prédiction market entre amis, jetons fictifs. Fichier unique (`index.html`) + backend Supabase, pas de build.

## Déploiement (identique à tes autres projets GitHub Pages)

1. Crée un projet sur [supabase.com](https://supabase.com) (gratuit).
2. Dans **SQL Editor**, colle et exécute le contenu de `schema.sql`.
3. Dans **Project Settings → API**, récupère :
   - `Project URL`
   - la clé `anon public`
4. Ouvre `index.html`, tout en haut du `<script>`, remplace :
   ```js
   const SUPABASE_URL = "REPLACE_WITH_SUPABASE_URL";
   const SUPABASE_ANON_KEY = "REPLACE_WITH_SUPABASE_ANON_KEY";
   ```
5. Pousse le repo sur GitHub, active GitHub Pages sur la branche/dossier concerné. C'est en ligne.

Pas d'authentification : chacun choisit/crée son pseudo au premier chargement (stocké dans `localStorage` du navigateur, comme le sélecteur de personne dans vivatech-tracker). Le solde de départ est fixé à 1000 jetons (`STARTING_BALANCE` en haut du script).

## Règles implémentées

- **Création d'un marché** : n'importe qui décrit une proposition, choisit son camp, fixe une **cote** et une **mise**. La mise du preneur est calculée automatiquement : `mise_preneur = mise_créateur / (cote - 1)`, pour que le pot se répartisse correctement selon la cote annoncée.
- **Prise de pari** : un autre joueur clique "Prendre ce pari" → le camp adverse et la mise correspondante sont engagés (gelés, pas encore débités).
- **Droit de rétractation** : le proposeur a 24h pour valider ou refuser. **Sans réponse, le pari est automatiquement validé** (hypothèse par défaut — facile à inverser dans `autoValidateExpired()` si tu préfères l'inverse).
- **Résolution** : n'importe quel des deux parieurs peut "Demander la résolution". Trois jurés sont **tirés au sort parmi les participants qui ne sont ni proposeur ni preneur**. Dès que 2 des 3 jurés votent dans le même sens, le pari est tranché et les jetons sont transférés automatiquement.
- **Classement** en temps quasi réel (rafraîchissement toutes les 15s).

## Limites volontaires du MVP

- Pas d'authentification réelle — repose sur la confiance "entre potes" (RLS Supabase ouverte).
- Un seul preneur par marché (pas de carnet d'ordres multi-preneurs type vrai Polymarket).
- Pas de notifications — chacun doit revenir sur la page pour voir si son pari a été pris / doit être jugé.

Si tu veux évoluer vers un vrai carnet d'ordres avec plusieurs preneurs par marché et un prix qui bouge (AMM), c'est une V2 différente — dis-le moi si ça t'intéresse.
