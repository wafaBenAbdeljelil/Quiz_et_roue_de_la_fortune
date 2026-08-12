# Guide d'utilisation — Quiz et roue de la fortune

Guide destiné à l'équipe du CRAAQ. Aucune compétence technique n'est requise.

## 1. Lancer l'application

Ouvrez le fichier **`quiz-et-roue-de-la-fortune.html`** (double-clic). Il s'ouvre dans votre
navigateur et fonctionne **sans connexion Internet**.

Pour le kiosque : appuyez sur **F11** (Windows) ou **Ctrl + Cmd + F** (Mac) pour le plein écran.

## 2. Avant l'événement : la configuration

Cliquez sur **Admin**, en bas à droite. Mot de passe initial : `craaq2026`
— **changez-le dès la première utilisation** (onglet Réglages).

L'onglet **Réglages** permet de tout configurer :

| Section | Ce que vous pouvez faire |
|---|---|
| Texte d'accueil et seuil | Modifier le titre, le texte et le pourcentage requis pour débloquer la roue |
| Questions du quiz | Ajouter, modifier, réordonner ou supprimer des questions |
| Roue de la fortune | Définir les lots, leur nombre et leur quantité |
| Formulaire de captation | Choisir les champs demandés aux participants |
| Mot de passe | Changer le mot de passe administrateur |
| Données | Exporter ou importer la configuration, effacer les prospects |

### Le jeton `{seuil}`

Dans le texte d'accueil, écrivez **`{seuil}`** à l'endroit où le pourcentage doit apparaître.
Il se met à jour automatiquement quand vous changez le seuil : vous n'avez jamais à modifier
le texte pour corriger un pourcentage.

### Les questions

Pour chaque question : saisissez l'énoncé, puis les options. **Cochez le cercle de la bonne
réponse.** Chaque option possède son propre texte d'explication, affiché au participant après
son choix : profitez-en pour expliquer aussi pourquoi une réponse est fausse.

### La roue

Pour chaque lot, indiquez une **quantité** (par exemple 2 exemplaires) ou cochez **Illimité**.

> **Règle importante :** au moins un lot doit être **illimité**. Sans cela, l'enregistrement
> est refusé, car la roue pourrait se retrouver sans aucun lot disponible.

Quand un lot à quantité limitée est épuisé, il disparaît automatiquement de la roue.
Le bouton **Réinitialiser les quantités** remet tous les compteurs à leur valeur de départ :
à utiliser avant chaque nouvel événement.

### Le formulaire

Deux versions sont configurables : une **si le quiz est réussi**, une **si le quiz est échoué**.
Types de champs disponibles : texte simple, courriel, liste déroulante, cases à cocher,
et case à cocher avec texte personnalisé (pour le consentement).

> Le champ **courriel** est obligatoire et ne peut être supprimé : il sert à reconnaître un
> participant déjà inscrit et à lui envoyer son prix.

## 3. Pendant l'événement

Le participant répond au quiz, découvre son résultat, tourne la roue s'il atteint le seuil,
puis laisse ses coordonnées. Le message final lui indique qu'il recevra son prix par courriel.

Une **seule participation par adresse courriel** est acceptée. Si quelqu'un tente de rejouer,
un message le lui indique et le lot est automatiquement remis en stock.

## 4. Après l'événement

1. Ouvrez **Admin > Prospects**.
2. Cliquez sur **Exporter en CSV (Excel)** : vous obtenez la liste complète avec les
   coordonnées, le consentement, le score, les réponses et le lot gagné.
3. Envoyez leurs prix aux gagnants à partir de cette liste.
4. Ne sollicitez que les personnes ayant **coché le consentement**.

## 5. Points de vigilance

- **Les données sont enregistrées sur l'appareil utilisé**, dans le navigateur. Utilisez le
  **même appareil et le même navigateur** pendant tout l'événement, et n'effacez pas les
  données de navigation avant d'avoir exporté le fichier CSV.
- **Exportez le CSV à la fin de chaque journée**, par précaution.
- Pour utiliser plusieurs tablettes : configurez-en une, puis **Exportez la configuration** et
  **Importez-la** sur les autres. Attention, les quantités limitées sont comptées séparément
  sur chaque appareil : réservez les lots à quantité limitée à un seul appareil.
- La protection par mot de passe décourage un accès opportuniste, mais ne protège pas contre
  une personne compétente ayant accès à l'appareil. **Ne laissez pas la tablette sans
  surveillance** et effacez les données après l'événement.

## 6. Effacer les données

Onglet **Réglages > Données**. Saisissez le mot de passe d'autorisation
**`suppressiondefinitive`**, puis confirmez. Seuls les prospects sont effacés :
votre configuration (questions, lots, formulaire) est conservée.
