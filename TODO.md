# TODO — Judo Plan d'Entraînement

> **Audit réalisé le 27 septembre 2026** — Comparaison du code `index.html` (117 Ko, 1835 lignes) avec `SPECIFICATIONS.md`, le référentiel scientifique et les documents de conception.
>
> Voir [SPECIFICATIONS.md](SPECIFICATIONS.md) pour les constats `AUD-*`, les exigences `F-*`/`N-*` et les scénarios de recette `R-*`.

Ce fichier constitue un backlog : les cases non cochées ne sont pas des corrections déjà réalisées. **P0** protège les données et leur interprétation ; **P1** complète la fiabilité fonctionnelle ; **P2** étend le produit selon le référentiel.

---

## État du projet

L'application a évolué depuis la conception initiale (PWA plan d'entraînement avec périodisation) vers un **outil de suivi et d'évaluation trimestriel** du judoka. Le fichier `index.html` actuel est un fichier unique standalone avec thème sombre, quatre onglets (Mon bilan, Évaluation, Mon plan, Événement), import/export Markdown et brouillon local.

### Changement de périmètre par rapport à la conception initiale

L'ancienne architecture (profil → génération automatique d'un plan périodisé → vue semaine détaillée → suivi RPE) a été **remplacée** par :
- un formulaire d'évaluation sur une page (23 rubriques) ;
- un plan saisi manuellement (périodes, échéances, vie personnelle, préparation mentale) ;
- un formulaire d'événement (compétition, grade, autre) ;
- un tableau de bord avec radar et indicateurs.

---

## Spécifications — standardisation et critères observables

- [x] Généraliser les remarques utilisateur en exigences de saisie canonique, suggestions, unités et valeurs absentes (F-10, N-09).
- [x] Définir le profil cible : quatre types de séances, Ambidextre, Colomiers par défaut, liste de grades, catégorie numérique, pratique effective/interruption/reprise et cinq situations scolaires/professionnelles (F-01).
- [x] Définir les échelles personnelles à quatre ou cinq choix, les observations d'hydratation, les statuts factuels d'objectifs et les exigences de grilles techniques (F-05, F-11 à F-13).
- [x] Documenter la compatibilité des anciennes données, le modèle cible et les scénarios R-13 à R-23.

> Ces cases concernent la documentation. Les modifications correspondantes de l'application restent à réaliser dans P1-13 à P1-19 ci-dessous.

## Ergonomie — mise à jour du 27 septembre 2026

- [x] Remplacer l'assistant par une évaluation entièrement visible, avec raccourcis par thème.
- [x] Remplacer les fenêtres de saisie du plan par des champs directement modifiables et une annulation de suppression.
- [x] Regrouper l'événement sur une page et adapter les résultats au type choisi.
- [x] Agrandir les commandes et les champs pour le smartphone ; placer la navigation principale en bas.
- [x] Ajouter la reprise automatique du brouillon sur le même navigateur, avec indication des limites de stockage.
- [x] Vérifier les trois formulaires dans Chromium aux largeurs 320, 390, 768 et 1 440 pixels.

> Sources et détail de la recette : [Ergonomie mobile](docs/ergonomie_mobile.md).

## Audit et documentation réalisés

- [x] Examiner les cinq fichiers de contenu et la structure du projet.
- [x] Vérifier la syntaxe JavaScript et exécuter des contrôles ciblés dans Node.js, avec DOM simulé.
- [x] Reproduire les corruptions de nombres, la perte de compétition au réimport et l'acceptation d'un profil invalide.
- [x] Vérifier les faux états favorables sur données absentes et les bornes anaérobies incohérentes.
- [x] Rédiger les spécifications distinguant existant, défauts et cible.
- [x] Établir les tâches, dépendances et critères d'acceptation ci-dessous.
- [x] Choisir et appliquer la licence MIT.

---

## P0 — Fiabiliser le socle

- [ ] **P0-01 — Définir le contrat de données et les correspondances Markdown.** Établir un dictionnaire partagé : clé stable, section, libellé, anciens libellés, type, unité, valeurs admises et caractère facultatif. Intégrer le modèle cible de la section 7.1, les alias de F-10 et les versions d'échelles. Versionner le format et prévoir les migrations de l'exemple et des exports actuels selon la section 7.2. Références : AUD-01/02/03/17, F-08/10, N-09/12.
  **Acceptation :** chaque champ saisi et chaque rubrique de l'exemple a une représentation définie ; zéro, faux, absence et non-applicabilité sont distingués ; les champs inconnus ont une politique explicite.

- [ ] **P0-02 — Corriger la lecture des évaluations Markdown.** Utiliser section et libellé pour retrouver la clé ; lire les nombres avec leurs unités sans concaténer les dénominateurs ; préserver heures, textes et booléens. Supprimer les collisions de libellés et le décalage des cellules vides. Dépend de P0-01. Références : AUD-01/05, F-08.
  **Acceptation :** R-01 et les cas de R-04 passent ; `6 /10` donne 6, `22:00` reste `22:00`, `Non` donne faux ; la durée de sommeil alimente `sommeil_duree`.

- [ ] **P0-03 — Rendre l'export et le réimport exhaustifs.** Ajouter les catégories omises : souplesse, explosivité judo, aérobie, anaérobie, nutrition, RED-S, SAS-2, école, blessures, technique/tokui-waza et compétitions trimestrielles. Conserver saison, journal d'imagerie, combats et états d'événement, ainsi que les versions d'échelles et données historiques lors de l'introduction du modèle cible. Unifier les types internes et accepter `Competition`/`Compétition`. Échapper les caractères Markdown et textes multilignes. Dépend de P0-01/02. Références : AUD-02/03, F-07/08.
  **Acceptation :** R-02 réussit sur l'exemple et un profil synthétique couvrant chaque champ ; comparaison des valeurs et collections après deux cycles, sans perte ni inflation des scores.

- [ ] **P0-04 — Valider les imports avant remplacement du profil.** Parser dans un objet temporaire, contrôler format et types, présenter erreurs et sections reconnues, puis seulement remplacer l'état actif. Gérer erreur de lecture, fichier vide, version incompatible et modifications en cours. Harmoniser import par bouton et glisser-déposer. Dépend de P0-01/02. Références : AUD-05, F-01/08.
  **Acceptation :** R-03 passe ; un simple texte ne devient pas un profil valide et ne modifie aucune donnée active.

- [ ] **P0-05 — Neutraliser les injections HTML dans tous les rendus.** Remplacer les interpolations de données dans `innerHTML` et les attributs par des affectations DOM sûres ou un échappement adapté au contexte. Couvrir profil, objectifs, difficultés, périodes, échéances, évaluations et événements, quelle que soit la source des données. Références : AUD-06, N-03.
  **Acceptation :** test navigateur avec marqueur inoffensif : une chaîne contenant une balise et un gestionnaire d'événement est affichée comme texte, sans exécution ; les guillemets ne modifient pas les attributs.

- [ ] **P0-06 — Distinguer les réponses absentes des valeurs mesurées.** Retirer les réponses implicites des curseurs et questionnaires, ou exiger une confirmation explicite de la valeur. Représenter les inconnues dans les calculs et supprimer les valeurs radar fictives. Dépend de P0-01. Références : AUD-07/11, F-02/03/04.
  **Acceptation :** R-05 passe ; un bilan vierge ne produit ni mobilité « OK », ni préhension à zéro présentée favorablement, ni axes à 50 % inventés. Les curseurs ont une valeur initiale cohérente avec leur plage lorsqu'ils sont activés.

- [ ] **P0-07 — Protéger la saisie et clarifier la sauvegarde.** Suivre les modifications non exportées, conserver les brouillons lors des changements d'onglet et distinguer enregistrement en mémoire et export. Choisir/documenter une récupération locale compatible avec l'ouverture directe du HTML, en gérant stockage indisponible et restauration. Protéger remplacement de profil et fermeture lorsque le navigateur le permet. Références : AUD-04, N-02/04.
  **Acceptation :** R-09 passe ; le statut affiché est exact, une routine mentale ne disparaît pas en changeant d'onglet et les limites de récupération sont expliquées dans l'application et le README.

---

## P1 — Rendre les parcours et indicateurs cohérents

- [ ] **P1-01 — Synchroniser profil importé et formulaires.** Après import, reconstruire/préremplir les informations de profil, attaque, objectifs et difficultés sans réutiliser les mesures historiques. Réagir immédiatement à naissance/sexe ; gérer les champs non applicables et permettre d'effacer une ancienne valeur facultative. Dépend de P0-04/06. Références : AUD-08, F-01/02/05.
  **Acceptation :** R-07 et la partie adaptation de R-08 passent ; finir une nouvelle évaluation conserve les objectifs importés et n'injecte pas ceux d'un ancien profil.

- [ ] **P1-02 — Ajouter une validation effective des saisies.** Contrôler les champs requis lors de l'enregistrement, les bornes propres à chaque mesure, les dates civiles, début/fin et la cohérence victoires/défaites/combats. Appliquer aussi la validation aux données importées. Préserver les brouillons incomplets et les valeurs signées légitimes, notamment certains tests de souplesse. Dépend de P0-01/06. Références : AUD-09, F-01/02/06/07.
  **Acceptation :** messages près des champs avec focus adapté ; absence d'identité ou date impossible signalée ; aucune donnée incohérente acceptée silencieusement ; bilan partiel possible avec calculs incomplets explicités.

- [ ] **P1-03 — Centraliser calculs, couleurs et bornes.** Définir le sens favorable de chaque variable ; partager les règles entre commandes de saisie, résumé, alertes et radar. Résoudre les divergences à 5 % et 10 % de perte, gérer divisions par zéro, améliorations et entrées incomplètes. Documenter le choix pour les différences code/référentiel, notamment sommeil. Associer chaque règle à sa version d'échelle : les seuils 0–10 ne s'appliquent pas aux nouvelles réponses 0–4. Dépend de P0-06 et P1-04 ; intégrer les grilles livrées par P1-15/18. Références : AUD-10/11/12, F-03/11/13.
  **Acceptation :** R-06 passe ; confiance élevée et fatigue élevée n'ont pas la même interprétation ; un même jeu de données donne la même couleur dans chaque vue ; aucune variation calculée entre échelles incompatibles.

- [ ] **P1-04 — Vérifier et tracer les règles métier et sources.** Relier chaque seuil à sa source et population, distinguer repères `[P]` et résultats publiés, vérifier l'adéquation des références bibliographiques et fédérales. Expliquer l'adaptation adulte/adolescent et les conditions d'applicabilité des domaines techniques. Ne pas présenter les volumes de pôles comme prescriptions universelles. Références : AUD-12/17, N-07.
  **Acceptation :** registre des règles avec source, date de vérification, justification et statut ; règles non vérifiées explicitement identifiées ; aucune promesse générale de validation scientifique non étayée.

- [ ] **P1-05 — Compléter SAS-2 et suivi des signaux RED-S.** Vérifier version française, items, cotation et conditions d'utilisation avant de revendiquer un questionnaire validé. Calculer les dimensions retenues à partir de réponses complètes et les conserver. Afficher les signaux RED-S sans seuil diagnostique inventé ; corriger l'axe « Stress/Anxiété » actuellement basé sur la seule fatigue mentale. Dépend de P0-03/06 et P1-04. Références : AUD-12, F-03.
  **Acceptation :** scores explicables et exportables ; réponse manquante visible ; signaux sélectionnés retrouvés dans le bilan ; ancien message « 3 signaux » justifié ou remplacé.

- [ ] **P1-06 — Fiabiliser dates, périodes et historique.** Choisir la date d'évaluation, calculer l'âge à cette date, rattacher janvier/février au bon bloc de décembre, préserver la saison importée et trier les évaluations. Éviter le décalage UTC pour les dates civiles saisies localement. Ajouter consultation, édition et suppression d'un bilan et empêcher les doublons involontaires. Dépend de P0-01/03. Références : AUD-14/15, F-02/04.
  **Acceptation :** R-08 passe avec horloge contrôlée ; le dernier bilan est choisi chronologiquement ; un bilan corrigé conserve son identité sans ajout d'une copie.

- [ ] **P1-07 — Compléter le tableau de bord longitudinal.** Afficher la synthèse par domaines sur le tableau de bord, les données manquantes et les dates de mesure. Étendre les neuf indicateurs actuels selon le référentiel, calculer les variations comparables et fournir une alternative textuelle au radar. Dépend de P0-06 et P1-03/06. Références : AUD-11, F-04.
  **Acceptation :** deux bilans permettent de retrouver valeurs, unités et évolutions ; aucune comparaison entre protocoles incompatibles ; aucune note globale du judoka.

- [ ] **P1-08 — Permettre l'édition complète des objectifs et du plan.** Compléter les formulaires directs déjà livrés : saison, objectifs de période, commentaires d'objectif et dates de difficulté. Permettre l'édition indépendante d'une évaluation et conserver le rattachement aux périodes. Appliquer aux objectifs les critères et statuts de F-05, détaillés dans P1-17. Références : F-05/06, AUD-04/17.
  **Acceptation :** création, modification et suppression des éléments sans ressaisie intégrale ; critères et résultats conservés ; périodes et contraintes de vie visibles dans le calendrier ; annulation sans mutation partielle.

- [ ] **P1-09 — Compléter les événements et la préparation mentale.** Implémenter `updateEventForm()`, les détails des combats, la consultation, l'édition, la suppression et le journal d'imagerie. Réinitialiser cases, curseurs et sommeil de la veille après enregistrement. Dépend de P0-03/06 et P1-02. Références : AUD-03/15, F-07.
  **Acceptation :** R-11 passe ; un grade n'impose pas de résultats de combat ; états avant/pendant/après et imagerie survivent à l'export/réimport.

- [ ] **P1-10 — Fiabiliser les imports CSV et ICS.** Définir le sous-ensemble supporté, gérer échappements et champs complexes, contrôler les dates, proposer aperçu et correspondance des colonnes. Définir une stratégie de doublons et signaler récurrences ou fuseaux non pris en charge. Ajouter un export FFJudo représentatif comme fixture si la compatibilité est maintenue. Dépend de P0-04/05. Références : AUD-13, F-09.
  **Acceptation :** R-10 passe, notamment CSV avec virgule/point-virgule dans un nom, cellule vide, date française et ICS replié ; un import invalide ne dégrade pas le plan.

- [ ] **P1-11 — Corriger l'accessibilité et vérifier le mobile.** Associer labels et champs, rendre les étapes accessibles au clavier, gérer le focus et annoncer erreurs/messages. Ajouter textes aux états colorés et équivalent au Canvas ; corriger l'attribut `class` dupliqué. Références : AUD-16, N-05.
  **Acceptation :** parcours R-12 au clavier, à largeur mobile et avec zoom ; les commandes restent utilisables et la couleur n'est jamais la seule information.

- [ ] **P1-12 — Aligner livraison hors ligne, documentation et licence.** Retirer toute requête vers des ressources distantes ou intégrer les polices nécessaires ; décrire exactement les étapes, la sauvegarde et les limites d'import. Ajouter les liens vers spécifications et référentiel au README. Références : AUD-17, N-01/08.
  **Acceptation :** parcours principal sans réseau, aucune ressource distante nécessaire ; README conforme aux fonctions effectivement testées ; licence explicite.

- [ ] **P1-13 — Standardiser les choix et la saisie assistée dans tous les formulaires.** Introduire des dictionnaires partagés de codes, libellés, alias et versions. Distinguer listes fermées et champs ouverts ; suggestions pendant la frappe, sélection sur correspondance exacte unique, résolution explicite des ambiguïtés. Séparer nombres et unités, accepter virgule décimale et unités collées reconnues, préserver les textes personnels. Dépend de P0-01. Références : F-10, N-09/11.
  **Acceptation :** R-13 et R-16 passent ; toucher et clavier utilisables ; `bleu` et `ceinture bleue` désignent la même valeur ; `Noire` ne choisit aucun dan ; `8 ou 9` n'est pas transformé en nombre arbitraire.

- [ ] **P1-14 — Détailler le profil sportif et le contexte de vie.** Mettre en place les quatre nombres de séances et le total complet/partiel, Ambidextre, Colomiers comme défaut limité à la création, les 15 grades définis en F-01, la catégorie numérique sans signe, l'expérience effective et les champs interruption/reprise. Proposer Primaire, Collège, Lycée, Études supérieures et Travail sans déduction par âge. Ne pas compter une séance mixte deux fois. Dépend de P0-01 et P1-13. Références : F-01a/b/c, section 7.1.
  **Acceptation :** R-14 à R-17 passent ; import et effacement du club respectés ; aucune durée effective calculée comme si les interruptions n'existaient pas ; un total ancien ne remplit pas les quatre compteurs.

- [ ] **P1-15 — Définir puis afficher les échelles personnelles simplifiées.** Inventorier les notes du bilan et des événements ; produire pour chacune identifiant/version, question, contexte, nature de la réponse et définition de chaque valeur. Proposer quatre ou cinq choix maximum, avec légendes visibles et état non évalué distinct de zéro. Employer des nombres de jours uniquement pour une fréquence sur une période de sept jours. Conserver cotation et formulation des questionnaires ou protocoles identifiés. Dépend de P0-01/06 et P1-04/13. Références : F-02/03/11, N-10.
  **Acceptation :** R-18 passe pour toutes les grilles introduites ; aucune note personnelle sans définition complète, aucun ressenti présenté comme mesure objective, aucune conversion des notes anciennes.

- [ ] **P1-16 — Remplacer l'hydratation qualitative par des observations.** Suivre la dernière séance, l'accès à l'eau, les pauses totales et celles où la personne a bu, la soif et sa persistance après avoir bu. Prévoir inconnu, absence de séance et non-applicabilité ; garder les observations séparées. Dépend de P0-01 et P1-13. Références : F-12, section 7.1.
  **Acceptation :** R-19 passe ; aucune inférence de Bonne/Moyenne/Insuffisante ni de diagnostic ; un compte supérieur au nombre de pauses est refusé ; une absence de souvenir ne devient pas zéro.

- [ ] **P1-17 — Introduire Mes objectifs et les statuts factuels.** Renommer le groupe et son raccourci en conservant les sous-rubriques. Retirer Scolaire des nouveaux types sans supprimer les anciens objectifs ni le contexte scolaire du quotidien. Ajouter action, échéance, critère de réussite, actions réalisées datées, résultat et date. Proposer À commencer, En cours, Atteint et Abandonné avec les conditions de F-05. Dépend de P0-01 et P1-13 ; coordonner l'édition avec P1-08. Références : F-05, N-11.
  **Acceptation :** R-20 passe ; Atteint nécessite un critère satisfait et un résultat daté ; pas de justificatif ou approbation extérieure obligatoire ; un objectif brouillon reste possible ; les statuts ne produisent pas de pourcentage de maîtrise.

- [ ] **P1-18 — Définir les grilles techniques par tâche et contexte.** Décliner F-13 sur les 22 domaines et huit critères de tokui-waza : tâche, critère de réussite, contexte, aide, essais/réussites et définition de chaque valeur proposée. Distinguer guidage, autonomie sans opposition, opposition convenue et libre lorsque cela convient au domaine. Prévoir une observation descriptive sans note lorsque la grille manque. Dépend de P0-01 et P1-04/13/15. Références : F-13, N-10.
  **Acceptation :** R-21 passe ; aucune grille de projection appliquée automatiquement aux chutes ; une réussite ponctuelle est distinguée de sa répétition ; aucun niveau déduit de la ceinture ou d'une victoire seule.

- [ ] **P1-19 — Migrer sans perte les profils, historiques et brouillons.** Implémenter les règles de la section 7.2 pour les anciennes fréquences, catégories signées, situations, objectifs, notes et hydratation. Conserver les valeurs sources et versions ; faire préciser les données ambiguës sans empêcher de consulter l'historique. Inclure le brouillon `judo-plan-brouillon-v1`. Dépend de P0-02/03/04 et des contrats introduits par P1-13 à P1-18. Références : F-08, N-12.
  **Acceptation :** R-22 et R-23 passent ; stress 8/10 jamais converti en 4/4 ; ancien statut 2 jamais assimilé à Atteint ; aucun ancien objectif Scolaire perdu ; catégorie signée source consultable après normalisation et export/réimport.

> Les nouvelles interfaces de P1-13 à P1-18 ne sont livrées qu'avec les migrations et l'interprétation compatibles correspondantes. La définition d'une grille ne vaut pas validation scientifique ; les versions et les limites doivent être documentées.

---

## P2 — Étendre le produit selon le référentiel

- [ ] **P2-01 — Journal quotidien et de séances.** Ajouter sommeil, fatigue, douleur, motivation, stress scolaire, durée, RPE et données utiles des séquences judo ; produire un bilan hebdomadaire. Dépend du modèle versionné et du socle P0. Références : référentiel sections 11 et 27, F-06.
  **Acceptation :** séances datées exportables, synthèses expliquées et conservation de l'historique ; aucune formule de charge non documentée.

- [ ] **P2-02 — Planification de semaines et récupération.** Relier saison, blocs, semaines, séances, compétitions prioritaires et contraintes scolaires/professionnelles. Prévoir des semaines de récupération modifiables ; rendre explicites les propositions d'adaptation. Dépend de P1-08 et P2-01. Références : référentiel sections 5–8 et 57–60.
  **Acceptation :** une semaine réelle du club peut être construite et modifiée autour d'un examen ou d'une compétition sans appliquer automatiquement les volumes des pôles.

- [ ] **P2-03 — Compléter les observations techniques et physiques.** Ajouter coordination/dissociation, essais de préhension, observations détaillées de kumikata/ne-waza et protocoles comparables. Dépend de P1-04/07. Références : référentiel sections 18, 20, 46–48.
  **Acceptation :** chaque nouvelle observation possède type, unité/échelle, protocole, contexte et correspondance Markdown testée.

- [ ] **P2-04 — Suivi structuré des grades.** Ajouter programme technique, exigences/UV, kata et progression lorsque pertinents ; vérifier les règles applicables avant de les intégrer. Dépend de P1-04/09. Référence : référentiel section 49.
  **Acceptation :** un objectif de grade peut être suivi indépendamment des compétitions, avec source et date des exigences affichées.

- [ ] **P2-05 — Questionnaires complémentaires et pratiques mentales.** Étudier ASSQ, CSAI-2R et suivi avant/après respiration ou recentrage ; vérifier utilité, version et conditions d'utilisation avant ajout. Dépend de P1-04/05/09. Références : référentiel sections 13 et 35–40.
  **Acceptation :** seuls les outils retenus et documentés sont proposés ; réponses, dimensions et historique sont conservés sans diagnostic automatique.

---

## Vérification et conditions de clôture

- [ ] **V-01 — Installer une base de tests ciblés des comportements critiques.** Priorité au contrat Markdown, aux migrations, imports invalides, données absentes, calculs, dates et collections. Employer des fixtures couvrant les données effectivement perdues, les alias, les totaux partiels, les interruptions, les versions d'échelles et les critères d'objectifs/techniques. À réaliser avec P0-01/02/03 et étendre avec P1-13 à P1-19.
  **Acceptation :** les tests reproduisent les défauts relevés avant correction et passent après correction ; les commandes sont documentées et exécutables sans modifier des profils personnels.

- [ ] **V-02 — Réaliser une recette navigateur complète.** Exécuter R-01 à R-23 sur ouverture locale et hébergement statique, au moins sur Chromium et Firefox, avec une vérification mobile. Couvrir téléchargement, réimport, injection HTML inoffensive, stockage indisponible, changements d'onglet, suggestions au toucher/clavier et définitions visibles de chaque valeur. À réaliser après les corrections concernées.
  **Acceptation :** résultat, environnement et limitations de chaque scénario consignés ; aucun scénario non exécuté présenté comme réussi.

- [ ] **V-03 — Préparer une livraison reproductible.** Documenter commandes de vérification et procédure de publication statique ; ajouter une intégration continue lorsque le dépôt et son hébergement sont disponibles.
  **Acceptation :** les tests critiques précèdent la livraison ; la publication reste une étape distincte de cette tâche documentaire.

> Une tâche n'est close qu'après satisfaction de son critère d'acceptation et mise à jour de la documentation concernée. Les P2 restent des extensions ; ils ne doivent pas retarder la correction des pertes de données déjà reproduites.

---

## Résumé de l'audit — État fonctionnel au 27 septembre 2026

### ✅ Ce qui fonctionne

| Domaine | État |
|---------|------|
| Structure fichier unique HTML standalone | Fonctionnel, 117 Ko (~500 Ko max) |
| Licence MIT | Choisie et appliquée |
| Quatre onglets (Bilan, Évaluation, Plan, Événement) | Navigation fonctionnelle |
| Formulaire d'évaluation sur une page (23 rubriques) | Saisie opérationnelle, rubriques conditionnelles selon âge/sexe |
| Raccourcis de navigation dans l'évaluation | 6 ancres thématiques |
| Sélecteurs visuels et curseurs | Fonctionnels, curseurs non préremplis |
| Import de profil Markdown (.md) | Fonctionnel (avec pertes, cf. P0-02/03) |
| Export de profil Markdown | Fonctionnel (partiel, cf. P0-03) |
| Brouillon local (localStorage) | Fonctionnel avec message de statut |
| Plan : ajout/édition/suppression de périodes, échéances, vie | Fonctionnel avec annulation |
| Préparation mentale (projet, mots-clés, routines) | Saisie et conservation |
| Événement : formulaire adapté au type (compétition, grade, autre) | Fonctionnel |
| Tableau de bord : profil, radar, alertes, évolution, objectifs | Fonctionnel (avec limitations, cf. P1) |
| Calculs SJFT et perte de rendement | Fonctionnels (incohérences connues, cf. AUD-10) |
| Import CSV et ICS vers échéances | Basique (limitations connues, cf. AUD-13) |
| Mobile-first : nav en bas, champs 16px, grandes zones tactiles | Vérifié Chromium 320–1440px |
| Pas de dépendance externe | Vérifié : aucune bibliothèque JS ni police distante |
| Référentiel scientifique documenté | 65 sections, sources PubMed et France Judo |
| Documentation complète | README, SPECIFICATIONS, ergonomie, sources |

### ⚠️ Problèmes critiques identifiés (P0)

| ID | Problème | Impact |
|----|----------|--------|
| AUD-01 | Corruption des nombres à l'import (`6 /10` → `610`, `22:00` → `2200`) | **Données faussées** |
| AUD-02 | Export partiel : 8 catégories sur 23 sérialisées | **Perte de données** |
| AUD-03 | Perte de compétitions au réimport (`Competition` ≠ `Compétition`) | **Perte de données** |
| AUD-05 | Un texte quelconque accepté comme profil valide | **Corruption silencieuse** |
| AUD-06 | Injection HTML possible via données importées/saisies (`innerHTML`) | **Sécurité** |
| AUD-07 | Faux états favorables sur données absentes (curseurs préremplis) | **Indicateurs trompeurs** |
| AUD-04 | Perte de saisie au changement d'onglet / rechargement | **Perte de données** |

### ⚠️ Problèmes élevés identifiés (P1)

| ID | Problème | Impact |
|----|----------|--------|
| AUD-08 | Import ne resynchronise pas les formulaires | Données orphelines |
| AUD-09 | Pas de validation effective des saisies | Données incohérentes |
| AUD-10 | Sens des couleurs inversé (confiance haute = défavorable) | Indicateurs trompeurs |
| AUD-11 | 5 axes du radar à 0.5 fictifs | Radar partiellement inventé |
| AUD-12 | RED-S toujours caché, SAS-2 non calculé | Fonctions annoncées mais inertes |
| AUD-13 | Import ICS/CSV fragile (pas d'échappement, pas de dédoublonnage) | Import peu fiable |
| AUD-14 | Dates : imposées UTC, janvier rattaché au mauvais trimestre | Historique faussé |
| AUD-15 | Pas d'édition/suppression d'évaluation ou événement | Parcours incomplet |
| AUD-16 | Labels non associés, radar sans alternative textuelle | Accessibilité |

### 📊 Statistiques du backlog

| Priorité | Total | Terminé | Restant |
|----------|-------|---------|---------|
| P0 — Fiabiliser le socle | 7 | 0 | **7** |
| P1 — Parcours et indicateurs | 19 | 0 | **19** |
| P2 — Extensions référentiel | 5 | 0 | **5** |
| V — Vérification | 3 | 0 | **3** |
| **Total tâches de développement** | **34** | **0** | **34** |
| Spécifications (doc) | 4 | 4 | 0 |
| Ergonomie (doc + code) | 6 | 6 | 0 |
| Audit (doc) | 6 | 6 | 0 |

### Prochaines étapes recommandées

1. **P0-01** → Contrat de données : c'est le prérequis de presque tout le reste.
2. **P0-05** → Sécurité : neutraliser les injections HTML (rapide, haut impact).
3. **P0-02 + P0-03** → Fiabiliser l'aller-retour Markdown (la perte de données est le défaut le plus visible).
4. **P0-04** → Validation des imports (empêcher la corruption).
5. **V-01** → Mettre en place des tests avant de corriger P1.
