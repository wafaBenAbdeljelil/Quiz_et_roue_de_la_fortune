# Quiz et roue de la fortune — CRAAQ

Application autonome en un seul fichier : [`quiz-et-roue-de-la-fortune.html`](quiz-et-roue-de-la-fortune.html).
Aucune installation, aucune connexion requise — le fichier s'ouvre directement dans un navigateur.

- [`PRD.md`](PRD.md) — spécification fonctionnelle
- [`guide-utilisation.md`](guide-utilisation.md) — guide pour l'équipe

## Publication sur GitHub Pages

Le workflow [`.github/workflows/pages.yml`](.github/workflows/pages.yml) publie le site à chaque
push sur `main`. Il copie `quiz-et-roue-de-la-fortune.html` vers `index.html` au moment du
déploiement : le HTML n'existe donc qu'en un seul exemplaire dans le dépôt, et il n'y a jamais
deux versions à garder synchronisées.

Adresse publique une fois la publication activée :
`https://wafabenabdeljelil.github.io/Quiz_et_roue_de_la_fortune/`

### Activation, une seule fois

1. Le dépôt doit être **public** (Settings → General → Danger Zone → Change visibility).
2. Settings → Pages → **Source : GitHub Actions**.
3. Pousser sur `main`. L'onglet Actions montre le déploiement ; la page est en ligne au bout
   d'une à deux minutes.

## Limites à connaître avant de diffuser le lien

**Les données restent dans le navigateur du visiteur.** L'application n'a pas de serveur : la
configuration et les prospects sont écrits dans le `localStorage` du navigateur qui affiche la
page. Conséquences dès que le lien est partagé largement :

- les coordonnées saisies par un visiteur **n'apparaissent pas** dans l'onglet Admin des autres
  appareils — impossible de centraliser la collecte de prospects ;
- les quantités de lots sont décomptées **par appareil**, donc un lot limité à 2 exemplaires peut
  être gagné par un nombre illimité de personnes ;
- la règle « une participation par personne » ne s'applique qu'à l'échelle d'un navigateur.

L'application est conçue pour un usage **borne / kiosque** : un appareil tenu par l'équipe lors
d'un événement, sur lequel l'admin exporte ensuite les prospects en CSV. Pour une vraie collecte
en ligne, il faut un service externe de captation.

**Le mot de passe administrateur est lisible dans le code source** (`craaq2026` au premier
lancement, ainsi que le mot de passe fixe d'autorisation d'effacement). Sur un dépôt public,
n'importe qui peut donc ouvrir le panneau Admin et modifier questions, lots et formulaires sur
son propre appareil. Le changer depuis l'onglet Réglages ne protège que l'appareil courant : la
valeur initiale reste dans le fichier.
