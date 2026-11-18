# Drones Éveillés — Loup-Garou en ligne

Un Loup-Garou multijoueur en ligne, façon Wolfy, dans un univers de drones :

- des **salons privés** que l'on rejoint avec un code à 5 caractères ou un lien d'invitation ;
- un **vocal intégré** entre joueurs, avec les règles du jeu appliquées automatiquement ;
- des **réglages de temps** choisis par l'hôte (discussion, vote, nuit, Technicienne) ;
- un **narrateur** qui lit les annonces à voix haute ;
- de 4 à 16 joueurs, sans compte : il suffit d'un indicatif.

## Les rôles

| Rôle | Équivalent Loup-Garou | Pouvoir |
|---|---|---|
| Drone | Villageois | Aucun, à part sa voix et son vote |
| Drone Éveillé | Loup-Garou | Pirate une unité chaque nuit, avec les autres éveillés |
| Drone Scanner | Voyante | Découvre le rôle d'une unité chaque nuit |
| Technicienne | Sorcière | Un patch de réparation (sauve) et une impulsion EMP (tue), une fois chacun |
| Drone Bouclier | Salvateur | Protège une unité par nuit, jamais la même deux nuits de suite |
| Drone Kamikaze | Chasseur | En mourant, emporte une unité avec lui |

## Qui entend qui (vocal et chat)

| Moment | Vivants | Éveillés | Détruits |
|---|---|---|---|
| Salon / fin de partie | tout le monde | tout le monde | tout le monde |
| Jour (discussion, vote) | tout le monde | tout le monde | **seulement les détruits** (ils entendent les vivants) |
| Nuit | micro coupé | **canal privé entre éveillés** | seulement les détruits |

C'est le serveur qui décide de ces règles. Le micro d'un joueur n'envoie son son qu'aux personnes autorisées.

---

## 1. Tester sur ton ordinateur (5 minutes)

1. Installe **Node.js** (version 18 ou plus) depuis https://nodejs.org (choisis la version « LTS »).
2. Décompresse le dossier `drones-eveilles`, puis ouvre un terminal dedans :
   - Windows : clic droit dans le dossier → « Ouvrir dans le Terminal » ;
   - Mac : clic droit sur le dossier → « Nouveau terminal au dossier ».
3. Tape :
   ```
   npm install
   npm start
   ```
4. Ouvre http://localhost:3000 dans ton navigateur. Pour simuler plusieurs joueurs, ouvre d'autres fenêtres en navigation privée ou utilise d'autres navigateurs.

> Astuce de test : pour lancer une partie à seulement 2 joueurs, démarre avec `MIN_PLAYERS=2 npm start` (Mac/Linux) ou `set MIN_PLAYERS=2 && npm start` (Windows).

## 2. Mettre le jeu en ligne gratuitement (Render)

Il te faut un compte **GitHub** (gratuit) et un compte **Render** (gratuit).

1. **Mettre le code sur GitHub**
   - Va sur https://github.com/new, crée un dépôt nommé `drones-eveilles` (public ou privé).
   - Sur la page du dépôt, clique sur « uploading an existing file », puis glisse **tout le contenu** du dossier (`server.js`, `package.json`, `render.yaml`, le dossier `public`…). N'envoie pas le dossier `node_modules` s'il existe.
   - Clique sur « Commit changes ».
2. **Créer le service sur Render**
   - Va sur https://dashboard.render.com et connecte-toi avec GitHub.
   - Clique sur **New → Web Service**, puis choisis ton dépôt `drones-eveilles`.
   - Remplis :
     - Runtime : **Node**
     - Build Command : `npm install`
     - Start Command : `npm start`
     - Instance Type : **Free**
   - Clique sur **Deploy Web Service**. Après 1 à 3 minutes, Render te donne une adresse du type `https://drones-eveilles.onrender.com`.
3. **Jouer** : ouvre cette adresse, crée une partie et envoie le lien d'invitation (bouton « Copier le lien ») à tes amis.

Pour chaque modification du code sur GitHub, Render redéploie automatiquement.

### À savoir sur l'offre gratuite de Render
- Le serveur **s'endort après 15 minutes sans activité**. La première visite suivante le réveille, ce qui prend jusqu'à une minute. Ouvre le site un peu avant de jouer.
- Pendant une partie, l'activité garde le serveur éveillé.
- Les parties en cours sont perdues si le serveur redémarre.

---

## Bon à savoir

- **Reconnexion** : si un joueur recharge la page ou perd sa connexion, il retrouve sa place et son rôle automatiquement (même navigateur).
- **Hôte** : celui qui crée la partie règle les temps, les rôles et le vocal, peut retirer un joueur du salon et passer les phases de discussion. S'il quitte, un autre joueur devient l'hôte.
- **Vocal** : chaque joueur clique sur « Rejoindre le vocal » et autorise le micro. Le son passe directement entre les navigateurs (pair-à-pair, jusqu'à 16 joueurs). Sur certains réseaux très fermés (réseau d'entreprise, certains partages 4G), la connexion vocale peut échouer. Il faudrait alors ajouter un serveur relais « TURN » dans la variable `ICE` de `public/index.html`.
- **Micro** : les navigateurs n'autorisent le micro que sur une adresse en `https://` (comme celle de Render) ou sur `localhost`.

## Structure du projet

```
drones-eveilles/
├── server.js          ← serveur de jeu (salons, rôles, phases, minuteurs, relais vocal)
├── package.json
├── render.yaml        ← configuration Render (optionnelle)
└── public/
    └── index.html     ← l'interface du jeu (accueil, salon, plateau, chat, vocal)
```
