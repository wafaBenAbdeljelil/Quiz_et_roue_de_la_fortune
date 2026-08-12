# PRD — Quiz et roue de la fortune (CRAAQ)

> **Statut : validé et implémenté** · Version 1.1 · Validé le 2026-08-11 · Dernière mise à jour : 2026-08-11
>
> Ce document est la **source de vérité unique** du produit. Toute décision, modification ou
> ajout est d'abord reflété ici, puis implémenté. Aucune fonctionnalité n'est codée sans
> figurer dans ce document.

---

## Table des matières

1. [Vision et objectif](#1-vision-et-objectif)
2. [Personas et parcours utilisateur](#2-personas-et-parcours-utilisateur)
3. [Périmètre fonctionnel](#3-périmètre-fonctionnel)
4. [Modèle de données](#4-modèle-de-données)
5. [Règles de gestion et de validation](#5-règles-de-gestion-et-de-validation)
6. [Critères d'acceptation](#6-critères-dacceptation)
7. [Contraintes techniques](#7-contraintes-techniques)
8. [Questions ouvertes et points à trancher](#8-questions-ouvertes-et-points-à-trancher)
9. [Hors périmètre](#9-hors-périmètre)
10. [Journal des décisions](#10-journal-des-décisions)

---

## 1. Vision et objectif

### 1.1 Vision

Offrir au CRAAQ une animation de kiosque **clé en main, autonome et entièrement configurable
sans développeur**, qui transforme un passage devant le kiosque en une conversation, puis en
un contact qualifié.

### 1.2 Problème résolu

Lors des événements, l'équipe du CRAAQ dispose de peu de temps pour engager les visiteurs et
capte trop peu de coordonnées exploitables. Une animation ludique crée un prétexte
d'engagement, tandis que le lot constitue une contrepartie légitime à la remise des
coordonnées et du consentement aux communications.

### 1.3 Objectifs mesurables

| # | Objectif | Indicateur |
|---|---|---|
| O1 | Capter des contacts qualifiés | Nombre de prospects enregistrés par événement |
| O2 | Obtenir un consentement valide aux communications | % de prospects ayant consenti |
| O3 | Faire découvrir l'offre du CRAAQ | % de participants terminant le quiz |
| O4 | Rendre l'outil autonome pour l'équipe marketing | Zéro intervention de développeur pour changer questions, lots ou formulaire |

### 1.4 Principes directeurs

- **Configurable avant tout** : aucun contenu métier (question, lot, champ, texte, seuil) n'est
  codé en dur. Tout est modifiable depuis l'interface d'administration.
- **Autonome** : fonctionne hors ligne, sans serveur ni connexion Internet.
- **Rapide** : un parcours complet doit prendre moins de deux minutes.
- **Conforme** : le consentement aux communications est explicite, jamais pré-coché.

---

## 2. Personas et parcours utilisateur

### 2.1 Personas

| Persona | Description | Besoins | Contexte d'usage |
|---|---|---|---|
| **Visiteur** (participant) | Producteur, conseiller, agronome, chercheur, étudiant ou relève, rencontré au kiosque | Jouer vite, comprendre ce qu'il gagne, ne pas remplir un formulaire interminable | Debout, sur tablette, en quelques minutes, environnement bruyant |
| **Ambassadeur** (équipe terrain) | Membre de l'équipe qui anime le kiosque | Relancer une partie rapidement, ne jamais rester bloqué | Debout, entre deux conversations |
| **Administrateur** (équipe marketing) | Responsable de l'animation et des données | Configurer le quiz et les lots avant l'événement, récupérer les prospects après | Assis, sur ordinateur, avant et après l'événement |

### 2.2 Parcours du visiteur

```
1. Accueil  ->  2. Quiz  ->  3. Résultat  ->  4. Roue (si réussite)  ->  5. Captation  ->  6. Confirmation
                                   |                                            ^
                                   +--------- si échec (sous le seuil) ---------+
```

1. **Accueil** — texte configurable, avec le seuil de réussite inséré automatiquement.
2. **Quiz** — questions à choix multiples, une à la fois. Après chaque réponse : indication
   bonne/mauvaise et affichage du **détail explicatif de l'option choisie**.
3. **Résultat** — score en nombre et en pourcentage. Détermine la suite du parcours.
4. **Roue de la fortune** — *uniquement si le score atteint le seuil*. Le visiteur tourne la
   roue et obtient un lot.
5. **Captation** — formulaire configurable. Deux variantes de champs et de textes : **réussite**
   (le visiteur a gagné un lot) ou **échec** (pas de lot).
6. **Confirmation** — message de remerciement, rappel du lot le cas échéant, puis retour à
   l'accueil pour le visiteur suivant.

> **Changement par rapport à l'existant** : dans la version actuelle, la captation a lieu
> *avant* la roue. Le présent PRD place la captation *après* la roue, conformément au parcours
> demandé. Implication traitée en §5.4 et en question ouverte Q1.

### 2.3 Parcours de l'administrateur

```
Lien « Admin »  ->  Mot de passe (masqué)  ->  Onglet « Prospects » : consulter, exporter
                                            ->  Onglet « Réglages »  : configurer, effacer, changer le mot de passe
```

---

## 3. Périmètre fonctionnel

Légende : **[E]** existant conservé · **[M]** existant modifié · **[N]** nouveau

### 3.1 Application visiteur

| Réf | Fonctionnalité | Statut |
|---|---|---|
| F1.1 | Écran d'accueil avec texte configurable | [M] |
| F1.2 | Insertion automatique du seuil de réussite dans le texte d'accueil | [N] |
| F1.3 | Quiz à choix multiples, une question par écran, barre de progression | [E] |
| F1.4 | Détail explicatif affiché après réponse, **propre à l'option choisie** | [M] |
| F1.5 | Écran de résultat (score et pourcentage) | [M] |
| F1.6 | Roue de la fortune animée, débloquée au-dessus du seuil | [E] |
| F1.7 | Mélange de la position des lots à chaque partie | [E] |
| F1.8 | Gestion du stock de lots (quantités limitées ou illimitées) | [N] |
| F1.9 | Formulaire de captation à champs configurables | [M] |
| F1.10 | Variantes du formulaire selon réussite ou échec | [N] |
| F1.11 | Détection d'une participation déjà enregistrée (même courriel) | [E] |
| F1.12 | Écran de confirmation et retour à l'accueil | [E] |

### 3.2 Interface d'administration

L'administration est réorganisée en **deux onglets**.

#### 3.2.1 Accès

| Réf | Fonctionnalité | Statut |
|---|---|---|
| F2.1 | Accès protégé par mot de passe | [E] |
| F2.2 | **Champ de saisie de type `password`** (caractères masqués) | [M] |
| F2.3 | Mot de passe administrateur modifiable depuis l'application | [N] |

#### 3.2.2 Onglet « Prospects »

| Réf | Fonctionnalité | Statut |
|---|---|---|
| F3.1 | Liste des prospects : date, réponses au formulaire, score, lot gagné, consentement | [E] |
| F3.2 | Colonnes dynamiques reflétant les champs configurés du formulaire | [M] |
| F3.3 | Consultation du détail des réponses au quiz | [E] |
| F3.4 | Export CSV compatible Excel | [E] |
| F3.5 | Bouton « Effacer les données » | [M] — **déplacé vers Réglages** |

#### 3.2.3 Onglet « Réglages »

| Réf | Fonctionnalité | Statut |
|---|---|---|
| F4.1 | Gestion des questions : ajouter, modifier, supprimer, réordonner | [N] |
| F4.2 | Édition d'une question : énoncé, options, détail par option, désignation de la bonne réponse | [N] |
| F4.3 | Modification du seuil de réussite | [N] |
| F4.4 | Configuration de la roue : lots, nombre, quantités (limitées ou illimitées) | [N] |
| F4.5 | Configuration du formulaire de captation (champs, types, obligation, variantes) | [N] |
| F4.6 | Modification du texte d'accueil du quiz | [N] |
| F4.7 | Changement du mot de passe administrateur | [N] |
| F4.8 | Effacement des données, protégé par mot de passe d'autorisation | [M] |
| F4.9 | Export et import de la configuration (recopie entre appareils) | [N] |

---

## 4. Modèle de données

Deux ensembles distincts sont persistés séparément : la **configuration** (ce que l'admin
paramètre) et les **prospects** (ce que les visiteurs génèrent).

### 4.1 Configuration

```jsonc
{
  "version": 1,
  "accueil": {
    "titre": "Testez vos connaissances!",
    "texte": "Répondez au quiz, puis tentez votre chance à la roue de la fortune. Obtenez {seuil} ou plus de bonnes réponses pour débloquer la roue!"
  },
  "quiz": {
    "seuilReussite": 50,
    "questions": [
      {
        "id": "q_a1b2c3",
        "ordre": 1,
        "enonce": "Depuis combien d'années le CRAAQ fait-il le pont entre la recherche et la pratique?",
        "options": [
          { "id": "o_1", "texte": "Environ 10 ans", "detail": "Pas tout à fait. Le CRAAQ agit depuis plus de 25 ans.", "correcte": false },
          { "id": "o_2", "texte": "Plus de 25 ans", "detail": "Bonne réponse! Organisme à but non lucratif, le CRAAQ réunit et diffuse les savoirs du secteur depuis plus de 25 ans.", "correcte": true },
          { "id": "o_3", "texte": "Depuis 2020", "detail": "Non. Le CRAAQ existe depuis bien plus longtemps.", "correcte": false }
        ]
      }
    ]
  },
  "roue": {
    "lots": [
      { "id": "l_x1", "libelle": "25% de rabais", "illimite": false, "quantiteInitiale": 2, "quantiteRestante": 2 },
      { "id": "l_x2", "libelle": "Cadeau surprise", "illimite": true, "quantiteInitiale": null, "quantiteRestante": null }
    ]
  },
  "formulaire": {
    "reussite": { "intro": "Entrez vos coordonnées pour recevoir votre prix.", "champs": [] },
    "echec": { "intro": "Entrez vos coordonnées pour rester informé des nouveautés du CRAAQ.", "champs": [] }
  },
  "admin": {
    "motDePasseHash": "<empreinte SHA-256 en hexadécimal>",
    "motDePasseSel": "<sel aléatoire>"
  }
}
```

### 4.2 Champ de formulaire

```jsonc
{
  "id": "c_courriel",
  "type": "courriel",
  "libelle": "Courriel",
  "aide": "",
  "obligatoire": true,
  "options": [],
  "texte": "",
  "ordre": 2
}
```

| Type | Rendu | `options` | `texte` | Notes |
|---|---|---|---|---|
| `texte` | Champ texte sur une ligne | — | — | Ex. nom, prénom, organisation |
| `courriel` | Champ courriel avec validation | — | — | **Exactement un par variante**, toujours obligatoire (R3.3) |
| `liste` | Liste déroulante, choix unique | requis (2 ou plus) | — | Ex. région, type de production |
| `cases` | Cases à cocher, choix multiple | requis (1 ou plus) | — | Ex. centres d'intérêt |
| `consentement` | Case à cocher unique avec texte personnalisé | — | requis | Jamais pré-cochée |

### 4.3 Prospect

```jsonc
{
  "id": "p_20260811_001",
  "horodatage": "2026-08-11T14:22:31.000Z",
  "variante": "reussite",
  "reponsesFormulaire": {
    "c_nom": "Marie Tremblay",
    "c_courriel": "marie@exemple.ca",
    "c_interets": ["Grandes cultures", "Gestion"],
    "c_consentement": true
  },
  "quiz": {
    "score": 5,
    "total": 6,
    "pourcentage": 83,
    "reussi": true,
    "reponses": [
      { "questionId": "q_a1b2c3", "enonce": "...", "optionChoisie": "Plus de 25 ans", "bonneReponse": "Plus de 25 ans", "correcte": true }
    ]
  },
  "lot": { "id": "l_x1", "libelle": "25% de rabais" }
}
```

### 4.4 Persistance

| Donnée | Clé de stockage | Portée |
|---|---|---|
| Configuration | `craaq_qr_config_v1` | Navigateur de l'appareil |
| Prospects | `craaq_qr_prospects_v1` | Navigateur de l'appareil |

Les données sont stockées dans le `localStorage` du navigateur : elles sont **propres à
l'appareil et au navigateur utilisés**, et ne sont pas partagées entre plusieurs tablettes.
Conséquences opérationnelles traitées en question ouverte Q3.

---

## 5. Règles de gestion et de validation

### 5.1 Quiz

| Réf | Règle |
|---|---|
| R1.1 | Une question comporte **au minimum 2 options**. |
| R1.2 | Une question désigne **exactement une** option comme bonne réponse. La sauvegarde est refusée sinon. |
| R1.3 | L'énoncé et le texte de chaque option sont obligatoires et non vides. |
| R1.4 | Le détail explicatif est **propre à chaque option** et facultatif ; s'il est vide, aucun encadré n'est affiché. |
| R1.5 | Le quiz doit comporter **au moins une question** pour pouvoir être lancé. |
| R1.6 | L'ordre d'affichage suit le champ `ordre`, modifiable par l'admin. |
| R1.7 | Les options sont présentées dans l'ordre défini par l'admin (pas de mélange automatique). |

### 5.2 Seuil de réussite

| Réf | Règle |
|---|---|
| R2.1 | Le seuil est un entier de **0 à 100** exprimé en pourcentage. |
| R2.2 | Le pourcentage obtenu est arrondi à l'entier le plus proche. |
| R2.3 | La roue est débloquée si `pourcentage >= seuil` (comparaison inclusive). |
| R2.4 | Le jeton `{seuil}` du texte d'accueil est remplacé à l'affichage par la valeur courante suivie du symbole `%`. Le pourcentage n'est **jamais** écrit en dur. |

### 5.3 Formulaire de captation

| Réf | Règle |
|---|---|
| R3.1 | Un champ marqué obligatoire bloque la soumission tant qu'il n'est pas renseigné. |
| R3.2 | Un champ `courriel` est validé sur son format (nom@domaine.ext). |
| R3.3 | Chaque variante contient **exactement un champ `courriel`, obligatoire**, servant de clé d'unicité. |
| R3.4 | Un champ `consentement` obligatoire bloque la soumission tant que la case n'est pas cochée. |
| R3.5 | Aucune case n'est pré-cochée. |
| R3.6 | Les champs `liste` et `cases` exigent au moins une option définie en configuration. |
| R3.7 | La comparaison des courriels ignore la casse et les espaces de début et de fin. |

### 5.4 Attribution et stock des lots

| Réf | Règle |
|---|---|
| R4.1 | Un lot est soit **illimité**, soit doté d'une **quantité entière supérieure ou égale à 1**. |
| R4.2 | La quantité par défaut d'un nouveau lot est **1**. |
| R4.3 | **Au moins un lot doit être illimité.** La sauvegarde de la roue est refusée sinon (règle bloquante). |
| R4.4 | Le tirage ne peut désigner qu'un lot **encore disponible** (illimité, ou quantité restante supérieure à 0). |
| R4.5 | Un lot épuisé n'est plus affiché sur la roue ni tiré. Grâce à R4.3, la roue comporte toujours au moins un lot. |
| R4.6 | Le stock est décrémenté **au moment du tirage**. |
| R4.7 | Si la participation est annulée après le tirage (doublon détecté ou abandon explicite), le stock du lot est **restitué**. |
| R4.8 | La position des lots sur la roue est mélangée à chaque partie ; la probabilité de chaque lot reste proportionnelle au nombre de quartiers disponibles qu'il occupe. |
| R4.9 | L'admin peut réinitialiser les quantités restantes à leur valeur initiale (remise à zéro entre deux événements). |

### 5.5 Unicité de participation

| Réf | Règle |
|---|---|
| R5.1 | Un courriel ne peut correspondre qu'à **une seule participation** enregistrée. |
| R5.2 | À la soumission, si le courriel existe déjà, la participation est **refusée** : aucun nouvel enregistrement, aucun lot attribué, stock restitué (R4.7). |
| R5.3 | Le visiteur en est informé par un message explicite rappelant la date de sa participation et, le cas échéant, le lot déjà obtenu. |

### 5.6 Administration et sécurité

| Réf | Règle |
|---|---|
| R6.1 | Le mot de passe administrateur est saisi dans un champ de type `password`. |
| R6.2 | Le mot de passe n'est jamais stocké en clair : seule une empreinte SHA-256 salée est conservée. |
| R6.3 | Le changement de mot de passe exige : ancien mot de passe valide, nouveau mot de passe d'au moins 8 caractères, confirmation identique. |
| R6.4 | Le mot de passe administrateur initial est `craaq2026`, à changer dès la première utilisation. |
| R6.5 | L'effacement des données exige la saisie du mot de passe d'autorisation **`suppressiondefinitive`**, fixe et non modifiable. |
| R6.6 | L'effacement supprime **les prospects uniquement** ; la configuration est conservée. |
| R6.7 | Toute modification de configuration n'est appliquée qu'après validation des règles ; en cas d'erreur, un message précis indique le champ fautif. |
| R6.8 | Chaque section des Réglages s'enregistre indépendamment : la sauvegarde repart de la dernière configuration valide et n'y applique que la section concernée. Un brouillon incomplet dans une autre section ne bloque donc pas l'enregistrement, et n'est jamais écrit. |

> **Avertissement de sécurité assumé** : l'application s'exécute entièrement dans le navigateur,
> sans serveur. La protection par mot de passe **dissuade** un accès opportuniste mais ne
> résiste pas à une personne techniquement compétente ayant accès à l'appareil, qui peut lire
> les données stockées. L'appareil du kiosque ne doit pas être laissé sans surveillance et les
> données doivent être exportées puis effacées à la fin de chaque événement.

---

## 6. Critères d'acceptation

Format : Étant donné … / Quand … / Alors …

### CA-1 — Texte d'accueil dynamique (F1.2, F4.6, R2.4)

- **CA-1.1** — Étant donné un seuil réglé à 60, quand j'ouvre l'accueil, alors le texte affiche « 60% » à l'emplacement du jeton `{seuil}`.
- **CA-1.2** — Étant donné que je modifie le seuil de 60 à 40, quand je retourne à l'accueil, alors le texte affiche « 40% » sans aucune autre intervention.
- **CA-1.3** — Étant donné un texte d'accueil sans jeton `{seuil}`, quand je l'enregistre, alors il s'affiche tel quel sans erreur.

### CA-2 — Détail explicatif par option (F1.4, R1.4)

- **CA-2.1** — Étant donné une question dont chaque option porte un détail distinct, quand je choisis la deuxième option, alors le détail affiché est **celui de la deuxième option**.
- **CA-2.2** — Étant donné une option sans détail, quand je la choisis, alors aucun encadré explicatif n'apparaît et le parcours continue normalement.
- **CA-2.3** — Quelle que soit l'option choisie, la bonne réponse est visuellement identifiée.

### CA-3 — Gestion des questions (F4.1, F4.2, R1.1 à R1.3)

- **CA-3.1** — Quand j'ajoute une question avec énoncé, 3 options et une bonne réponse désignée, alors elle est enregistrée et apparaît dans le quiz.
- **CA-3.2** — Quand je tente d'enregistrer une question sans bonne réponse désignée, alors la sauvegarde est refusée avec un message explicite.
- **CA-3.3** — Quand je tente d'enregistrer une question avec deux bonnes réponses, alors la sauvegarde est refusée.
- **CA-3.4** — Quand je tente d'enregistrer une question avec une seule option, alors la sauvegarde est refusée.
- **CA-3.5** — Quand je réordonne les questions, alors le quiz les présente dans le nouvel ordre.
- **CA-3.6** — Quand je supprime une question, alors elle disparaît du quiz après confirmation.

### CA-4 — Seuil de réussite (F4.3, R2.1 à R2.3)

- **CA-4.1** — Étant donné un seuil de 50 et un score de 50%, alors la roue est débloquée (comparaison inclusive).
- **CA-4.2** — Étant donné un seuil de 50 et un score de 49%, alors la roue n'est pas débloquée et le formulaire s'affiche en variante « échec ».
- **CA-4.3** — Quand je saisis un seuil hors de l'intervalle 0-100, alors la sauvegarde est refusée.

### CA-5 — Configuration de la roue (F4.4, R4.1 à R4.9)

- **CA-5.1** — Quand j'ajoute un lot, alors sa quantité par défaut est **1**.
- **CA-5.2** — Quand je tente d'enregistrer une roue dont aucun lot n'est illimité, alors la sauvegarde est **refusée** avec un message indiquant qu'au moins un lot illimité est requis.
- **CA-5.3** — Étant donné un lot en quantité 1 déjà gagné une fois, quand un nouveau visiteur tourne la roue, alors ce lot n'est ni affiché ni tiré.
- **CA-5.4** — Étant donné un lot illimité, quand il est gagné plusieurs fois, alors il reste disponible indéfiniment.
- **CA-5.5** — Quand je réinitialise les quantités, alors chaque lot retrouve sa quantité initiale.
- **CA-5.6** — Quand je saisis une quantité de 0 ou négative pour un lot non illimité, alors la sauvegarde est refusée.

### CA-6 — Formulaire configurable (F4.5, F1.10, R3.1 à R3.7)

- **CA-6.1** — Quand j'ajoute un champ de chaque type (texte, courriel, liste, cases, consentement), alors chacun s'affiche correctement dans le formulaire visiteur.
- **CA-6.2** — Étant donné un champ obligatoire laissé vide, quand je soumets, alors la soumission est bloquée et le champ fautif est signalé.
- **CA-6.3** — Étant donné un champ `consentement` obligatoire non coché, quand je soumets, alors la soumission est bloquée avec un message explicite.
- **CA-6.4** — Étant donné une variante « échec » configurée différemment, quand un visiteur échoue au quiz, alors il voit les champs et textes de la variante « échec ».
- **CA-6.5** — Quand je tente d'enregistrer une variante sans champ `courriel`, alors la sauvegarde est refusée.
- **CA-6.6** — Un courriel mal formé bloque la soumission.

### CA-7 — Unicité de participation (F1.11, R5.1 à R5.3)

- **CA-7.1** — Étant donné un courriel déjà enregistré, quand je soumets le formulaire avec ce courriel, alors la participation est refusée et un message rappelle la date de la participation précédente.
- **CA-7.2** — Dans ce cas, aucun second enregistrement n'est créé et le stock du lot tiré est restitué.
- **CA-7.3** — La détection fonctionne indépendamment de la casse et des espaces superflus.

### CA-8 — Accès administrateur (F2.2, F2.3, R6.1 à R6.4)

- **CA-8.1** — Quand je saisis le mot de passe administrateur, alors les caractères sont **masqués**.
- **CA-8.2** — Quand je saisis un mot de passe erroné, alors l'accès est refusé.
- **CA-8.3** — Quand je change mon mot de passe avec l'ancien correct, un nouveau d'au moins 8 caractères et une confirmation identique, alors le changement est effectif à la connexion suivante.
- **CA-8.4** — Quand l'ancien mot de passe est erroné, ou la confirmation différente, alors le changement est refusé avec un message précis.

### CA-9 — Onglets et effacement (F3.5, F4.8, R6.5, R6.6)

- **CA-9.1** — L'administration présente deux onglets : « Prospects » et « Réglages ».
- **CA-9.2** — Le bouton « Effacer les données » **n'est plus** dans l'onglet Prospects.
- **CA-9.3** — Dans Réglages, l'effacement exige la saisie de `suppressiondefinitive` ; toute autre saisie le refuse.
- **CA-9.4** — Après effacement, la liste des prospects est vide et la **configuration est intacte**.

### CA-10 — Prospects et export (F3.1 à F3.4)

- **CA-10.1** — La liste affiche une colonne par champ configuré du formulaire.
- **CA-10.2** — L'export CSV s'ouvre correctement dans Excel avec les accents préservés.
- **CA-10.3** — L'export contient les réponses au quiz, le score, le lot et le consentement.

### CA-11 — Enregistrement par section (R6.8)

- **CA-11.1** — Étant donné une question incomplète en cours de rédaction, quand j'enregistre la roue, alors la roue est enregistrée et la question incomplète n'est pas écrite.
- **CA-11.2** — Quand j'enregistre une section, alors les modifications non enregistrées des autres sections restent présentes à l'écran.
- **CA-11.3** — Quand une section est refusée, alors aucune donnée n'est écrite et la configuration précédente reste intacte.

---

## 7. Contraintes techniques

### 7.1 Contraintes imposées par le contexte

| # | Contrainte | Conséquence |
|---|---|---|
| C1 | **Fonctionnement hors ligne** au kiosque, sans réseau garanti | Aucun appel serveur ; tout est embarqué |
| C2 | **Aucune installation** sur l'appareil | Ouverture par simple double-clic sur un fichier |
| C3 | Utilisation sur **tablette tactile** et écran, en plein écran | Interface tactile, cibles d'au moins 44 px, texte lisible debout |
| C4 | Maintenance par une **équipe non technique** | Configuration exclusivement par interface, jamais par le code |
| C5 | Aucune donnée personnelle transmise à un tiers | Stockage local uniquement, export manuel |

### 7.2 Stack proposée (à confirmer — voir Q5)

- **HTML, CSS et JavaScript natifs**, sans cadriciel ni dépendance d'exécution.
- **Aucune étape de compilation obligatoire** pour développer.
- Un **script d'assemblage** (Node.js, sans dépendance externe) produit un **fichier HTML unique
  et autonome** (`dist/quiz-et-roue-de-la-fortune.html`) incluant styles, scripts et images en
  ligne : c'est le livrable déployé au kiosque.
- Persistance via `localStorage`, avec repli en mémoire si indisponible.
- Empreinte de mot de passe via l'API Web Crypto (SHA-256).

**Justification** : le contexte (hors ligne, aucune installation, équipe non technique, durée de
vie longue sans maintenance) rend un cadriciel plus coûteux que bénéfique. Le fichier unique
supprime toute dépendance au moment de l'événement.

### 7.3 Arborescence du projet

```
quiz-et-roue-de-la-fortune/
├── PRD.md                  <- source de vérité
├── README.md
├── .gitignore
├── package.json            <- scripts de développement et d'assemblage
├── src/
│   ├── index.html
│   ├── styles/             <- feuilles de style
│   ├── scripts/            <- modules JavaScript (quiz, roue, formulaire, admin, stockage)
│   └── data/               <- configuration par défaut
├── public/                 <- images (logos CRAAQ)
├── scripts/build.js        <- assemblage en fichier unique
├── dist/                   <- livrable généré (non versionné)
├── docs/                   <- guide d'utilisation pour l'équipe
└── tests/                  <- tests des règles de gestion
```

### 7.4 Compatibilité

Navigateurs récents : Chrome, Edge, Safari, Firefox (deux dernières versions majeures).
Résolutions cibles : tablette 1024 x 768 et plus, écran de bureau 1920 x 1080.

### 7.5 Accessibilité et langue

- Interface intégralement en **français du Québec**.
- Contrastes conformes au niveau AA, navigation possible au clavier, libellés associés aux champs.

### 7.6 Identité visuelle

Palette CRAAQ : vert `#2E6B3E`, vert foncé `#1E4A2A`, lime `#A2D95A`, crème `#F3F7EC`,
encre `#1E3A16`, doré `#E0A82E`. Logos officiels fournis (horizontal blanc, carré couleur).

---

## 8. Points tranchés

> Ces points ont été soumis puis **validés le 2026-08-11**. Ils font désormais partie de la
> spécification au même titre que le reste du document.

| # | Sujet | Décision retenue |
|---|---|---|
| **Q1** | Position de la captation | **Après la roue.** Le visiteur joue, gagne, puis laisse ses coordonnées : la motivation est plus forte. Le prix étant envoyé par courriel, le consentement reste obligatoire au formulaire. Cela modifie le comportement de la version actuelle. |
| **Q2** | Lot épuisé | **Le lot disparaît de la roue** dès que sa quantité est épuisée : il n'est ni affiché ni tirable. La règle R4.3 garantit qu'il reste toujours au moins un lot. |
| **Q3** | Données par appareil | **Un seul appareil de référence** pour les quantités limitées. Un export et un import de la configuration sont fournis pour recopier les réglages sur d'autres appareils (F4.9). |
| **Q4** | Champ courriel | **Obligatoire et unique dans chaque variante.** Il sert de clé d'unicité et d'adresse d'envoi du prix. Sa suppression est refusée par la validation. |
| **Q5** | Stack technique | **HTML, CSS et JavaScript natifs**, sans cadriciel, assemblés en un fichier HTML unique et autonome fonctionnant hors ligne. |
| **Q6** | Reprise des données | **Repartir de zéro.** Aucune reprise des participants de la version précédente. |
| **Q7** | Réinitialisation du stock | **Manuelle**, par un bouton dans l'onglet Réglages. |
| **Q8** | Envoi des prix | **Manuel**, par l'équipe, à partir de l'export CSV. Aucun envoi automatique. |

## 9. Hors périmètre

Éléments explicitement **non** couverts par cette version :

- Envoi automatique de courriels aux gagnants.
- Serveur, base de données centralisée ou synchronisation entre appareils.
- Comptes utilisateurs multiples ou rôles différenciés côté administration.
- Tirage au sort global en fin d'événement.
- Statistiques avancées et tableaux de bord analytiques.
- Version multilingue.
- Intégration directe à un outil d'infolettre.

---

## 10. Journal des décisions

| Date | # | Décision | Motif | Statut |
|---|---|---|---|---|
| 2026-08-11 | D1 | Création d'un projet autonome `quiz-et-roue-de-la-fortune`, dépôt Git propre, sans couplage au dossier parent | Demande explicite ; le dossier parent n'est pas un dépôt Git, aucun couplage n'existe | Fait |
| 2026-08-11 | D2 | `PRD.md` établi comme source de vérité unique, mis à jour avant toute implémentation | Demande explicite | Fait |
| 2026-08-11 | D3 | Administration réorganisée en deux onglets : Prospects et Réglages | Demande explicite | Fait |
| 2026-08-11 | D4 | Champ de mot de passe administrateur de type `password` | Demande explicite ; le mot de passe ne doit pas être visible au kiosque | Fait |
| 2026-08-11 | D5 | « Effacer les données » déplacé vers Réglages, protégé par le mot de passe fixe `suppressiondefinitive` | Demande explicite ; action destructrice à isoler des usages courants | Fait |
| 2026-08-11 | D6 | Mot de passe administrateur modifiable, stocké sous forme d'empreinte SHA-256 salée | Demande explicite ; ne jamais conserver de mot de passe en clair | Fait |
| 2026-08-11 | D7 | Le détail explicatif devient propre à **chaque option**, et non plus à la question | Demande explicite ; permet d'expliquer aussi pourquoi une réponse est fausse | Fait |
| 2026-08-11 | D8 | Au moins un lot illimité obligatoire (règle bloquante) | Demande explicite ; garantit qu'une roue reste toujours jouable malgré l'épuisement des stocks | Fait |
| 2026-08-11 | D9 | Quantité par défaut d'un nouveau lot fixée à 1 | Demande explicite | Fait |
| 2026-08-11 | D10 | Le seuil de réussite est injecté dans le texte d'accueil via le jeton `{seuil}` | Demande explicite : le pourcentage ne doit jamais être codé en dur | Fait |
| 2026-08-11 | D11 | Formulaire de captation configurable, avec variantes « réussite » et « échec » | Demande explicite | Fait |
| 2026-08-11 | D12 | Captation déplacée **après** la roue, conformément au parcours décrit | Parcours demandé ; contradiction avec l'implémentation actuelle signalée en Q1 | Fait |
| 2026-08-11 | D13 | Stack HTML/CSS/JS natifs, livrable en fichier HTML unique autonome | Contexte hors ligne, aucune installation, maintenance par équipe non technique | Fait |
| 2026-08-11 | D14 | Stock décrémenté au tirage et restitué si la participation est annulée | Évite qu'un même lot limité soit attribué deux fois | Fait |
| 2026-08-11 | D15 | PRD validé en bloc par la responsable ; passage en version 1.0 et ouverture du développement | Validation explicite du 2026-08-11 | Fait |
| 2026-08-11 | D16 | Ajout de l'export et de l'import de configuration (F4.9) | Conséquence de Q3 : permet de recopier les réglages sur plusieurs appareils sans ressaisie | Fait |
| 2026-08-11 | D17 | Empreinte SHA-256 implémentée en JavaScript natif, sans dépendre de l'API Web Crypto | `crypto.subtle` est indisponible hors contexte sécurisé, or l'application s'ouvre en `file://` au kiosque | Fait |
| 2026-08-11 | D18 | Enregistrement des Réglages **section par section** (R6.8) plutôt qu'en bloc | Défaut relevé par les tests de bout en bout : un brouillon incomplet dans une section bloquait l'enregistrement des autres, et une sauvegarde globale risquait d'écrire un contenu invalide | Fait |
| 2026-08-11 | D19 | Le champ courriel ne peut être ni supprimé ni changé de type depuis l'interface | Application de Q4 : sans lui, ni le dédoublonnage ni l'envoi du prix ne sont possibles | Fait |

---

## Annexe A — Glossaire

| Terme | Définition |
|---|---|
| **Participant / visiteur** | Personne qui joue au quiz au kiosque |
| **Prospect** | Participant ayant laissé ses coordonnées |
| **Lot** | Récompense pouvant être gagnée à la roue |
| **Quartier** | Portion visuelle de la roue correspondant à un lot |
| **Seuil de réussite** | Pourcentage minimal de bonnes réponses débloquant la roue |
| **Variante** | Version du formulaire de captation selon la réussite ou l'échec au quiz |
| **Configuration** | Ensemble des paramètres modifiables par l'administrateur |
