# Correspondance détaillée des variables : GardienClient

Script : `ReplicatedStorage["Système Raphy"].SystemeWhitelist.GardienClient`

## Légende

- **Supprimée** : la variable n'existait que par un artefact du décompilateur. Sa valeur est écrite directement dans le code reconstitué.
- **`?`** après un type : la valeur peut être `nil`.
- **(déduit)** : le type vient de l'usage dans le script, pas d'une déclaration explicite (cas des modules externes `Interface`, `PanneauClient`, `Config`, dont on n'a pas le code).
- **Portée** : indiquée dans le titre de chaque section (globale au script ou locale à une fonction).

---

## 1. Variables globales du script

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `Players` | `Players` | Instance (service) | `game:GetService("Players")`. Sert à obtenir `LocalPlayer` |
| `TeleportService` | `TeleportService` | Instance (service) | `SetTeleportGui(gui)` et événement `TeleportInitFailed` |
| `TweenService` | `TweenService` | Instance (service) | `TweenService:Create(objet, TweenInfo, propriétés):Play()` |
| `UserInputService` | `UserInputService` | Instance (service) | Événement `InputBegan` (touche Échap) |
| `Workspace` | `Workspace` | Instance (service) | Sert à lire `Workspace.CurrentCamera` |
| `LocalPlayer` | `joueurLocal` | Player | `Players.LocalPlayer`. Comparé au joueur reçu dans `TeleportInitFailed` |
| `Parent` | `dossierSysteme` | Instance | `script.Parent`. Contient `Config`, `Interface`, `PanneauClient`, `Remotes` |
| `Config` | `moduleConfig` | table (retour de `require`) | Champs lus : `.Textes`, `.Officier` |
| `Interface` | `moduleInterface` | table de fonctions et constantes (déduit) | Fournit `THEME`, `creer`, `px`, `hauteurBarre`, `liste`, `papier`, `echelle`, `etiquette`, `tampon`, `trait`, `marges`, `bouton` |
| `PanneauClient` | `modulePanneauClient` | table de fonctions (déduit) | `fermer()`, `ouvrir(options)`, `estOuvert()`, `rafraichir()` |
| `Remotes` | `dossierRemotes` | Instance (Folder) | `dossierSysteme:WaitForChild("Remotes")`. Contient les deux remotes |
| `Requete` | `remoteRequete` | RemoteFunction | `InvokeServer("PretRedirection")` et `InvokeServer("AllerAccueil")`. Aussi passé à `ouvrir` via `requete = ...` |
| `Evenement` | `remoteEvenement` | RemoteEvent | `OnClientEvent(typeEvenement, donnees)` |
| `THEME` | `theme` | table de Color3 (déduit) | Champs : `Contenu`, `Fond`, `Or`, `Encre`, `EncreSombre`, `GrisSombre`, `Tampon` |
| `creer` | `creerInstance` | function (déduit) | `creerInstance(classe: string, propriétés: table, enfants: table?) -> Instance` |
| `px` | `pixels` | function (déduit) | `pixels(x, y) -> UDim2`, utilisé pour `Size` et `Position` |
| `v1` | `textesConfig` | table | `Config.Textes or {}`. Champs lus : `Redirection`, `RedirectionHRP` (strings) |
| `v2` | `officierConfig` | table | `Config.Officier or {}`. Champs lus : `Service`, `Bureau` (strings) |
| `PlayerGui` | `guiJoueur` | PlayerGui | `joueurLocal:WaitForChild("PlayerGui")`. Parent de tous les écrans |
| `v4` | `ecranPrincipal` | ScreenGui | Écran `WhitelistRaphy`, DisplayOrder 60. Parent du fond, du panneau et des notifications |
| `v5` | `hauteurBarre` | number | Retour de `Interface.hauteurBarre()`, en pixels. Sert de décalage vertical du panneau |
| `v6` | `fondPanneau` | Frame | Fond opaque derrière le panneau. `Visible` suit `estOuvert()` |
| `v7` | `conteneurPanneau` | Frame | Zone où `PanneauClient.ouvrir` dessine le panneau |
| `v8` | `conteneurNotifications` | Frame | Colonne de notifications alignée en haut à droite |
| `t2` | `notificationsActives` | table (tableau de TextButton) | File des notifications visibles, 4 maximum |
| `n1` | `compteurNotifications` | number (entier) | Incrémenté à chaque notification. Son opposé sert de `LayoutOrder` |
| `v9` | `ecranTransition` | ScreenGui ou nil | `nil` jusqu'à la première redirection. Exclu de la détection d'écran de chargement |
| `t3` | `etatTeleportation` | table | Une seule clé : `messageBase` (string). Lue par l'animation des points |
| `t3.base` | `etatTeleportation.messageBase` | string | Texte affiché en bas de l'écran. Modifié en cas d'erreur, d'échec ou de lenteur |
| `v10` | `redirectionEnCours` | boolean | Verrou : empêche de lancer deux redirections |
| `v36` | `ouvrirPanneau` | function | `ouvrirPanneau(idDossier: number?)`. Déclarée en avance, définie après `allerAccueil` |

---

## 2. Fonctions

| Décompilé | Reconstitué | Signature | Format d'usage / rôle |
|---|---|---|---|
| `texte` | `valeurOuDefaut` | `(valeur: any, valeurParDefaut: string?) -> string` | Convertit en string. Renvoie le défaut si `nil` ou vide |
| `echelleVue` | `echelleVue` | `() -> number` | Facteur d'échelle de l'écran, borné entre 0.45 et 1.6 |
| `construireEcran` | `construireEcran` | `(parentGui: Instance, messageHaut: string, messageBas: string, echelleImposee: number?) -> Frame, TextLabel, Frame` | Construit l'écran « KONTROLLE » (retours : fond, étiquette du bas, feuille) |
| `preparerTeleportation` | `preparerTeleportation` | `(messageHaut: string, messageBas: string) -> ()` | Prépare l'écran affiché pendant le changement de place via `SetTeleportGui` |
| `ecranChargementActif` | `ecranChargementActif` | `() -> boolean` | `true` si un ScreenGui tiers dont le nom contient `load` ou `charg` est visible |
| `attendreChargement` | `attendreChargement` | `() -> ()` (yield) | Attend `game:IsLoaded()` puis la fin des écrans de chargement, 20 s maximum |
| `surRedirection` | `surRedirection` | `(donneesRedirection: table?) -> ()` (yield) | Lancée par `task.spawn` sur l'événement `"Redirection"` |
| `retirerNotification` | `retirerNotification` | `(notificationARetirer: TextButton) -> ()` | Retire de la liste et détruit l'instance |
| `largeurNotification` | `largeurNotification` | `() -> number` | 28 % de la largeur d'écran, bornée entre 260 et 380 |
| `notifier` | `notifier` | `(donneesNotification: table?) -> ()` | Crée une notification. Champs lus : `titre`, `texte`, `id` |
| `allerAccueil` | `allerAccueil` | `() -> ()` | Bouton du panneau : demande `"AllerAccueil"` au serveur et notifie le résultat |
| `v36` | `ouvrirPanneau` | `(idDossier: number?) -> ()` | Ferme puis rouvre le panneau avec le bouton « Aller au bureau d'accueil » |

---

## 3. Dans `valeurOuDefaut` (ex-`texte`)

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p1` | `valeur` | any | Valeur à convertir |
| `p2` | `valeurParDefaut` | string ou nil | Texte de repli. `""` si absent |
| `v3` | `valeurTexte` | string | `tostring(valeur)`, testée contre `""` |

## 4. Dans `echelleVue`

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `CurrentCamera` | `cameraCourante` | Camera ou nil | `Workspace.CurrentCamera` |
| `ViewportSize` | `tailleEcran` | Vector2 | Taille d'écran, `(1280, 720)` si pas de caméra |

## 5. Dans `construireEcran`

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p3` | `parentGui` | Instance | Parent du fond (ScreenGui) |
| `p4` | `messageHaut` | string | Message principal (taille 27, centré) |
| `p5` | `messageBas` | string | Message du bas (taille 22), mis à jour par l'animation |
| `p6` | `echelleImposee` | number ou nil | Si présent, `UIScale` fixe. Sinon `Interface.echelle(feuille, 1100, 620)` |
| `v14` | `fondTransition` | Frame | Fond plein écran de couleur `theme.Fond` |
| `v15` | `feuilleCentrale` | Frame (retour de `papier`, déduit) | Feuille 840x440 avec bordure dorée |
| `v19` | `etiquetteMessageBas` | TextLabel (retour de `etiquette`, déduit) | Étiquette dont `.Text` est animé |
| `etiquette`, `etiquette2`, `etiquette3` | Supprimées | function | Alias de `Interface.etiquette` |
| `t4`, `t5`, `t6` | Supprimées | table | Propriétés d'étiquettes, fusionnées dans l'appel |
| `upper` | Supprimée | function | Alias de `string.upper` |
| `Service`, `v16` | Supprimées | string ou nil, puis string | Remplacées par `valeurOuDefaut(officierConfig.Service, "Ortskommandantur de Carentan")` |
| `Bureau`, `v17` | Supprimées | string ou nil, puis string | Remplacées par `valeurOuDefaut(officierConfig.Bureau, "Passierscheinstelle, …")` |
| `RedirectionHRP`, `v18` | Supprimées | string ou nil, puis string | Remplacées par `valeurOuDefaut(textesConfig.RedirectionHRP, "")` |

## 6. Dans `preparerTeleportation`

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p7` | `messageHaut` | string | Transmis à `construireEcran` |
| `p8` | `messageBas` | string | Transmis à `construireEcran` |
| `v20` | `guiTeleportation` | ScreenGui | Créé sans parent, remis à `SetTeleportGui` |
| `CurrentCamera`, `ViewportSize` | Supprimées | Camera ou nil, Vector2 | Remplacées par l'appel à `echelleVue()` |
| `SetTeleportGui` | Supprimée | function | Remplacée par `TeleportService.SetTeleportGui` |
| `ok` | `succesPreparation` | boolean | Résultat du `pcall` |
| `result` | `erreurPreparation` | any (string si échec) | Message d'erreur, passé à `warn` |

## 7. Dans `ecranChargementActif`

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `child` (boucle externe) | `guiEnfant` | Instance | Candidat parcouru dans `PlayerGui` |
| `v24` | `nomMinuscule` | string | `string.lower(guiEnfant.Name)`, testé avec `string.find(..., 1, true)` |
| `child` (boucle interne) | `elementGui` | Instance | Enfant du candidat, testé avec `IsA("GuiObject")` et `.Visible` |

## 8. Dans `attendreChargement`

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `elapsed` | `heureDebut` | number | `os.clock()` au départ. Sert à mesurer le délai de 20 s |

## 9. Dans `surRedirection`

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p9` | `donneesRedirection` | table (ou n'importe quoi, normalisé en table) | Charge utile de l'événement. Champ lu : `texte` |
| `texte` (locale) | Supprimée | string ou nil | Remplacée par `donneesRedirection.texte` |
| `Redirection`, `v25` | `messageRedirectionDefaut` | string | `textesConfig.Redirection` ou `"Direction le bureau d'accueil."` |
| `v26` | `messageRedirection` | string | Message final affiché sur l'écran |
| `v27` | `fondTransition` | Frame | Fond de l'écran, animé par un Tween de transparence |
| `v28` | `etiquetteMessageBas` | TextLabel (déduit) | Texte animé « … » |
| `v29` | `feuilleCentrale` | Frame | Masquée au départ, affichée après 0,35 s |
| `v30` | `ecranTransitionCourant` | ScreenGui | Copie locale de `ecranTransition`, capturée par les closures |
| `n2` | `nbPoints` | number (1 à 3) | Nombre de points de l'animation |
| `InvokeServer` | Supprimée | function | Remplacée par `remoteRequete.InvokeServer` |
| `PretRedirection` | `requeteReussie` | boolean | Résultat du `pcall` |
| `PretRedirection2` | `reponseServeur` | any (table `{ ok: boolean, erreur: string? }` attendue) | Réponse du serveur |
| `erreur`, `v31` | Supprimées | string ou nil, puis string | Remplacées par `valeurOuDefaut(reponseServeur.erreur, "Téléportation refusée")` |

## 10. Dans `retirerNotification`

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p10` | `notificationARetirer` | TextButton | Notification à retirer |
| `v32` | `indexNotification` | number | Index dans `notificationsActives` |
| `v33` | `notificationCourante` | TextButton | Élément courant de la boucle |

## 11. Dans `largeurNotification`

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `CurrentCamera` | `cameraCourante` | Camera ou nil | Caméra du Workspace |
| (inlinée) | `largeurEcran` | number | `ViewportSize.X`, ou 1280 par défaut (variable ajoutée) |

## 12. Dans `notifier`

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p11` | `donneesNotification` | table | Champs lus : `titre`, `texte`, `id` |
| `v37` | Supprimée | table | Version normalisée de `p11`, fusionnée dans `donneesNotification` |
| `v38` | `idDossier` | number ou nil | `tonumber(donneesNotification.id)`. Si présent : bouton « Voir » et durée de 15 s |
| `t7` | Supprimée | table | Propriétés du TextButton, fusionnées dans l'appel |
| `fromOffset` | Supprimée | function | Remplacée par `UDim2.fromOffset` |
| `CurrentCamera` | Supprimée | Camera ou nil | Remplacée par `largeurNotification()` |
| `v41` | `notification` | TextButton | Notification (cliquable pour la fermer) |
| `etiquette` (×2), `t8`, `t9` | Supprimées | function, table | Alias et tables fusionnés dans les appels |
| `titre`, `v42` | Supprimées | any, puis string | Remplacées par `valeurOuDefaut(donneesNotification.titre, "Whitelist")` |
| `texte`, `v43` | `texteCorps` | string | Corps du message. L'étiquette n'est créée que s'il est non vide |
| `texte` (2e), `v44` | Supprimées | any, puis string | Doublon de `texteCorps` |

## 13. Dans `allerAccueil`

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `InvokeServer` | Supprimée | function | Remplacée par `remoteRequete.InvokeServer` |
| `AllerAccueil` | `requeteReussie` | boolean | Résultat du `pcall` |
| `AllerAccueil2` | `reponseServeur` | any (table `{ ok: boolean, erreur: string? }` attendue) | Réponse du serveur |
| `t10` | Supprimée | table | Table de notification, fusionnée dans l'appel |
| `v45`, `v46`, `v47`, `erreur` | `messageErreur` | string | Message d'erreur. Trois cas : `erreur` du serveur, `"Action refusée."`, `"Réponse invalide du serveur."` |

## 14. Dans `ouvrirPanneau` (ex-`v36`)

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p12` | `idDossier` | number ou nil | Id transmis à `PanneauClient.ouvrir({ id = ... })` |

## 15. Dans les connexions d'événements

| Décompilé | Reconstitué | Type | Format d'usage / rôle |
|---|---|---|---|
| `p13` (`TeleportInitFailed`) | `joueurEnEchec` | Player | Comparé à `joueurLocal` |
| `p14` (`InputBegan`) | `entree` | InputObject | `entree.KeyCode == Enum.KeyCode.Escape` |
| `p15` (`OnClientEvent`) | `typeEvenement` | string | `"Redirection"`, `"Notification"`, `"OuvrirPanneau"` ou `"Rafraichir"` |
| `p16` (`OnClientEvent`) | `donneesEvenement` | any (table attendue) | Charge utile transmise à `surRedirection`, `notifier` ou `ouvrirPanneau` |
| `v50` (boucle `task.spawn`) | `panneauOuvert` | boolean | `PanneauClient.estOuvert()`, relevé toutes les 0,25 s |

---

## Remarques

- Les variables `_` des boucles `for _, ... in ipairs` sont restées telles quelles (convention Lua pour une valeur ignorée).
- Les clés de tables échangées avec le serveur ou les autres modules (`texte`, `titre`, `ok`, `erreur`, `id`, `plein`, `requete`, `boutons`, `fonction`...) n'ont pas été renommées, pour ne pas casser la communication.
- Les types marqués « déduit » reposent sur l'usage dans ce script. Ils seraient confirmés avec le code des modules `Interface`, `PanneauClient` et `Config`.
