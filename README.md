# 🥋 Judo Plan d'Entraînement

Outil de suivi et de planification de la préparation du judoka.

## Présentation

Cette application web permet aux judokas (adolescents et adultes, hommes et femmes) de :

- **Évaluer** leur condition physique, technique, mentale et hygiène de vie tous les trimestres
- **Planifier** leur préparation en intégrant le calendrier sportif et la vie personnelle
- **Suivre** leur progression dans le temps grâce à un profil multidimensionnel
- **Analyser** leurs compétitions et événements sportifs

## Comment ça marche

1. **Ouvrir `index.html`** dans un navigateur (fonctionne localement, aucun serveur nécessaire)
2. **Créer ou importer un profil** au format Markdown (`.md`)
3. **Remplir l'évaluation sur une seule page**, dans l'ordre souhaité, avec les raccourcis par rubrique
4. **Consulter le tableau de bord** avec les indicateurs 🟢🟠🔴
5. **Exporter le profil** mis à jour

## Caractéristiques

- **Page unique standalone** : un seul fichier HTML, zéro dépendance, zéro installation
- **Fichier profil Markdown** : lisible directement sur smartphone, portable
- **Adapté aux adolescents et adultes** : sections et seuils adaptés selon l'âge et le sexe
- **Évaluation sur une page** : rubriques regroupées par profil, quotidien, santé, judo, tests physiques et résumé ; aucun bouton « Suivant »
- **Saisie sur smartphone** : navigation en bas de l'écran, champs lisibles, grandes zones tactiles et bouton d'enregistrement accessible pendant le défilement
- **Plan directement modifiable** : périodes, échéances et disponibilités se saisissent dans la page, sans fenêtres successives
- **Événement sur une page** : informations, résultats et ressenti avant/pendant/après ; résultats de combat affichés uniquement pour une compétition
- **Brouillon local automatique** : reprise de la saisie sur le même appareil et navigateur, lorsque le stockage local est disponible
- **Préparation mentale** : inspirée de "Apprendre à Gagner" de Christian Ramos
- **Import de calendriers** : CSV FFJudo, fichiers ICS standard
- **Références scientifiques** : basé sur des publications validées

## Utilisation

### En local
Ouvrir `index.html` avec n'importe quel navigateur web.

### Sur GitHub Pages
Le repository peut être publié directement comme site GitHub Pages.

### Remplir depuis un smartphone

Commencer par le prénom et la naissance, puis renseigner les rubriques connues. Les autres réponses sont facultatives ; les tests physiques peuvent être complétés plus tard avec l'entraîneur. Un curseur non utilisé reste sans réponse.

Le brouillon conserve le profil et les formulaires sur cet appareil via le stockage du navigateur. Il n'est pas synchronisé. En navigation privée, après effacement des données du navigateur ou selon les restrictions d'ouverture locale, cette reprise peut être indisponible. L'indication sous l'en-tête précise le statut. Utiliser **Exporter** pour télécharger un fichier personnel.

Les choix de conception, les sources consultées et la recette de cette interface sont décrits dans [Ergonomie mobile](docs/ergonomie_mobile.md). Les limites de l'import/export existant restent suivies dans [TODO.md](TODO.md).

## Fichier profil

Le profil du judoka est stocké dans un fichier Markdown (`.md`) portable et lisible.
Voir `exemples/profil_exemple.md` pour un exemple complet.

## Structure du projet

```
judo_plan_entrainement/
├── index.html             ← Application complète (page unique)
├── README.md              ← Ce fichier
├── SPECIFICATIONS.md      ← Audit et spécifications
├── TODO.md                ← Travaux priorisés
├── exemples/
│   └── profil_exemple.md  ← Exemple de profil
└── docs/
    ├── sources.md         ← Références scientifiques
    ├── ergonomie_mobile.md ← Choix d'interface et sources UX
    └── referentiel_preparation_annuelle_judoka_adolescent.md
```

## Sources scientifiques principales

Voir [docs/sources.md](docs/sources.md) pour la liste complète des références.

## Licence

Ce projet est distribué sous licence **MIT**. Tu peux l’utiliser, le modifier et le redistribuer, y compris dans un projet commercial, en conservant la notice de copyright et le texte de la licence.

Voir le fichier [LICENSE](LICENSE). Identifiant SPDX : `MIT`.
