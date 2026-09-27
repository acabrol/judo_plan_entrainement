# Spécifications — Judo Plan d’Entraînement

Date de l’audit : 26 septembre 2026.

État de l’application au 27 septembre 2026 : formulaires sur une page, édition directe du plan et reprise locale des brouillons. Voir [Ergonomie mobile](docs/ergonomie_mobile.md) pour les changements et leur vérification ; les défauts du format Markdown restent à traiter.

Révision des exigences du 27 septembre 2026 : standardisation des saisies, profil détaillé et évaluations fondées sur des observations. Les sections 3 et 4 décrivent l’existant ; la section 5 conserve l’audit historique du 26 septembre ; les sections 6 à 10 définissent la cible. Cette révision documentaire ne signifie pas que les nouvelles fonctions sont déjà implémentées.

Ce document décrit le projet observé, ses défauts et la cible fonctionnelle déduite de son contenu. Les exigences cibles ne sont pas toutes implémentées. Les travaux correspondants sont regroupés dans [TODO.md](TODO.md).

## 1. Périmètre et méthode

L’audit porte sur les cinq fichiers de contenu présents :

| Fichier | Rôle |
|---|---|
| [README.md](README.md) | Promesse produit et instructions d’utilisation |
| [index.html](index.html) | Interface, styles, logique métier, import et export |
| [exemples/profil_exemple.md](exemples/profil_exemple.md) | Exemple de données et de document portable |
| [docs/sources.md](docs/sources.md) | Bibliographie scientifique |
| [Référentiel annuel](docs/referentiel_preparation_annuelle_judoka_adolescent.md) | Besoins métier, suivi et périodisation |

La lecture du code a été complétée par l’exécution de ses fonctions JavaScript dans Node.js 22.22.0, avec interfaces DOM minimales simulées : import de l’exemple, export puis réimport, données synthétiques, indicateurs sans données et bornes de calcul. La syntaxe du script a également été vérifiée.

Les vérifications initiales ne constituaient pas une recette dans un navigateur. La refonte du 27 septembre a ensuite fait l’objet de contrôles Chromium, détaillés dans le document d’ergonomie ; la recette complète et les essais auprès d’adolescents restent à réaliser. L’audit initial ne valide ni les affirmations scientifiques ni les règles fédérales citées. Les sources de nomenclature consultées pour cette révision sont indiquées en F-01. Aucun dépôt Git exploitable n’était présent dans l’environnement audité, donc aucun historique de versions n’a été analysé.

## 2. Finalité du produit

Permettre à un judoka de documenter sa préparation, ses évaluations, sa progression et ses compétitions dans un profil Markdown personnel, lisible et transportable.

Le référentiel donne la priorité au suivi individuel dans le temps, à la progression technique, à la récupération, à la scolarité et au plaisir de pratiquer. Il demande de conserver les dimensions séparées, sans note globale du judoka.

Le README vise adolescents et adultes, hommes et femmes. Le référentiel détaillé est centré sur les adolescents ; l’adaptation aux adultes est donc partielle et doit être explicitée. Le judoka est l’utilisateur principal. L’entraîneur et, pour les mineurs, les responsables familiaux peuvent accompagner la saisie et la lecture, sans comptes ni rôles applicatifs à ce stade.

### Limites de périmètre

- Application personnelle et locale, utilisable sans serveur applicatif.
- Un profil actif à la fois ; changement de personne par import de fichier.
- Aucun compte, synchronisation distante, espace club ou partage automatique existant.
- Aucune génération automatique de programme individualisé existante.
- Les futures aides à la décision doivent rester explicables et contextualisées ; le référentiel ne justifie pas un diagnostic automatisé.

## 3. Architecture observée au 27 septembre 2026

| Élément | État actuel |
|---|---|
| Livraison | Un fichier HTML ouvrable directement ; hébergement statique possible, déploiement non vérifié |
| Interface | HTML et CSS intégrés, quatre onglets, formulaires sur une page et navigation inférieure sur smartphone |
| Logique | JavaScript natif, variables globales, gestionnaires en ligne |
| Graphique | Radar dessiné avec Canvas 2D |
| État | Objet global `D`, `hasProfile`, tableaux temporaires d’objectifs et difficultés |
| Fichiers | Lecture par `FileReader`, export par `Blob` et lien de téléchargement |
| Persistance | État en mémoire, brouillon automatique dans `localStorage` si disponible, export manuel distinct |
| Dépendances | Pas de bibliothèque JavaScript ni de police distante ; polices locales de secours |
| Qualité | Contrôles ponctuels Node.js et Chromium documentés ; pas de suite de tests livrée dans le projet ni d’intégration continue |

Le parcours principal ne nécessite plus de ressource réseau. Le brouillon est propre au navigateur et à l’appareil ; sa disponibilité dépend du stockage local. Il ne constitue ni une synchronisation ni une écriture automatique dans le fichier Markdown.

## 4. Couverture fonctionnelle au 27 septembre 2026

| Domaine | Existant dans le code | Écart ou limite |
|---|---|---|
| Profil | Identité, naissance, sexe, club, grade, catégorie, fréquence globale, latéralité, ancienneté ; validation des champs obligatoires | Grade et catégorie en texte libre ; pas d’Ambidextre, de répartition des séances ni de suivi des interruptions |
| Système d’attaque | Techniques favorites, secondaires, enchaînements, contres, garde, sol, transitions | Suppression d’une ancienne valeur par champ vidé non prise en compte |
| Évaluation | 23 rubriques définies, résumé compris, regroupées par thèmes sur une page | Rubrique croissance conditionnelle ; nombreuses notes sans définition complète des valeurs |
| Adaptation du formulaire | Croissance et champs RED-S conditionnés par le profil saisi ; identité et objectifs préremplis après import | Adaptation métier adulte/adolescent à approfondir |
| Objectifs et difficultés | Ajout, suppression, statut et actions ; import de commentaires et dates | Commentaire d’objectif non éditable ; historique non rattaché aux bilans |
| Synthèse | Résumé actualisé pendant la saisie ; plusieurs indicateurs incomplets affichés « À renseigner » | Règles dispersées ; gestion des données absentes à compléter dans toutes les vues |
| Tableau de bord | Profil, radar, alertes, évolution, objectifs, échéances et événements | Radar partiellement fictif ; tableau longitudinal limité à neuf indicateurs |
| Plan | Ajout, édition directe et suppression de périodes, échéances et disponibilités ; annulation temporaire de suppression | Saison importée ignorée ; contrôles métier encore partiels |
| Préparation mentale | Projet personnel, mots-clés et routines | Journal d’imagerie présent dans l’exemple mais non géré |
| Événements | Formulaire sur une page ; résultats de combat conditionnés au type ; réinitialisation après enregistrement | Pas de détail des combats ni édition ; pas de résultat propre au passage de grade |
| Calendriers | Import CSV et ICS vers les échéances | Prise en charge limitée ; absence de validation, aperçu et dédoublonnage |
| Portabilité Markdown | Import, export et glisser-déposer | Aller-retour destructeur : données modifiées ou omises |
| Suivi quotidien/hebdomadaire | Décrit dans le référentiel | Pas de journal de séances ni suivi de charge dans l’application |
| Périodisation détaillée | Périodes et échéances | Pas de semaines, séances ou articulation des cycles |
| Questionnaires complémentaires | SAS-2 : 15 réponses saisissables | Scores SAS-2 non calculés ; ASSQ et CSAI-2R absents |

### Inventaire des 23 rubriques

Cette liste recense les domaines ; elle n’impose aucun parcours séquentiel. Le regroupement actuel est décrit dans le document d’ergonomie, et son évolution cible en F-05.

1. Profil.
2. Système d’attaque.
3. Croissance et santé, conditionnelle.
4. Sommeil.
5. Mobilité.
6. Souplesse.
7. Force et préhension.
8. Explosivité générale.
9. Explosivité spécifique judo.
10. Aérobie.
11. SJFT.
12. Anaérobie.
13. Nutrition.
14. Signaux RED-S.
15. Écrans.
16. Stress et mental.
17. Questionnaire présenté comme SAS-2.
18. École ou travail.
19. Blessures.
20. Technique : 22 domaines et huit critères de tokui-waza.
21. Compétitions du trimestre.
22. Objectifs et difficultés.
23. Résumé et enregistrement.

## 5. Constats historiques de l’audit du 26 septembre 2026

Les identifiants `AUD-*` servent de références aux tâches. « Reproduit » signifie vérifié en exécutant les fonctions avec DOM simulé ; « lecture » désigne une conclusion tirée du code à cette date. Les noms de fonctions et numéros de ligne sont historiques ; certains points ont été partiellement corrigés depuis. L’état actuel figure aux sections 3 et 4 et dans le suivi du TODO.

| ID | Gravité | Constat et preuve | Localisation |
|---|---|---|---|
| AUD-01 | Critique | Reproduit : le stress `6 /10` devient `610`, la confiance `5 /10` devient `510`, `22:00` devient `2200`. La durée de sommeil est rangée dans `duree_moyenne`, alors que l’application attend `sommeil_duree`. Les noms sont dérivés du texte sans tenir compte du domaine ; des libellés peuvent se collisionner. | `parseMarkdownProfile()`, vers lignes 1435–1444 |
| AUD-02 | Critique | Lecture et export synthétique : seules huit catégories d’évaluation sont sérialisées. Souplesse, explosivité judo, aérobie, anaérobie, nutrition, RED-S, SAS-2, école, blessures, technique/tokui-waza et bilan des compétitions ne sont pas exportés. | `generateMarkdown()`, objet `cats`, vers ligne 1495 |
| AUD-03 | Critique | Reproduit : la compétition de l’exemple est importée, puis disparaît après export/réimport. L’export écrit `Competition` et le parseur attend `Compétition`. Les tableaux avant/pendant/après et combats ne sont pas importés ; la saison et le journal d’imagerie sont ignorés. | Parseur et sérialiseur, vers lignes 1359, 1389 et 1530 |
| AUD-04 | Élevée | Lecture : « enregistré » ou « sauvegardé » signifie seulement modification de `D`. Rechargement ou fermeture perd les changements non exportés. Le plan mental non enregistré est écrasé lors d’un nouveau rendu de l’onglet. | `finishEval()`, `savePlanData()`, `saveEvent()`, `renderPlan()` |
| AUD-05 | Élevée | Reproduit : un texte quelconque vide le profil actif et finit avec `hasProfile = true`. Lecture : les cellules vides sont supprimées par `filter(Boolean)`, ce qui décale les colonnes. Aucune validation transactionnelle ni gestion d’erreur de lecture. | `parseMarkdownProfile()`, import de fichiers |
| AUD-06 | Élevée | Lecture : des valeurs saisies ou importées sont interpolées directement dans `innerHTML`, y compris des attributs `value`. Un document importé peut injecter du HTML et des gestionnaires d’événements. Aucune exploitation navigateur exécutée pendant cet audit. | `renderDashboard()`, formulaires dynamiques, rendus du plan et des événements |
| AUD-07 | Élevée | Reproduit : données vides → mobilité, préhension, anaérobie, nutrition, écrans et blessures en vert. Lecture : les curseurs préremplis valent 5, même pour `max=3` ; des sélecteurs ont déjà une réponse, sans action utilisateur. | `buildSlider()`, `computeAxes()`, `sas2Q()` |
| AUD-08 | Élevée | Lecture : l’import ne rappelle pas `prefillFromProfile()` ni `renderWizard()`. Objectifs et difficultés importés ne sont pas copiés dans les tableaux temporaires à ce moment ; terminer une évaluation peut donc les remplacer par des tableaux vides ou anciens. Les champs liés au sexe sont construits avant la saisie. | Fin de `parseMarkdownProfile()`, `renderWizard()`, `finishEval()`, `defineSteps()` |
| AUD-09 | Élevée | Lecture : les attributs `required`, `min`, `max` ne sont pas contrôlés par les actions de navigation/enregistrement. Une évaluation peut être enregistrée sans identité. Aucun contrôle de cohérence des dates, des résultats ou des valeurs importées. | `wizardNext()`, `wizGoTo()`, `finishEval()`, `saveEvent()` |
| AUD-10 | Moyenne | Lecture : une forte confiance ou qualité de sommeil reçoit la même couleur défavorable qu’une forte fatigue. Reproduit : 5 % de perte anaérobie est vert dans `computeAxes()`, orange dans `calcLoss()` ; 10 % est orange dans le premier et rouge dans le second. | `updateSlider()`, `threshold()`, `computeAxes()`, `calcLoss()` |
| AUD-11 | Élevée | Lecture : cinq axes du radar restent à `0.5` — mobilité, aérobie, anaérobie, SJFT, technique. Préhension et CMJ sont normalisés sur des constantes sans justification liée au profil. Absence de donnée parfois présentée comme niveau maximal. | `drawRadar()` |
| AUD-12 | Élevée | Lecture : bloc d’alerte RED-S toujours caché et jamais actualisé ; aucune agrégation des items SAS-2. L’axe « Stress/Anxiété » utilise uniquement la fatigue mentale. La consigne RED-S « 3 signaux » n’est pas justifiée dans le référentiel local. | Étapes `reds`, `sas2`, `computeAxes()`, `renderAlerts()` |
| AUD-13 | Moyenne | Lecture : ICS découpé sommairement, sans lignes repliées ni récurrences ; CSV découpé avec `split`, incompatible avec des séparateurs dans un champ entre guillemets. Pas de dédoublonnage. Compatibilité FFJudo non démontrée par un fichier témoin. | `importICS()`, `importCSV()` |
| AUD-14 | Moyenne | Lecture : date d’évaluation imposée au jour courant UTC ; janvier/février associés à « Décembre » de la même année. Dernier bilan choisi par ordre du tableau, sans tri chronologique. | `finishEval()`, `getTrimestreName()`, `drawRadar()`, `renderAlerts()` |
| AUD-15 | Moyenne | Lecture : `updateEventForm()` vide ; réinitialisation partielle après sauvegarde d’événement, laissant curseurs, cases et sommeil de la veille. Pas d’édition/suppression d’évaluation ou événement. | `updateEventForm()`, `saveEvent()` |
| AUD-16 | Moyenne | Lecture : labels généralement non associés aux champs, étapes cliquables en `div`, radar sans équivalent textuel complet, absence de région d’annonce pour les messages. Attribut `class` dupliqué sur `evolution-card`. | HTML statique, `buildSlider()`, `updateWizard()`, `toast()` |
| AUD-17 | Moyenne | Documentation : absence de contrat de données versionné, tests et licence définie ; promesses d’adaptation et de validation scientifique plus larges que la démonstration fournie. Sources scientifiques et institutionnelles réparties dans deux documents. | README, documentation et structure du projet |

## 6. Exigences fonctionnelles cibles

### F-01 — Profil et démarrage

L’utilisateur doit pouvoir créer un profil, importer un profil compatible et retrouver ses informations dans tous les formulaires. Prénom et naissance doivent être contrôlés avant enregistrement d’un profil complet. Une naissance future doit être refusée. Une information facultative doit pouvoir être supprimée en vidant son champ.

Le changement de profil doit traiter explicitement les modifications non exportées. Un import invalide doit laisser intactes les données actives. Le sexe et la naissance doivent actualiser immédiatement les champs conditionnels, sans effacer les autres réponses. Un champ non applicable doit être distingué d’une mesure égale à zéro.

#### F-01a — Fréquence, identité sportive et catégorie

| Champ | Saisie cible et règle |
|---|---|
| Séances par semaine habituelle | Quatre nombres entiers positifs ou nuls : cours compétition/randori, technique, préparation physique, autre sport. « Compétition » désigne ici les cours, pas les tournois disputés. |
| Total hebdomadaire | Somme calculée des quatre nombres, sans champ total concurrent. Une séance mixte se compte une seule fois selon son objectif principal. Si une catégorie reste inconnue, afficher une somme partielle explicitement signalée, pas un total complet. |
| Autre sport | Nombre de séances ; nom du sport facultatif, proposé par une liste recherchable avec saisie d’un sport non répertorié possible. |
| Latéralité | Choix canoniques : Droitier, Gaucher, Ambidextre. Aucun choix implicite. |
| Club | « Colomiers » prérempli uniquement à la création d’un nouveau profil. Valeur modifiable ; ne pas écraser un club importé, restauré ou déjà saisi. Ne pas rétablir le défaut après effacement volontaire. |
| Grade | Liste ordonnée et recherchable définie ci-dessous ; choix existant sélectionné lorsque la saisie correspond sans ambiguïté à une valeur ou un alias. |
| Catégorie de poids | Nombre strictement positif, entier ou décimal, avec l’unité « kg » affichée hors du champ. Le nouveau modèle ne stocke ni lettre ni signe −/+. Ce champ n’est pas le poids mesuré et ne permet pas de reconstituer une catégorie fédérale signée. |

Liste de grades : **Blanche, Blanche-jaune, Jaune, Jaune-orange, Orange, Orange-verte, Verte, Bleue, Marron, 1er dan, 2e dan, 3e dan, 4e dan, 5e dan, 6e dan**. Chaque entrée possède un code stable distinct de son libellé ; aucune valeur par défaut ni sélection automatique fondée sur l’âge.

Exemples d’alias : `blanc` → Blanche, `bleu` ou `ceinture bleue` → Bleue, `blanche jaune` ou `blanche/jaune` → Blanche-jaune, `2eme dan` ou `2ème dan` → 2e dan. `Noire` seul reste ambigu : proposer les dans disponibles sans en choisir un. Une faute approchante donne des suggestions, pas une correction silencieuse.

La nomenclature s’appuie sur [France Judo — couleurs de ceinture](https://www.ffjudo.com/niveaux-de-ceintures) et sur les grades intermédiaires présentés par [Meuse Judo](https://meusejudo.ffjudo.com/genese-des-grades). Ces sources servent aux libellés ; elles ne constituent pas une règle d’attribution automatique d’un grade dans l’application.

#### F-01b — Expérience et interruptions

Présenter côte à côte, ou à la suite sur smartphone, **année de début du judo** et **nombre d’années réellement pratiquées**, avec une unité en années et une saisie numérique non négative acceptant les décimales. Permettre de renseigner l’une sans imposer l’autre.

Ajouter la case « J’ai interrompu le judo ». Lorsqu’elle est cochée, afficher « Nombre d’années depuis la dernière reprise ». Une réponse laissée vide reste inconnue ; zéro peut correspondre à une reprise très récente. La durée effective exclut toutes les interruptions, alors que la durée depuis la reprise décrit seulement la dernière période continue.

Sans interruption, une année de début peut proposer une durée approximative, à confirmer ; elle ne fournit pas une date précise de premier cours. En présence d’interruptions, ne convertir automatiquement ni année de début en pratique effective, ni pratique effective en année de début, sans durée d’arrêt connue. Garder les saisies indépendantes. Signaler une incohérence apparente avec l’âge ou la chronologie en tenant compte de la précision annuelle ; ne pas écraser une valeur pour la résoudre.

#### F-01c — Situation scolaire ou professionnelle

Remplacer le choix générique « Scolaire / Professionnel » par **Primaire, Collège, Lycée, Études supérieures, Travail**, sans choix implicite. Ce contexte reste dans « Mon quotidien » et sert à adapter les formulations des questions de charge et de fatigue. Il n’est pas déduit de l’âge. Une situation non couverte peut rester non renseignée, sans contraindre la personne à choisir une réponse fausse.

### F-02 — Évaluations

Les domaines recensés à la section 4 doivent être conservés de bout en bout : saisie, stockage, export, réimport et affichage. Chaque bilan doit avoir une date choisissable, une période et un identifiant stable. L’âge utilisé pour interpréter un bilan historique doit être celui à sa date.

La saisie doit distinguer absence de réponse, zéro et réponse négative. Aucun score prérempli ne doit être interprété comme une observation. Un brouillon peut être incomplet ; seules les données nécessaires à un calcul doivent conditionner ce calcul.

L’utilisateur doit pouvoir consulter, corriger et supprimer un bilan sans dupliquer involontairement la saisie précédente. Les valeurs historiques ne doivent pas devenir automatiquement les réponses d’un nouveau bilan.

Les réponses restent facultatives hors identité requise. La nouvelle précision des critères ne doit pas imposer une évaluation complète, une validation obligatoire par l’entraîneur ou le téléversement de justificatifs. Les exigences F-10 à F-13 s’appliquent aussi aux états avant/pendant/après un événement, pas uniquement au bilan trimestriel.

### F-03 — Calculs et indicateurs

Les calculs doivent partager un dictionnaire précisant type, unité, plage, sens d’évolution, données requises, formule et provenance de la règle.

| Calcul ou affichage | Comportement cible |
|---|---|
| SJFT | `(FC fin + FC à 1 min) / projections`, uniquement si toutes les entrées sont valides et les projections strictement positives ; arrondi réservé à l’affichage |
| Perte de rendement | `(première série − dernière série) / première série × 100` ; deuxième valeur manquante distincte de zéro ; amélioration traitée explicitement |
| Asymétrie de préhension | Formule actuelle : `abs(D − G) / max(D, G) × 100` ; calcul impossible si données insuffisantes ; seuil à documenter |
| Évolution | Comparaison datée avec une mesure de même protocole et unité ; pourcentage non calculé sur une référence nulle |
| Couleurs | Même règle dans formulaire, résumé, tableau de bord et export ; bornes non ambiguës ; sens favorable propre à chaque mesure |
| Donnée absente | État textuel « Non renseigné » ou « Non applicable », sans score favorable ou défavorable inventé |
| Radar | Seulement des valeurs calculables et documentées ; aucune constante de remplissage présentée comme une mesure |

Les seuils actuels sont des règles de code, pas une validation scientifique. Les repères marqués `[P]` dans le référentiel doivent être identifiés comme repères de pilotage. Les divergences entre code et référentiel doivent être arbitrées avant diffusion d’indicateurs présentés comme fiables.

Les questionnaires doivent conserver leurs réponses et, lorsque la version retenue est vérifiée, calculer leurs dimensions. Le libellé d’un axe doit correspondre aux mesures réellement utilisées. Les signaux RED-S doivent être visibles et traçables ; aucun seuil de diagnostic ne doit être déduit du simple nombre de cases cochées.

Un calcul, une couleur ou une variation doit être associé à la version de l’échelle qui lui donne son sens. Un score ordinal 0–4 ne doit pas devenir automatiquement un pourcentage de maîtrise. Ne pas appliquer aux nouvelles réponses les anciens seuils codés pour 0–10.

### F-04 — Tableau de bord et historique

Afficher le profil, la date du dernier bilan chronologique, les dimensions disponibles, les alertes contextualisées, l’évolution et les objectifs. Fournir un équivalent textuel des visualisations. Étendre progressivement le suivi aux indicateurs du tableau longitudinal du référentiel, avec unités et variations individuelles.

Le bilan doit aider à choisir deux ou trois priorités pour la période suivante. Ne pas produire de score global agrégeant santé, technique, mental et résultats.

### F-05 — Objectifs et difficultés

Renommer le groupe « Mon judo » en **« Mes objectifs »**, y compris son raccourci. Conserver ses sous-rubriques : objectifs et difficultés, système d’attaque, technique, compétitions et anxiété en compétition. Le changement de titre ne supprime aucune donnée technique.

Conserver type, description, commentaire, dates et actions. Permettre leur modification indépendamment d’une nouvelle évaluation. Rattacher les objectifs à une période et conserver l’historique utile. Proposer les types Technique, Tactique, Physique, Cardio, Mental, Grade, Santé et Résultat ; **Scolaire n’est plus proposé pour un nouvel objectif**. Les anciens objectifs scolaires restent consultables sans conversion forcée ; le suivi scolaire dans « Mon quotidien » est maintenu.

Un objectif comprend une **action**, une **échéance** et un **critère de réussite vérifiable**. Ce critère décrit le résultat attendu et son contexte : par exemple « participer à deux séances de préparation physique par semaine pendant trois semaines » ou « obtenir le grade visé lors du passage daté ». En cas de mesure numérique, conserver cible, unité et résultat observé ; une cible non numérique doit décrire un événement ou un comportement identifiable. Un objectif incomplet peut être conservé comme brouillon, mais ne peut être déclaré atteint.

Remplacer l’ancienne échelle 0–3 de maîtrise par des statuts de suivi, sans leur attribuer un score de performance :

| Statut | Définition et condition |
|---|---|
| À commencer | Aucune action réalisée n’a été renseignée. |
| En cours | Au moins une action datée est renseignée ; le critère final n’a pas été déclaré satisfait avec un résultat. |
| Atteint | Le critère est satisfait ; le résultat observé et sa date sont renseignés. La déclaration peut venir du judoka ; aucune validation extérieure obligatoire. |
| Abandonné | Décision explicite de cesser cet objectif ; motif facultatif. Ce statut ne représente pas une note plus élevée ou plus basse. |

Ne pas autoriser « Atteint » sur la seule saisie d’une note ou d’un commentaire vague. La confirmation d’un entraîneur peut être consignée lorsqu’elle existe. Le détail du résultat et du critère doit rester visible après export/réimport.

### F-06 — Planification

Conserver la saison choisie, les périodes datées, leurs objectifs et priorités physiques, techniques et mentales. Gérer les échéances avec type, nom, lieu, catégorie et importance, ainsi que les périodes de vie scolaire, professionnelle ou familiale.

Permettre ajout, édition et suppression avec contrôle des dates. Signaler les chevauchements utiles à la décision sans interdire les événements simultanés. Afficher ensemble périodes, échéances et contraintes de vie.

L’extension issue du référentiel comprend semaines, séances, récupération et articulation saison → blocs → semaines. Elle vient après la fiabilisation du socle et ne constitue pas une fonction déjà livrée.

### F-07 — Préparation mentale et événements

Conserver projet personnel, mots-clés, routines et journal d’imagerie : date, durée, qualité, contenu. Pour un événement, conserver les informations générales, résultats, détail des combats et états avant/pendant/après présents dans l’exemple.

Adapter le formulaire au type : compétition, grade, autre. Un grade doit disposer de son résultat et de ses exigences à suivre ; la définition réglementaire de ces exigences reste à vérifier. Après enregistrement, proposer une nouvelle saisie propre. La consultation détaillée, l’édition et la suppression doivent être disponibles.

### F-08 — Import/export de profil

Le Markdown est le format portable de référence. Définir une version de format et une correspondance explicite entre sections, libellés et clés internes. Préserver les anciennes variantes utiles, dont `Compétition`/`Competition`, sans confondre clé interne et libellé affiché.

Un cycle export → import doit conserver les valeurs, les types, les dates, toutes les collections et leur sens. Les cellules vides, caractères `|`, accents et textes multilignes doivent être représentables. Une valeur `6 /10` doit être lue comme 6 ; `22:00` doit rester une heure ; `Non` doit devenir un booléen faux pour un champ booléen.

Les éléments inconnus doivent être préservés ou signalés avant import, jamais perdus silencieusement. Charger dans un objet temporaire, valider, puis remplacer le profil actif. Afficher un bilan des sections reconnues et des anomalies. Aucune donnée importée ne doit être interprétée comme du HTML exécutable.

Les migrations vers les données standardisées suivent la section 7.2. Appliquer les mêmes dictionnaires aux formulaires, brouillons et fichiers ; un changement de libellé ne doit pas casser les clés historiques ni supprimer une rubrique. L’import d’un ancien score ne le transforme pas en réponse à une nouvelle échelle.

### F-09 — Import de calendriers

Définir le sous-ensemble ICS supporté : dates simples et journées entières, textes échappés et lignes repliées ; préciser le traitement des heures, fuseaux, récurrences et exceptions. Les fonctions non prises en charge doivent être signalées.

Pour CSV, gérer séparateurs, champs entre guillemets, cellules vides et formats de date documentés. Confirmer les correspondances de colonnes par aperçu. Tester la compatibilité FFJudo sur un export représentatif avant de la promettre. Un second import identique ne doit pas créer des doublons silencieux.

### F-10 — Saisie standardisée et suggestions

Cette exigence concerne tous les formulaires et leurs équivalents dans les fichiers, notamment profils, objectifs, plans, événements, techniques et types d’activités.

1. Pour une catégorie connue, employer une liste de valeurs canoniques, avec code stable, libellé affiché et alias. Utiliser des choix visibles pour une liste courte, ou une liste recherchable lorsque la longueur le justifie.
2. Pendant la frappe, proposer les valeurs existantes. Normaliser casse, accents, espaces périphériques ou répétés et séparateurs documentés. À la validation du champ, sélectionner la valeur canonique si la correspondance exacte ou l’alias est unique. Un préfixe ou une ressemblance seuls ne justifient jamais une sélection définitive.
3. Pour plusieurs correspondances, laisser l’utilisateur choisir. Pour une valeur inconnue d’une liste fermée, expliquer les possibilités sans ajouter automatiquement une variante au référentiel. Conserver la saisie comme brouillon à résoudre. Pour les listes ouvertes, comme le club ou un autre sport, permettre une valeur nouvelle sans créer de faux rapprochement.
4. Réserver le texte libre aux noms non répertoriés, commentaires, apprentissages, descriptions et actions personnelles. Ne pas réécrire ces textes au titre de la standardisation.
5. Pour une quantité, séparer le nombre de l’unité fixe et utiliser le clavier numérique adapté. Lire `8,5` et `8.5` comme la même valeur ; une unité reconnue collée avec un nombre peut être retirée selon une règle propre au champ. Ne pas extraire arbitrairement des chiffres d’un texte ambigu tel que `8 ou 9`.
6. Ne pas présélectionner une mesure ni une autoévaluation. Un défaut de confort explicitement défini, comme le club, reste modifiable et ne devient pas une règle de migration.

Les propositions doivent fonctionner au toucher et au clavier, avec libellés persistants, indication du choix courant et possibilité d’effacer. Aucun parcours supplémentaire ni nouvelle étape obligatoire n’est introduit.

### F-11 — Échelles personnelles à critères explicites

Remplacer les échelles personnelles vagues par **quatre ou cinq réponses maximum**, généralement cinq choix codés de 0 à 4. Chaque choix montre son intitulé et sa définition au moment de répondre ; les seules extrémités d’un curseur ne suffisent pas. « Non évalué », « Je ne sais pas » et « Non applicable » sont des états hors échelle, sans valeur numérique.

Chaque définition d’échelle doit contenir : identifiant, version, question, phénomène évalué, nature de l’information (mesure, observation ou ressenti), période ou séance de référence, définition de chacune des valeurs, sens d’évolution et provenance. Les choix doivent être distincts et interprétables sans deviner la différence entre « bon » et « moyen ».

| Nature de la réponse | Règle |
|---|---|
| Fréquence | Donner des nombres et une période explicite ; « souvent » seul ne suffit pas. |
| Intensité ressentie | Décrire ce que la personne perçoit ou ce que cela change dans son activité, avec des repères propres à la question. Le ressenti reste déclaré, pas présenté comme une mesure objective. |
| Exécution d’une tâche | Décrire tâche, contexte, aide éventuelle, essais et critère de réussite ; suivre F-13. |
| Mesure instrumentée | Conserver la valeur et son unité ; ne pas remplacer une mesure disponible par un jugement vague. |
| Questionnaire ou protocole standardisé identifié | Conserver questions, période et cotation de la version retenue. Ne pas convertir en 0–4 pour uniformiser l’interface. Une version reformulée ne doit pas être présentée comme l’outil validé. |

**Modèle de fréquence sur les sept derniers jours**, applicable seulement à une question exprimée en nombre de jours :

| Valeur | Définition |
|---|---|
| 0 | Aucun jour sur les sept derniers jours. |
| 1 | Un ou deux jours sur les sept derniers jours. |
| 2 | Trois ou quatre jours sur les sept derniers jours. |
| 3 | Cinq ou six jours sur les sept derniers jours. |
| 4 | Tous les jours, soit sept jours sur sept. |

Pour une question concernant des entraînements, le nombre de jours ne remplace pas le nombre de séances. Préciser les occasions réellement observées ; absence d’occasion signifie « Non applicable », pas zéro. Une intensité de stress ne se déduit pas de sa fréquence.

La refonte doit inventorier toutes les notes personnelles existantes — sommeil, coordination, qualité d’exécution, mental, charge scolaire/professionnelle et états d’événement — et fournir leur grille avant mise en service. Une échelle sans définition complète reste indisponible pour noter ou comparer ; elle ne peut pas conserver des nombres nus par défaut. Les grilles créées pour le produit sont identifiées comme repères de suivi, sans revendication de validation scientifique.

### F-12 — Hydratation décrite par des observations

Supprimer le jugement global « Bonne / Moyenne / Insuffisante ». Renseigner des observations distinctes portant sur **la dernière séance effectivement réalisée**, avec sa date si connue :

| Observation | Réponse et précision |
|---|---|
| Eau disponible pendant la séance | Oui / Non / Je ne sais pas. Décrit l’accès à de l’eau, pas la présence obligatoire d’une gourde personnelle. |
| Pauses où la personne a bu | Nombre de pauses où elle a bu et nombre total de pauses observées, entiers non négatifs ; premier nombre inférieur ou égal au second. Sans pause, noter « aucune occasion observée », sans jugement sur la consommation. |
| Soif ressentie pendant ou à la fin de la séance | Oui / Non / Je ne sais pas. |
| Soif persistante après avoir bu | Oui / Non / Je ne sais pas ; question applicable uniquement si la personne a eu soif et a bu, y compris hors des pauses comptées. |

Les comptes et réponses factuelles ne sont pas forcés dans une échelle à cinq niveaux. Conserver « inconnu » lorsque la personne ne se souvient pas et « aucune séance » lorsqu’il n’existe pas de référence. Ne pas assimiler un champ vide à zéro, ni l’absence de soif à une hydratation suffisante.

Afficher les observations et leur contexte, sans score global, seuil de volume imposé ou diagnostic automatique. L’ancienne réponse qualitative reste uniquement dans l’historique ; elle ne permet pas d’inventer ces nouvelles observations.

### F-13 — Technique : tâche, contexte et résultats observés

Les 22 domaines techniques et les huit critères de tokui-waza nécessitent des grilles adaptées. « Statique », « randori » ou « compétition » nomment un contexte ; ils ne définissent pas à eux seuls une réussite ou une maîtrise.

Pour une observation, conserver la tâche ou technique ciblée, son critère de réussite, la date, le contexte, l’aide reçue, le nombre d’essais et le nombre de réussites lorsqu’ils sont connus. Indiquer si l’observation est déclarée par le judoka ou renseignée avec l’entraîneur, sans imposer un compte ou une approbation extérieure.

Le modèle suivant décrit le **contexte d’une réussite observée**, pas une maîtrise générale. Chaque domaine doit préciser la tâche et les critères nécessaires pour l’employer :

| Valeur | Définition minimale vérifiable |
|---|---|
| 0 | La tâche a été essayée dans le contexte indiqué ; aucune réussite répondant au critère n’a été observée. |
| 1 | Au moins une réussite répond au critère avec guidage pendant l’exécution ; préciser l’aide. |
| 2 | Au moins une réussite répond au critère sans guidage, dans une situation convenue sans opposition ; préciser si elle comprend un déplacement. |
| 3 | Au moins une réussite répond au critère sans guidage, avec une opposition convenue ; décrire la résistance ou les contraintes autorisées. |
| 4 | Au moins une réussite répond au critère sans guidage en opposition libre ; préciser la situation et le résultat observé. |

« Non essayé » reste hors de l’échelle. Une réussite ponctuelle ne signifie pas que la technique est reproductible : afficher le nombre de réussites sur le nombre d’essais, lorsqu’il est disponible, et ne pas comparer deux rapports obtenus dans des contextes différents comme s’ils étaient équivalents.

Ne pas imposer cette progression à tous les domaines. Pour les ukemi, documenter la forme de chute demandée et les critères de réalisation retenus avec l’entraîneur ; pour une projection, identifier la technique, le contexte d’opposition et ce qui compte comme projection réussie ; pour une immobilisation, documenter position de départ, opposition et durée cible. Aucun seuil de réussite propre à un domaine n’est déduit de l’exemple générique.

Une grille spécifique doit définir chaque valeur effectivement proposée. Tant qu’elle n’est pas définie, permettre une observation descriptive mais pas une note arbitraire. Ne déduire automatiquement un niveau ni du grade porté, ni d’une victoire en compétition, ni d’un score moyen regroupant des tâches différentes.

## 7. Modèle de données

L’objet `D` actuel a la structure suivante ; les valeurs de formulaire restent souvent des chaînes, même lorsqu’elles représentent des nombres.

```text
D
├── profil : objet
├── attaque : objet
├── objectifs : tableau
├── difficultes : tableau
├── evaluations : tableau d’objets plats, date + trimestre + champs
├── plan
│   ├── periodes : tableau
│   ├── echeances : tableau
│   ├── vie : tableau
│   └── mental : objet
└── evenements : tableau, avec avant / pendant / apres lors de la saisie
```

Le contrat cible doit conserver ces concepts et ajouter les informations manquantes sans imposer une réécriture technique préalable :

| Entité | Données attendues |
|---|---|
| Document | Version de schéma et identifiant du profil |
| Profil | Informations personnelles, grade et latéralité canoniques, catégorie numérique, répartition hebdomadaire, expérience et interruptions |
| Attaque | Les neuf champs existants, textes et listes avec séparateurs définis |
| Évaluation | Identifiant, date, période, mesures typées, protocoles, contexte scolaire/professionnel, observations d’hydratation et réponses avec identifiant/version d’échelle |
| Objectif / difficulté | Identifiant, date, domaine/type, description, actions, commentaire et période ; pour un objectif : échéance, critère, statut de suivi, résultat et date du résultat |
| Plan | Saison explicite, périodes, échéances, calendrier de vie, préparation mentale |
| Événement | Identifiant, type stable, date, informations, résultats, combats, états et apprentissages |
| Extensions | Séances, semaines et synthèses de charge après stabilisation du socle |

Conventions cibles : nombres finis avec unité définie, dates civiles `AAAA-MM-JJ`, heures `HH:mm`, booléens explicites, valeur absente distincte de `0` et `false`. Les libellés français servent à la lecture ; les clés stables servent au stockage. Les données sources des calculs doivent être conservées pour permettre leur recalcul.

### 7.1 Contrats complémentaires à introduire

| Donnée | Type et sémantique |
|---|---|
| Référentiel de choix | Code stable, libellé français, alias explicites et version ; distinct du texte tapé pour rechercher. |
| Fréquence hebdomadaire | Quatre compteurs nommés `competition`, `technique`, `preparation_physique`, `autre_sport` ; chacun entier ≥ 0 ou inconnu. Le total se calcule, sans stocker une seconde valeur contradictoire. |
| Catégorie de poids | Nombre > 0 ou inconnu ; unité kg fixée par le schéma, sans signe de catégorie. Valeur originale importée conservée dans la trace de migration. |
| Expérience | Année de début facultative ; années de pratique effective ; interruption déclarée ; années depuis la dernière reprise si applicable ; date de référence et caractère estimé/confirmé de la durée. Une estimation ne remplace pas une donnée confirmée. |
| Situation | Code parmi Primaire, Collège, Lycée, Études supérieures, Travail, ou absence de réponse. Les libellés ne servent pas de clés internes. |
| Réponse à une échelle | Identifiant et version d’échelle, valeur, période/contexte et nature de l’information. En l’absence de valeur, conserver le motif : non renseigné, non évalué, inconnu ou non applicable. |
| Hydratation | Dernière séance/date, accès à l’eau, pauses totales et pauses avec prise de boisson, soif ressentie et persistance ; chaque observation peut être inconnue ou non applicable. |
| Objectif | Type canonique, action, échéance, critère de réussite, actions réalisées datées, statut parmi `a_commencer`, `en_cours`, `atteint`, `abandonne`, résultat et date du résultat lorsque atteint. |
| Observation technique | Domaine/tâche, grille et version, critères, date/contexte, aide, essais/réussites connus et origine de l’observation. Note seulement si les critères de la grille sont renseignés. |

La version de format du document et celle de chaque échelle sont distinctes : l’une permet la lecture du fichier, l’autre l’interprétation d’une réponse. Conserver les données historiques avec leur version, même lorsque le formulaire propose désormais une autre échelle. Aucun service distant ni nouvelle API n’est nécessaire à ces évolutions.

### 7.2 Compatibilité et migration

| Donnée ancienne | Traitement requis |
|---|---|
| Grade ou latéralité avec une variante reconnue | Résoudre via les alias explicites ; garder une trace du texte d’origine lors d’une migration. Une valeur ambiguë ou hors liste reste historique et signalée comme à préciser. |
| Club importé ou restauré | Conserver la valeur ; ne jamais remplacer par Colomiers au titre d’un défaut. |
| Total d’entraînements seul | Conserver le total historique ; laisser les quatre nouveaux compteurs inconnus. Ne répartir ni uniformément ni selon le profil supposé. |
| `-50kg`, `+73 kg` ou équivalent non ambigu | Produire respectivement 50 et 73 pour le nouveau champ, conformément au choix de nombre seul ; conserver la chaîne signée originale dans l’historique/trace. Expliquer dans le bilan d’import que la nouvelle catégorie ne distingue plus les signes. |
| Catégorie contenant plusieurs nombres ou une plage | Ne pas deviner un nombre unique. Conserver la source et demander une précision lors de la reprise de ce champ. |
| Année de début seule | La conserver ; ne pas inventer des années effectives, une absence d’interruption ou une date de reprise. Une éventuelle estimation est présentée séparément et confirmée par la personne. |
| Situation `Scolaire` | Conserver l’ancienne valeur ; nouvelle situation à préciser. Ne pas choisir Primaire, Collège ou Lycée selon l’âge. |
| Situation `Professionnel` | Correspondance explicite vers Travail, avec conservation de l’origine. |
| Objectif de type Scolaire | Conserver type et contenu dans l’historique, consultables et exportables ; ne plus proposer ce type pour la création d’un nouvel objectif. |
| Ancien statut numérique d’objectif | Conserver code et libellé historiques. Ne pas déduire « Atteint » d’un ancien niveau de maîtrise ; afficher « Statut à préciser » comme état de migration, hors des quatre statuts métier. |
| Note personnelle 0–10, ancienne note technique ou autre grille remplacée | Conserver valeur, échelle et contexte historiques. Aucune conversion proportionnelle, aucun arrondi vers 0–4 et aucun préremplissage d’une nouvelle réponse. |
| Hydratation Bonne/Moyenne/Insuffisante | Conserver uniquement comme ancienne réponse qualitative ; nouvelles observations inconnues. |

La migration doit couvrir aussi le brouillon local existant, actuellement au format `judo-plan-brouillon-v1`. Une nouvelle version ne doit pas supprimer ou rendre illisible le brouillon antérieur. Préparer et valider la transformation avant de remplacer les données ; toute information sans équivalent reste accessible dans l’historique ou un compte rendu de migration exportable.

Les vues longitudinales séparent les versions incompatibles et expliquent le changement de grille. Elles peuvent afficher côte à côte les valeurs originales, sans calculer de variation numérique entre elles. Un export/réimport doit préserver à la fois les nouvelles données et les informations historiques conservées.

## 8. Exigences transversales

| ID | Exigence |
|---|---|
| N-01 | Rester utilisable par ouverture locale et hébergement statique ; aucune ressource réseau nécessaire au parcours principal |
| N-02 | Indiquer clairement « modifié », « en mémoire » et « exporté » ; protéger les brouillons contre les pertes prévisibles et documenter les limites du stockage local retenu |
| N-03 | Traiter textes saisis et fichiers comme des données ; construire les rendus sans insertion HTML non échappée |
| N-04 | Informer de la présence de données personnelles et sensibles dans le fichier exporté ; ne pas ajouter de transmission distante implicite |
| N-05 | Permettre navigation clavier, labels associés, focus visible, messages annoncés, états compréhensibles sans couleur et lecture mobile |
| N-06 | Séparer suffisamment calculs, conversion Markdown et rendu pour tester les comportements critiques ; un livrable HTML unique peut être conservé |
| N-07 | Documenter provenance, population concernée, version et statut des règles ; distinguer publication scientifique, règle fédérale et choix de pilotage |
| N-08 | Faire correspondre README, exemple, spécifications et comportement réel ; définir la licence avant diffusion sous licence explicite |
| N-09 | Appliquer les mêmes choix canoniques, alias et unités à tous les formulaires, imports, exports et brouillons ; distinguer listes fermées et textes ouverts selon F-10 |
| N-10 | Associer chaque valeur d’échelle à une définition visible, un contexte et une version ; distinguer observation, mesure et ressenti ; ne pas afficher de score sans grille explicite |
| N-11 | Préserver la saisie rapide sur une page : rubriques libres d’accès, réponses facultatives, suggestions tactiles et clavier, absence de justificatif ou de validation extérieure obligatoire |
| N-12 | Conserver les données historiques lors des changements de vocabulaire, type ou échelle ; ne comparer que des mesures ou réponses compatibles |

## 9. Recette cible

Les scénarios ci-dessous décrivent des critères futurs, pas des tests déjà réussis.

| ID | Scénario | Résultat attendu |
|---|---|---|
| R-01 | Importer l’exemple complet | Sommeil à 8,5 h, stress historique à 6/10, confiance historique à 5/10, coucher à 22:00 ; toutes les sections conservées ; aucune transformation en nouveaux scores 0–4 |
| R-02 | Exporter puis réimporter deux fois un profil complet | Égalité sémantique des données ; compétition, états, combats, saison et imagerie conservés |
| R-03 | Importer texte invalide, fichier tronqué ou version inconnue | Erreur compréhensible, profil actif intact ; incompatibilités signalées |
| R-04 | Importer textes contenant guillemets, chevrons, barres verticales, lignes multiples et cellules vides | Contenu fidèle, colonnes stables, aucun code exécuté |
| R-05 | Enregistrer un bilan partiel puis un bilan avec zéros explicites | Absence, zéro et faux restent distincts ; aucun indicateur inventé |
| R-06 | Tester les calculs et leurs bornes | SJFT pour 185, 148 et 24 : 13,88 à l’affichage ; division par zéro évitée ; couleurs identiques dans les vues |
| R-07 | Importer puis créer un nouveau bilan | Identité préremplie, objectifs et difficultés conservés ; mesures du bilan précédent non réutilisées sans action explicite |
| R-08 | Modifier naissance/sexe ; consulter un bilan ancien ; saisir en janvier ou février | Champs applicables corrects, âge historique et rattachement de période cohérents |
| R-09 | Modifier un plan, changer d’onglet, recharger | Brouillon conservé selon la stratégie retenue ; statut de sauvegarde exact |
| R-10 | Importer deux fois CSV/ICS avec dates, guillemets et lignes repliées | Données correctement interprétées ou limitation expliquée ; doublons maîtrisés |
| R-11 | Enregistrer deux événements successifs de types différents | Données séparées, second formulaire propre, édition sans perte |
| R-12 | Parcours complet local et statique, hors ligne, mobile et clavier | Création → évaluation → plan → événement → export → réimport réalisable ; aucune erreur JavaScript bloquante |
| R-13 | Saisir variantes, préfixes et inconnues | `bleu`, `BLEUE` et `ceinture bleue` → Bleue ; `blanche jaune` → Blanche-jaune ; `2eme dan` → 2e dan ; `Noire` ou un préfixe ambigu ne sélectionne aucun dan ; aucun doublon de référentiel créé |
| R-14 | Créer, importer puis modifier un profil | Colomiers prérempli seulement à la création, club importé conservé, effacement respecté ; Ambidextre sélectionnable ; cinq situations proposées sans choix selon l’âge |
| R-15 | Renseigner la semaine habituelle | 2 séances compétition + 1 technique + 1 préparation physique + 0 autre = 4 ; catégorie vide signalée comme inconnue ; séance mixte comptée une seule fois ; ancien total non réparti arbitrairement |
| R-16 | Saisir nombre, unité et ancienne catégorie signée | `50`, `50 kg` collé et ancien `-50kg` deviennent 50 selon les règles du champ ; `73,5` et `73.5` sont équivalents ; source signée conservée ; `50 ou 55` non réduit automatiquement à un nombre |
| R-17 | Décrire début, pratique et reprise | Pour un début en 2016, six années réellement pratiquées et deux depuis la dernière reprise, conserver les trois informations distinctes ; aucune durée effective déduite des seules dates ; année seule et durée seule acceptées |
| R-18 | Examiner chaque nouvelle échelle personnelle | Au plus cinq valeurs, chacune définie et contextualisée ; pour le modèle sur sept jours : 0 jour → 0, 2 → 1, 4 → 2, 6 → 3, 7 → 4 ; non évalué distinct de zéro ; questionnaire identifié inchangé |
| R-19 | Renseigner hydratation et absence de séance | Deux pauses avec prise de boisson sur trois conservées comme 2/3 ; quatre sur trois refusé ; soif inconnue et absence de séance explicites ; aucun score Bonne/Moyenne ni diagnostic produit |
| R-20 | Créer et suivre un objectif | Groupe nommé Mes objectifs ; Scolaire absent des nouveaux types mais présent dans l’historique ; Atteint refusé sans critère, résultat et date ; Abandonné reste une décision sans note de performance |
| R-21 | Évaluer technique, contexte et résultats | Non essayé distinct de 0 réussite ; 2 réussites sur 5 avec aide distinguées de 2 sur 5 sans aide ; opposition décrite ; grille spécifique nécessaire ; aucune note déduite de la ceinture ou d’une victoire |
| R-22 | Migrer anciennes notes, statuts et contexte | Stress 8/10 conservé comme ancien score et non converti en 4/4 ; ancien statut 2 ne devient pas Atteint ; Scolaire ne devient pas Collège ; changements d’échelle signalés dans l’historique |
| R-23 | Restaurer un ancien brouillon puis exporter/réimporter | Nouvelles données typées, versions d’échelle et sources historiques conservées ; aucune suppression de brouillon avant validation de migration ; calculs et suggestions cohérents avec les formulaires |

## 10. Ordre de livraison

1. **Socle fiable** : contrat de données, aller-retour Markdown, import sûr, neutralisation HTML, absence de réponses et protection contre la perte de saisie.
2. **Saisies et suivi cohérents** : dictionnaires et migrations, profil détaillé, grilles explicites, objectifs vérifiables, puis calculs, historique, événements, calendrier et accessibilité. Une nouvelle grille et ses règles d’interprétation doivent être livrées ensemble.
3. **Extension du référentiel** : suivi quotidien/hebdomadaire, semaines et séances, préparation des grades, questionnaires complémentaires et aide à la planification.

Les choix actés sont : séances compétition/randori plutôt que tournois dans la fréquence hebdomadaire ; catégorie de poids numérique sans signe ; pratique effective hors interruptions ; échelles personnelles à quatre ou cinq réponses définies ; conservation des questionnaires identifiés ; suivi scolaire maintenu hors des nouveaux types d’objectifs.

Restent à documenter dans les travaux d’implémentation : les grilles propres à chaque mesure et domaine technique, la version des questionnaires et leurs conditions d’utilisation, les règles adaptées aux adultes, le sous-ensemble ICS et la licence. Les exigences de complétude des grilles sont définies en F-11 et F-13 ; elles ne sont pas remplacées par des seuils inventés ou une conversion des anciennes notes. Les limites du brouillon local et sa migration doivent être testées lors de l’évolution du format.
