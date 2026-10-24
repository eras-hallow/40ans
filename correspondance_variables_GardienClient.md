# Correspondance des variables : GardienClient

Script : `ReplicatedStorage["Système Raphy"].SystemeWhitelist.GardienClient`

« Supprimée » signifie que la variable n'existait que par un artefact du décompilateur et que sa valeur est écrite directement dans le code reconstitué.

## Niveau global (haut du script)

| Décompilé | Reconstitué |
|---|---|
| `Players`, `TeleportService`, `TweenService`, `UserInputService`, `Workspace` | inchangés (services Roblox) |
| `LocalPlayer` | `joueurLocal` |
| `Parent` | `dossierSysteme` |
| `Config` | `moduleConfig` |
| `Interface` | `moduleInterface` |
| `PanneauClient` | `modulePanneauClient` |
| `Remotes` | `dossierRemotes` |
| `Requete` | `remoteRequete` |
| `Evenement` | `remoteEvenement` |
| `THEME` | `theme` |
| `creer` | `creerInstance` |
| `px` | `pixels` |
| `v1` | `textesConfig` |
| `v2` | `officierConfig` |
| `PlayerGui` | `guiJoueur` |
| `v4` | `ecranPrincipal` |
| `v5` | `hauteurBarre` |
| `v6` | `fondPanneau` |
| `v7` | `conteneurPanneau` |
| `v8` | `conteneurNotifications` |
| `t2` | `notificationsActives` |
| `n1` | `compteurNotifications` |
| `v9` | `ecranTransition` |
| `t3` | `etatTeleportation` |
| `t3.base` | `etatTeleportation.messageBase` |
| `v10` | `redirectionEnCours` |
| `v36` | `ouvrirPanneau` |

## Fonctions

| Décompilé | Reconstitué |
|---|---|
| `texte` | `valeurOuDefaut` |
| `echelleVue` | `echelleVue` (inchangé) |
| `construireEcran` | `construireEcran` (inchangé) |
| `preparerTeleportation` | `preparerTeleportation` (inchangé) |
| `ecranChargementActif` | `ecranChargementActif` (inchangé) |
| `attendreChargement` | `attendreChargement` (inchangé) |
| `surRedirection` | `surRedirection` (inchangé) |
| `retirerNotification` | `retirerNotification` (inchangé) |
| `largeurNotification` | `largeurNotification` (inchangé) |
| `notifier` | `notifier` (inchangé) |
| `allerAccueil` | `allerAccueil` (inchangé) |
| `v36` | `ouvrirPanneau` |

## Dans `valeurOuDefaut` (ex-`texte`)

| Décompilé | Reconstitué |
|---|---|
| `p1` | `valeur` |
| `p2` | `valeurParDefaut` |
| `v3` | `valeurTexte` |

## Dans `echelleVue`

| Décompilé | Reconstitué |
|---|---|
| `CurrentCamera` | `cameraCourante` |
| `ViewportSize` | `tailleEcran` |

## Dans `construireEcran`

| Décompilé | Reconstitué |
|---|---|
| `p3` | `parentGui` |
| `p4` | `messageHaut` |
| `p5` | `messageBas` |
| `p6` | `echelleImposee` |
| `v14` | `fondTransition` |
| `v15` | `feuilleCentrale` |
| `v19` | `etiquetteMessageBas` |
| `etiquette`, `etiquette2`, `etiquette3` | supprimées (appel direct à `moduleInterface.etiquette`) |
| `t4`, `t5`, `t6` | supprimées (tables fusionnées dans l'appel) |
| `upper` | supprimée (`string.upper` direct) |
| `Service`, `v16` | supprimées (remplacées par `valeurOuDefaut(officierConfig.Service, …)`) |
| `Bureau`, `v17` | supprimées (idem avec `officierConfig.Bureau`) |
| `RedirectionHRP`, `v18` | supprimées (idem avec `textesConfig.RedirectionHRP`) |

## Dans `preparerTeleportation`

| Décompilé | Reconstitué |
|---|---|
| `p7` | `messageHaut` |
| `p8` | `messageBas` |
| `v20` | `guiTeleportation` |
| `CurrentCamera`, `ViewportSize` | supprimées (remplacées par `echelleVue()`) |
| `SetTeleportGui` | supprimée (`TeleportService.SetTeleportGui` direct) |
| `ok` | `succesPreparation` |
| `result` | `erreurPreparation` |

## Dans `ecranChargementActif`

| Décompilé | Reconstitué |
|---|---|
| `child` (boucle externe) | `guiEnfant` |
| `v24` | `nomMinuscule` |
| `child` (boucle interne) | `elementGui` |

## Dans `attendreChargement`

| Décompilé | Reconstitué |
|---|---|
| `elapsed` | `heureDebut` |

## Dans `surRedirection`

| Décompilé | Reconstitué |
|---|---|
| `p9` | `donneesRedirection` |
| `texte` (variable locale) | supprimée (devient `donneesRedirection.texte`) |
| `Redirection`, `v25` | `messageRedirectionDefaut` |
| `v26` | `messageRedirection` |
| `v27` | `fondTransition` |
| `v28` | `etiquetteMessageBas` |
| `v29` | `feuilleCentrale` |
| `v30` | `ecranTransitionCourant` |
| `n2` | `nbPoints` |
| `InvokeServer` | supprimée (`remoteRequete.InvokeServer` direct) |
| `PretRedirection` | `requeteReussie` |
| `PretRedirection2` | `reponseServeur` |
| `erreur`, `v31` | supprimées (remplacées par `valeurOuDefaut(reponseServeur.erreur, …)`) |

## Dans `retirerNotification`

| Décompilé | Reconstitué |
|---|---|
| `p10` | `notificationARetirer` |
| `v32` | `indexNotification` |
| `v33` | `notificationCourante` |

## Dans `largeurNotification`

| Décompilé | Reconstitué |
|---|---|
| `CurrentCamera` | `cameraCourante` |
| (aucune, l'expression était inlinée) | `largeurEcran` (ajoutée) |

## Dans `notifier`

| Décompilé | Reconstitué |
|---|---|
| `p11` | `donneesNotification` |
| `v37` | supprimée (fusionnée dans `donneesNotification`) |
| `v38` | `idDossier` |
| `t7` | supprimée (table fusionnée dans l'appel `creerInstance`) |
| `fromOffset` | supprimée (`UDim2.fromOffset` direct) |
| `CurrentCamera` | supprimée (remplacée par `largeurNotification()`) |
| `v41` | `notification` |
| `etiquette` (×2), `t8`, `t9` | supprimées (appels et tables fusionnés) |
| `titre`, `v42` | supprimées (remplacées par `valeurOuDefaut(donneesNotification.titre, "Whitelist")`) |
| `texte`, `v43` | `texteCorps` |
| `texte` (2e), `v44` | supprimées (doublon de `texteCorps`) |

## Dans `allerAccueil`

| Décompilé | Reconstitué |
|---|---|
| `InvokeServer` | supprimée (`remoteRequete.InvokeServer` direct) |
| `AllerAccueil` | `requeteReussie` |
| `AllerAccueil2` | `reponseServeur` |
| `t10` | supprimée (table fusionnée dans l'appel `notifier`) |
| `v45`, `v46`, `v47`, `erreur` | `messageErreur` |

## Dans `ouvrirPanneau` (ex-`v36`)

| Décompilé | Reconstitué |
|---|---|
| `p12` | `idDossier` |

## Dans les connexions d'événements

| Décompilé | Reconstitué |
|---|---|
| `p13` (`TeleportInitFailed`) | `joueurEnEchec` |
| `p14` (`InputBegan`) | `entree` |
| `p15` (`OnClientEvent`) | `typeEvenement` |
| `p16` (`OnClientEvent`) | `donneesEvenement` |
| `v50` (boucle `task.spawn`) | `panneauOuvert` |

## Remarques

- Les variables `_` des boucles `for _, … in ipairs` sont restées telles quelles (convention Lua pour une valeur ignorée).
- Les clés de tables échangées avec le serveur ou les autres modules (`texte`, `titre`, `ok`, `erreur`, `id`, `plein`, `requete`, `boutons`, `fonction`…) n'ont pas été renommées, pour ne pas casser la communication.
- Les noms des fonctions et champs des modules externes (`Interface.papier`, `Interface.tampon`, `PanneauClient.ouvrir`, etc.) sont conservés tels quels.
