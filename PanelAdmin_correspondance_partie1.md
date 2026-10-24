# PanelAdmin : correspondance des variables (partie 1)

Script : `game.StarterPlayer.StarterPlayerScripts.PanelAdmin`  
Fichier renommé : `PanelAdmin_renomme_partie1.lua`

## État d'avancement

- Variables renommées et documentées dans ce tableau : **197**.
- Identifiants décompilés (`v…`, `t…`, `p…`, `n…`) qui restent à traiter dans le fichier : **953**.
- Périmètre traité : helpers d'interface, pile de toasts, messages plein écran, bandeau d'aide, décompte, fenêtre déplaçable, et les variables d'état partagées dont le rôle est certain.
- Non traité pour l'instant : `showAlerte` et tout ce qui suit (suivi de joueurs, freecam, vol, barre de commande, pages, logs, animations, `applyInfo`...).

## Nettoyage mécanique appliqué avant le renommage

Le décompilateur avait recopié partout le contenu de la fonction `make` (création d'une Instance, boucle sur les propriétés, `Parent` appliqué en dernier). Ces blocs sont remplacés ainsi :

- 88 blocs `Instance.new` + boucle deviennent `make("Classe", proprietes)`.
- 33 blocs de la même forme (instance déjà créée) deviennent `appliquerProprietes(instance, proprietes)`, une fonction ajoutée en haut du script.
- Les variables temporaires de ces boucles sont supprimées quand plus rien ne les utilise.
- Résultat : 11 560 lignes, ramenées à environ 9 350.

## Légende

- `?` ou « ou nil » : la valeur peut être `nil`.
- « déduit » : le type ou le rôle vient de l'usage dans le code, sans déclaration explicite.
- Les noms de paramètres identiques dans des fonctions différentes (`options`, `enfants`, `titreAffiche`...) sont volontaires : chaque portée est indépendante.

## Haut du script (services, remotes, palette, détection d'appareil)

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `LocalPlayer` | `joueurLocal` | Player | Players.LocalPlayer |
| `PlayerGui` | `guiJoueur` | PlayerGui | Parent de l'écran StaffAdminUI |
| `v1` | `remoteInfoClient` | Instance (RemoteEvent, déduit) | StaffAdmin.ClientInfo : FireServer(typeAppareil) |
| `v2` | `typeAppareil` | string | "Console", "VR", "PC", "Tablette" ou "Mobile" |
| `t1` | `couleurs` | table de Color3 | Palette : bg, bg2, panel, panel2, input, stroke, text, muted, accent, accent2, green, red, orange, gold, purple |
| `t2` | `couleursRang` | table indexée 0 à 5 | Couleur d'affichage selon le rang staff |
| `t3` | `polices` | table d'Enum.Font | bold, medium, regular, mono |

## État partagé (variables déclarées à l'avance)

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `v11` | `etatStaff` | table | État staff du joueur : rank, rankName, prefix, commands |
| `t29` | `etatStaffInitial` | table | Valeur de départ de etatStaff (rank 0, "Joueur", prefixe ":") |
| `v12` | `vitesseVol` | number | Vitesse de vol en studs/s (60 au départ) |
| `v13` | `vitesseTriple` | boolean | Active le multiplicateur x3 de la vitesse de vol |
| `v14` | `volActif` | boolean (déduit) | Vol activé ou non, testé par basculerVol |
| `v15` | `etiquetteVitesseVol` | TextLabel | Affiche « Vol : N studs/s » |
| `v16` | `barreCommande` | Frame | Conteneur de la barre de commande |
| `v17` | `etiquettePrefixe` | TextLabel | Affiche etatStaff.prefix |
| `t8` | `suggestionsCourantes` | table (déduit) | Vidée à chaque updateSuggestions |
| `t9` | `historiqueCommandes` | table | Commandes saisies, la plus récente en premier |
| `v18` | `indexHistorique` | number (déduit) | Position de navigation dans l'historique (0 = aucune) |
| `v19` | `modeAnimation` | boolean (déduit) | Panneau Animation (true) ou Staff (false) |
| `v20` | `ongletActif` | string | Id de l'onglet affiché, "players" au départ |
| `t10` | `pagesParOnglet` | table | Frame de page indexée par id d'onglet (newPage) |
| `t11` | `boutonsOnglets` | table (déduit) | Boutons d'onglets, parcourus par ajusterMenu |
| `v21` | `echelleMenu` | number | Échelle UIScale du menu, bornée 0.45 à 1 |
| `v22` | `afficherOnglet` | function | afficherOnglet(id) : change d'onglet |
| `v25` | `titrePanneau` | TextLabel | "PANEL STAFF" ou "PANEL ANIMATION" |
| `v26` | `sousTitrePanneau` | TextLabel | Nombre de joueurs en ligne et pseudo local |
| `v27` | `pastilleRang` | TextLabel (pill) | Nom du rang (etatStaff.rankName) |
| `t12` | `rangMinParOutil` | table | Rang minimum requis, indexé par id d'outil |
| `v37` | `valeurEnLigneDepuis` | TextLabel | Durée d'activité du serveur (fmtDuration) |
| `v38` | `valeurVersionJeu` | TextLabel | "v" .. PlaceVersion |

## make

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p1` | `classe` | string | Nom de classe Instance.new |
| `p2` | `proprietes` | table | Propriétés à appliquer ; la clé Parent est appliquée en dernier |
| `p3` | `enfants` | table ou nil | Instances à parenter à l'objet créé |
| `v3` | `instance` | Instance | Objet créé |
| `v4` | `parentFinal` | Instance ou nil | Parent retenu |
| `v5` | `parentTemp` | Instance ou nil | Valeur temporaire du Parent dans la boucle |
| `v6` | `clePropriete` | string | Nom de la propriété |
| `v7` | `valeur` | any | Valeur de la propriété |
| `v8` | `enfant` | Instance | Enfant en cours |

## merge

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p4` | `cible` | table | Table modifiée par merge |
| `p5` | `source` | table ou nil | Table copiée dans cible |
| `v9` | `clePropriete` | any | Clé |
| `v10` | `valeur` | any | Valeur |

## corner

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p6` | `rayon` | number ou nil | Rayon du UICorner en pixels (8 par défaut) |
| `t14` | `proprietesCoin` | table | Propriétés passées à make("UICorner") |

## round

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `t15` | `proprietesRond` | table | Propriétés passées à make("UICorner") (rayon 1 = rond) |

## stroke

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p7` | `couleur` | Color3 ou nil | Couleur du contour |
| `p8` | `epaisseur` | number ou nil | Épaisseur du contour (1 par défaut) |
| `p9` | `transparence` | number ou nil | Transparence du contour |
| `t16` | `proprietesContour` | table | Propriétés passées à make("UIStroke") |

## pad

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p10` | `haut` | number | Marge haute en pixels (sert de défaut aux autres) |
| `p11` | `droite` | number ou nil | Marge droite |
| `p12` | `bas` | number ou nil | Marge basse |
| `p13` | `gauche` | number ou nil | Marge gauche |
| `t17` | `proprietesMarges` | table | Propriétés passées à make("UIPadding") |

## vlist

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p14` | `espacement` | number ou nil | Espacement vertical (6 par défaut) |
| `t18` | `proprietesListeVerticale` | table | Propriétés passées à make("UIListLayout") |

## hlist

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p15` | `espacement` | number ou nil | Espacement horizontal (6 par défaut) |
| `p16` | `alignementHorizontal` | Enum.HorizontalAlignment ou nil | Alignement horizontal |
| `t19` | `proprietesListeHorizontale` | table | Propriétés passées à make("UIListLayout") |

## label

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p17` | `options` | table ou nil | Propriétés qui remplacent celles par défaut de l'étiquette |
| `p18` | `enfants` | table ou nil | Enfants de l'étiquette |
| `t20` | `proprietesEtiquette` | table | Propriétés passées à make("TextLabel") (défauts du helper label) |
| `v72` | `clePropriete` | string | Propriété à écraser |
| `v73` | `valeur` | any | Valeur |

## button

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p19` | `options` | table ou nil | Propriétés qui remplacent celles par défaut du bouton |
| `p20` | `enfants` | table ou nil | Enfants du bouton |
| `t21` | `proprietesBouton` | table | Propriétés passées à make("TextButton") (défauts du helper button) |
| `v74` | `clePropriete` | string | Propriété à écraser |
| `v75` | `valeur` | any | Valeur |
| `v76` | `boutonCree` | TextButton | Bouton créé |
| `t22` | `proprietesCoinBouton` | table | Propriétés passées à make("UICorner") |

## textbox

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p21` | `options` | table ou nil | Propriétés qui remplacent celles par défaut du champ |
| `t23` | `proprietesChampTexte` | table | Propriétés passées à make("TextBox") (défauts du helper textbox) |
| `v81` | `clePropriete` | string | Propriété à écraser |
| `v82` | `valeur` | any | Valeur |
| `t24` | `proprietesCoinChamp` | table | Propriétés passées à make("UICorner") |
| `t25` | `proprietesContourChamp` | table | Propriétés passées à make("UIStroke") |

## scroller

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p22` | `options` | table ou nil | Propriétés qui remplacent celles par défaut du ScrollingFrame |
| `t26` | `proprietesDefilement` | table | Propriétés passées à make("ScrollingFrame") (défauts du helper scroller) |
| `v95` | `clePropriete` | string | Propriété à écraser |
| `v96` | `valeur` | any | Valeur |

## pill

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p23` | `texteAffiche` | string | Texte de la pastille |
| `p24` | `couleur` | Color3 | Couleur du fond et du texte |
| `p25` | `options` | table ou nil | Propriétés supplémentaires |
| `t27` | `proprietesPastille` | table | Propriétés passées à make("TextLabel") (défauts du helper pill) |
| `v101` | `clePropriete` | string | Propriété à écraser |
| `v102` | `valeur` | any | Valeur |
| `v103` | `pastille` | TextLabel | Pastille créée |
| `t28` | `proprietesCoinPastille` | table | Propriétés passées à make("UICorner") |

## clearChildren

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p26` | `conteneur` | Instance | Détruit tous ses enfants GuiObject |

## tween

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p27` | `objet` | Instance | Objet animé |
| `p28` | `duree` | number | Durée du tween en secondes |
| `p29` | `proprietesCibles` | table | Propriétés d'arrivée du tween |
| `p30` | `styleAnimation` | Enum.EasingStyle ou nil | Style (Quint par défaut) |

## thumb

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p31` | `idUtilisateur` | number | UserId du joueur |
| `p32` | `taille` | number | Largeur et hauteur de la vignette |

## fmtTime

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p33` | `horodatage` | number | Timestamp Unix (formaterHeure) |
| `v108` | `formaterHeure` | function | Formate l'heure en fr-fr (appelée via pcall) |

## fmtDate

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p34` | `horodatage` | number | Timestamp Unix (formaterDate) |
| `v109` | `formaterDate` | function | Formate la date en fr-fr (appelée via pcall) |

## fmtDuration

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p35` | `secondes` | number | Durée en secondes |
| `v111` | `secondesEntieres` | number | Durée arrondie à l'entier, minimum 0 |

## sanitize

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p36` | `valeur` | any | Valeur à nettoyer des caractères \| \r \n |

## sendCommand

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p37` | `commandeEnvoyee` | string | Texte envoyé à remoteCommande |

## fetch

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p38` | `typeRequete` | string | Premier argument de GetData:InvokeServer |
| `p39` | `argumentRequete` | any | Second argument de InvokeServer |
| `v120` | `appelerServeur` | function | Appelle GetData:InvokeServer sous pcall |

## Pile des toasts (création de l'écran StaffAdminUI)

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `t30` | `proprietesEcranStaff` | table | Propriétés passées à make("ScreenGui (StaffAdminUI)") |
| `t31` | `proprietesPileToasts` | table | Propriétés passées à make("Frame") (colonne des toasts) |
| `t33` | `proprietesListePileToasts` | table | Propriétés passées à make("UIListLayout") |
| `v129` | `pileToasts` | Frame | Colonne des toasts, en bas à gauche |
| `n1` | `compteurToasts` | number | Incrémenté à chaque toast (LayoutOrder) |

## toast

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p40` | `titreAffiche` | string | Titre du toast |
| `p41` | `message` | string | Corps du toast |
| `p42` | `typeToast` | string | "err", "ok", "help" ou autre (accent) |
| `p43` | `optionsToast` | table ou nil | buttons (liste de {text, color, onClick}) et duration |
| `v130` | `couleurToast` | Color3 | Couleur de la barre et du titre |
| `v131` | `couleurTrouvee` | boolean | Artefact : true quand le type err/ok a donné une couleur |
| `v132` | `estAide` | boolean | typeToast == "help" |
| `t34` | `toastsExistants` | table | CanvasGroup déjà affichés, triés par LayoutOrder |
| `p44` | `toastA` | CanvasGroup | Premier élément du tri |
| `p45` | `toastB` | CanvasGroup | Second élément du tri |
| `t35` | `proprietesToast` | table | Propriétés passées à make("CanvasGroup") |
| `t37` | `proprietesCoinToast` | table | Propriétés passées à make("UICorner") |
| `v137` | `carteToast` | CanvasGroup | Carte du toast |
| `t38` | `proprietesBarreCouleur` | table | Propriétés passées à make("Frame") (barre colorée de 4 px) |
| `t39` | `proprietesContenuToast` | table | Propriétés passées à make("Frame") |
| `v142` | `margesContenuToast` | UIPadding | Marges du contenu |
| `t41` | `proprietesListeContenuToast` | table | Propriétés passées à make("UIListLayout") |
| `v147` | `contenuToast` | Frame | Contient titre, message et boutons |
| `v148` | `rangeeBoutonsToast` | Frame | Rangée des boutons optionnels |
| `v149` | `indexBouton` | number | Index du bouton |
| `v150` | `definitionBouton` | table | {text, color, onClick} |
| `t42` | `proprietesBoutonToast` | table | Propriétés passées à make("TextButton") |
| `v151` | `boutonToast` | TextButton | Bouton d'action du toast |
| `t43` | `proprietesApparition` | table | Tween : GroupTransparency vers 0 |
| `t44` | `proprietesDisparition` | table | Tween : GroupTransparency vers 1 |

## showMessage

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p46` | `titreAffiche` | string | Titre du message plein écran |
| `p47` | `message` | any | Corps du message (converti en string) |
| `v152` | `texteMessage` | string | tostring(message) ; sa longueur règle la durée d'affichage |
| `t45` | `proprietesFondMessage` | table | Propriétés passées à make("TextButton") (fond cliquable) |
| `t46` | `proprietesCarteMessage` | table | Propriétés passées à make("Frame") |
| `t48` | `proprietesContrainteMessage` | table | Propriétés passées à make("UISizeConstraint") (largeur max 620) |
| `t49` | `proprietesCoinMessage` | table | Propriétés passées à make("UICorner") |
| `t50` | `proprietesContourMessage` | table | Propriétés passées à make("UIStroke") |
| `t51` | `proprietesDegradeMessage` | table | Propriétés passées à make("UIGradient") |
| `v173` | `margesMessage` | UIPadding | Marges de la carte |
| `t52` | `proprietesListeMessage` | table | Propriétés passées à make("UIListLayout") |
| `v178` | `carteMessage` | Frame | Carte centrale |
| `t53` | `proprietesFondApparition` | table | Tween : BackgroundTransparency vers 0.4 |

## showHint

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p48` | `nomCommande` | any | Nom affiché avant « : » |
| `p49` | `description` | any | Texte affiché après « : » |
| `t54` | `proprietesBandeauAide` | table | Propriétés passées à make("Frame") |
| `t55` | `proprietesLigneBandeau` | table | Propriétés passées à make("Frame") (ligne d'accent de 2 px) |
| `n2` | `compteurDecomptes` | number | Identifie le décompte le plus récent |
| `v188` | `carteDecompteActif` | Frame ou nil | Décompte actuellement affiché |

## showCountdown

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p50` | `dureeSecondes` | number | Durée du décompte |
| `p51` | `titreEvenement` | string ou nil | Titre (« Événement » par défaut) |
| `p52` | `auteur` | any | Auteur, affiché après « par » |
| `v189` | `secondesRestantes` | number | Copie de dureeSecondes |
| `v190` | `idDecompte` | number | Valeur de compteurDecomptes à la création |
| `v191` | `dureeNombre` | number | tonumber(dureeSecondes) ou 0 |
| `t56` | `proprietesCarteDecompte` | table | Propriétés passées à make("Frame") |
| `t58` | `proprietesCoinDecompte` | table | Propriétés passées à make("UICorner") |
| `t59` | `proprietesContourDecompte` | table | Propriétés passées à make("UIStroke") |
| `v200` | `margesDecompte` | UIPadding | Marges de la carte |
| `t60` | `proprietesListeDecompte` | table | Propriétés passées à make("UIListLayout") |
| `v205` | `carteDecompte` | Frame | Carte du décompte |
| `t61` | `proprietesTitreDecompte` | table | Propriétés passées à make("TextLabel") |
| `v206` | `titreDecompte` | string | Titre retenu |
| `v207` | `titreValide` | boolean | Artefact : titre non vide |
| `v208` | `titreBrut` | string | tostring(titreEvenement) |
| `v209` | `etiquetteChiffres` | TextLabel | Chiffres du décompte puis « GO ! » |
| `t62` | `proprietesEchelleChiffres` | table | Propriétés passées à make("UIScale") |
| `t63` | `proprietesRetourEchelle` | table | Tween : Scale vers 1 |

## makeDraggable

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p53` | `fenetre` | GuiObject | Objet déplacé |
| `p54` | `poignee` | GuiObject | Zone qui lance le glissement |
| `v216` | `glissementEnCours` | boolean | true pendant un glissement |
| `p55` | `entreeDebut` | InputObject | Clic ou toucher initial |
| `p56` | `entreeDeplacement` | InputObject | Mouvement de souris ou de doigt |
| `v217` | `deplacement` | Vector3 | Position actuelle moins position de départ |

## openWindow

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p57` | `titreAffiche` | any | Titre de la fenêtre |
| `p58` | `lignes` | table ou nil | Lignes de texte ; « (vide) » si absent |
| `t64` | `proprietesFenetre` | table | Propriétés passées à make("Frame") |
| `t66` | `proprietesContrainteFenetre` | table | Propriétés passées à make("UISizeConstraint") (480 x 380 max) |
| `t67` | `proprietesCoinFenetre` | table | Propriétés passées à make("UICorner") |
| `t68` | `proprietesContourFenetre` | table | Propriétés passées à make("UIStroke") |
| `t69` | `proprietesDegradeFenetre` | table | Propriétés passées à make("UIGradient") |
| `v234` | `fenetre` | Frame | Fenêtre |
| `v235` | `etiquetteTitreFenetre` | TextLabel | Barre de titre (poignée de déplacement) |
| `t70` | `proprietesSeparateurFenetre` | table | Propriétés passées à make("Frame") |
| `v240` | `defilementFenetre` | ScrollingFrame | Zone des lignes |
| `t71` | `proprietesListeLignes` | table | Propriétés passées à make("UIListLayout") |
| `v245` | `lignesAffichees` | table | lignes, ou { "(vide)" } si vide |
| `v246` | `indexLigne` | number | Index |
| `v247` | `ligneTexte` | any | Ligne de texte |

## Remarques

- Les noms d'origine conservés par le décompilateur (`toast`, `showMessage`, `openWindow`, `make`, `label`, `button`, `scroller`...) n'ont pas été touchés.
- Les variables nommées mais réutilisées pour plusieurs rôles (`Frame`, `Frame2`, `TextButton`, `UIScale`, `UICorner`...) sont laissées telles quelles : le décompilateur réutilise ces noms pour des objets différents d'une fonction à l'autre.
- Les clés de tables et noms de champs (`buttons`, `duration`, `text`, `color`, `onClick`, `rank`, `rankName`, `prefix`, `commands`, `minRank`...) ne sont pas renommés, car elles circulent entre le client et le serveur.
- Le script n'a pas été exécuté (pas de Roblox Studio ici) : teste-le avant de t'en servir.