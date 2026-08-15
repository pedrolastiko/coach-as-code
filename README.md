# Coach as Code — Plan Marathon

Application web installable (PWA) qui affiche le plan d'entraînement hebdomadaire pour le **Marathon du P'tit Train du Nord** (04/10/2026, objectif 3h55–3h59). C'est une page unique, sans backend ni build, pensée pour être consultée depuis un téléphone, y compris hors connexion.

**Démo en ligne :** `https://pedrolastiko.github.io/coach-as-code/` (une fois GitHub Pages activé, voir plus bas).

## Fonctionnalités

- **16 semaines de plan** (S1 à S16), regroupées par semaine, dépliables au clic.
- Chaque séance affiche : type d'effort (EF, seuil, fractionné, sortie longue…), statut (réalisée / à venir / substituée), distance, météo, répartition des segments, métriques (FC, cadence, D+…) et un commentaire d'analyse.
- Bouton **« Tout déplier / Tout réduire »** pour parcourir rapidement l'ensemble du plan.
- Interface responsive, optimisée mobile.
- **Fonctionne hors ligne** grâce à un service worker qui met en cache l'application après la première visite.
- **Installable** sur l'écran d'accueil (Android et iOS) comme une vraie app, sans passer par un store.

## Structure du projet

```
.
├── index.html          # Application complète (structure, style, données du plan, logique d'affichage)
├── manifest.json        # Manifeste PWA (nom, icônes, couleurs, mode standalone)
├── service-worker.js    # Mise en cache des fichiers pour le mode hors ligne
└── icons/
    ├── icon-192.png
    ├── icon-512.png
    └── icon-512-maskable.png
```

Aucune dépendance, aucun build : tout est en HTML/CSS/JS natif dans `index.html`.

## Déployer sur GitHub Pages

1. Sur le dépôt GitHub, aller dans **Settings → Pages**.
2. Sous **Build and deployment**, choisir **Source : Deploy from a branch**.
3. Sélectionner la branche `main` et le dossier **/ (root)**, puis **Save**.
4. Après une à deux minutes, l'app est disponible à l'adresse :
   `https://<ton-pseudo-github>.github.io/coach-as-code/`

Aucune action GitHub Actions n'est nécessaire : la page est statique et servie telle quelle.

## Installer l'app sur son téléphone

Une fois la page publiée, ouvre l'URL depuis le navigateur de ton téléphone puis :

**Android (Chrome) :**
Menu ⋮ → **« Installer l'application »** (ou **« Ajouter à l'écran d'accueil »**).

**iPhone / iPad (Safari) :**
Bouton **Partager** (icône carrée avec une flèche) → **« Sur l'écran d'accueil »**.

L'icône apparaît alors comme une app native, s'ouvre en plein écran (sans barre d'adresse) et reste consultable même sans réseau grâce au cache du service worker.

> ⚠️ Le PWA doit être servi en HTTPS pour être installable — c'est le cas par défaut avec GitHub Pages.

## Développement / test en local

Comme un service worker exige un serveur (pas de `file://`), lance un petit serveur local à la racine du projet :

```bash
python3 -m http.server 8080
# puis ouvrir http://localhost:8080
```

## Mettre à jour le plan

Les données de chaque semaine et séance sont définies directement dans le tableau `weeks` en JavaScript, dans `index.html`. Pour ajouter une séance réalisée ou ajuster une semaine à venir, il suffit d'éditer les entrées correspondantes (statut `done`/`todo`/`warn`, distance, métriques, commentaire) puis de committer/pousser — GitHub Pages republie automatiquement le changement.

Pense à incrémenter `CACHE_NAME` dans `service-worker.js` (ex. `plan-marathon-v2`) après une mise à jour du contenu, pour forcer le rafraîchissement du cache sur les appareils qui ont déjà installé l'app.

## Licence

MIT — voir [LICENSE](LICENSE).
