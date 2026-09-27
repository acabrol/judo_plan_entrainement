# Ergonomie mobile — 27 septembre 2026

## Objectif

Permettre à un adolescent de remplir les formulaires sur smartphone avec peu d’explications, de retrouver sa saisie après une interruption et de choisir l’ordre des rubriques. L’évaluation, le plan et l’événement restent chacun sur une seule page, conformément à la demande utilisateur.

## Sources consultées et application

| Source | Enseignement retenu | Application dans le projet |
|---|---|---|
| [Nielsen Norman Group — Teenager’s UX](https://www.nngroup.com/articles/usability-of-websites-for-teenagers/) | Les observations auprès d’adolescents appuient des contenus courts, des regroupements lisibles et des interactions simples, sans ton infantilisant. Les petites cibles et les pages lentes compliquent l’usage mobile. | Consignes courtes, rubriques du quotidien placées avant les tests, suppression des fenêtres successives et de la police distante. |
| [W3C — Target Size Enhanced](https://www.w3.org/WAI/WCAG22/Understanding/target-size-enhanced.html) | Le critère renforcé propose des cibles de 44 × 44 pixels CSS, sous réserve de ses exceptions. | Boutons d’au moins 44 pixels, champs de 48 pixels et libellés de cases cliquables. Ce choix ne constitue pas une déclaration de conformité WCAG globale. |
| [W3C — Labeling Controls](https://www.w3.org/WAI/tutorials/forms/labels/) | Chaque commande doit être nommée ; un label associé facilite son activation et sa compréhension par les technologies d’assistance. | Labels reliés aux champs, noms accessibles pour les scores techniques, valeurs des curseurs exposées et messages annoncés. |
| [GOV.UK — Question pages](https://design-system.service.gov.uk/patterns/question-pages/) | Demander seulement les informations utiles, préciser le caractère facultatif et éviter de faire ressaisir ce qui est déjà connu. | Profil prérempli, réponses facultatives, curseurs sans score implicite et brouillon local. Le modèle séquentiel également proposé par cette source n’est pas retenu ici : la demande impose une page unique. |

La navigation inférieure, la taille de texte de 16 pixels dans les champs et l’ordre des rubriques sont des choix de conception pour ce projet. Ils doivent encore être confrontés à un usage réel par des adolescents.

## Changements livrés

### Évaluation

- Toutes les rubriques sont visibles sur une page, à l’exception des champs non applicables à l’âge ou au sexe renseignés.
- Six raccourcis : profil, quotidien, santé, judo, tests physiques et résumé.
- Les tests peuvent être renseignés plus tard ; prénom et naissance sont les seuls champs obligatoires.
- Identité importée préremplie et objectifs conservés lors du démarrage du formulaire.
- Curseurs non utilisés sans score enregistré ; choix simples initialement sur « Choisir… ».
- Résumé actualisé pendant la saisie, avec état « À renseigner » lorsque les données requises manquent.
- Bouton d’enregistrement accessible pendant le défilement.

### Plan

- Ajout et modification des périodes, échéances et disponibilités dans la page.
- Dates choisies dans des champs natifs ; priorités et objectifs directement éditables.
- Champs de catégorie et d’importance disponibles pour les échéances.
- Annulation d’une suppression proposée pendant dix secondes.
- Préparation mentale conservée lors des changements d’onglet.

### Événement

- Informations, résultats et ressenti avant/pendant/après sur la même page.
- Nom et date requis, autres informations facultatives.
- Date initialisée au jour local ; clavier numérique pour les mesures.
- Résultats de combat et question de garde affichés uniquement pour une compétition.
- Saisie réinitialisée après l’enregistrement, y compris cases, curseurs et sommeil de la veille.

### Usage mobile et brouillon

- Une colonne sur petit écran, navigation principale en bas et commandes d’enregistrement au-dessus.
- Champs de texte de 16 pixels, boutons et sélecteurs agrandis, focus visible et prise en compte de la réduction des animations.
- Sauvegarde locale différée pendant la saisie et lors du passage en arrière-plan ; restauration du profil, des formulaires et de l’onglet.
- Message distinct si le stockage est indisponible. Le brouillon reste propre au navigateur/appareil ; le fichier exporté reste distinct.
- Aucune nouvelle bibliothèque et aucune ressource réseau nécessaire au parcours principal.

## Vérifications effectuées

Recette automatisée avec Playwright et Chromium, par ouverture directe de `index.html` :

- affichage des 23 rubriques applicables à un profil adolescent, sans bouton suivant ;
- changement des champs conditionnels selon le profil ;
- absence d’enregistrement d’un curseur non utilisé ;
- reprise de l’identité, des notes et des objectifs après rechargement ;
- ajout, édition, suppression et annulation dans le plan ;
- conservation de la préparation mentale lors des changements d’onglet ;
- type d’événement, enregistrement et réinitialisation ;
- téléchargement du profil ;
- absence d’erreurs JavaScript dans ces parcours et de requêtes HTTP ;
- absence de débordement horizontal des trois formulaires aux largeurs 320, 390, 768 et 1 440 pixels ;
- hauteur minimale des boutons, taille du texte des champs et nom accessible des champs visibles.

L’inspection visuelle a été effectuée sur des captures de la version mobile. Il s’agit d’une émulation navigateur : Safari/iOS, le clavier virtuel réel et les lecteurs d’écran n’ont pas été testés. Aucun test auprès d’adolescents n’a encore eu lieu ; aucun gain de temps chiffré n’est revendiqué.

## Suite à vérifier avec des utilisateurs

Faire réaliser à quelques adolescents trois tâches : renseigner un bilan partiel, ajouter un tournoi au plan et enregistrer un événement. Observer les hésitations, erreurs de saisie, rubriques ignorées et la reprise après interruption. Comparer les résultats avant de réduire davantage le contenu ou de modifier les échelles.

Cette refonte ne clôt pas l’audit initial. La fiabilité complète de l’aller-retour Markdown, les questionnaires et les règles de calcul restent suivis dans [TODO.md](../TODO.md).
