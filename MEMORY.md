# MEMORY.md · GLA City

Mémoire du projet : préférences de l'utilisateur, décisions prises, état actuel et historique. Mise à jour au 21 novembre 2026.

## Préférences de l'utilisateur
- Priorité numéro un : la fluidité. Il joue surtout sur mobile, et le jeu lui paraît fluide.
- Il déteste les obstacles au milieu des routes (poteaux, lampadaires, arbres). Il l'a signalé plusieurs fois.
- Il veut un jeu facile et agréable : police peu agressive, poursuites courtes, rien de perdu à la mort ou à l'arrestation.
- Il refuse les explosions dues aux collisions. Seuls les tirs peuvent faire exploser une voiture.
- Il aime les cinématiques courtes, les ajouts de diversité et les boutons de test pour sauter à un acte.
- Réponses en français, au masculin, sans tiret long, avec des tableaux simples.

## Décisions de conception
- Un seul fichier HTML publié en artefact. Pas de Steam ni d'appli native pour l'instant, une appli web suffit.
- Les voitures roulent toujours à plat : pas de ponts surélevés, rampes ou avenues en diagonale. La circulation suit la grille des rues.
- Les Roms sont représentés comme un camp neutre (roulottes, chevaux), pas comme un gang.
- Les Marseillais portent un t-shirt blanc avec une bande bleue (#1f6fd0) en travers de la poitrine, sans logo.
- La circulation est limitée à 12 voitures autour du joueur.
- L'ancien stade rond de Paris (30, 390), qui bloquait deux rues, a été remplacé par un square. Le Parc des Princes est maintenant en banlieue sud.
- Central Park : une seule route (la transversale z=-280). Le reste est une pelouse libre où l'on roule. Les voitures IA évitent le parc (`CITIES.ny.blocked`), et l'héliport de New York est sur la pelouse en (OX2+95, -345).
- Lune : seulement 3 × 3 pistes de poussière (x et z = -240, 0, 240), pas de grille de rues. Les bâtiments sont décalés au hasard, avec des zones vides et des rochers, pour un aspect d'avant-poste.
- Cinématiques de mission en deux plans : les personnages qui parlent se réunissent autour du joueur, puis un plan final de 3,6 s montre l'objectif avec un bandeau « 🎯 OBJECTIF » (`cineShot(d)`, `#mcGoal`). Voiture à voler, fourgon ou cible à détruire, boss, lieu de rendez-vous, colis, zone à défendre, aéroport. La voiture ou la cible est créée dès la cinématique (`PRE.car`, `PRE.target`) puis réutilisée par `startMission`, et le boss est apparu à l'avance. La caméra choisit un angle dégagé et monte au-dessus des immeubles si la vue est bouchée.
- Boss dans des lieux à thème : Marseillais au Parc des Princes, Corses sur la place du village de Saint-Lutèce, Dragons devant une pagode à Hyde Park, Lord Ashby dans une villa de la banlieue de Londres, Yakuzas au jardin zen avec torii de Central Park, Moretti dans une villa de Long Island, Vega devant la tour Orion, Nyx devant le Canon stellaire.
- Versailles : zone ouest de Paris (x de -860 à -473, z de -232 à 42), prolongement du boulevard z=-120 (Champs-Élysées). Château en U, grille dorée, cour d'honneur, parterres, bassins de Latone et d'Apollon, Grand Canal. Boss optionnel hors histoire : le Marquis de Montclair (gang « cour »), qui donne le Tromblon du Marquis. Cœur caché dans les jardins.
- Château de Monte-Cristo : banlieue sud de Paris en (-185, 800), accès par une route depuis l'impasse des villas. Boss bonus hors histoire : le Comte de Monte-Cristo (gang « comte »), qui donne le Pistolet de duel du Comte. Cœur caché derrière le château.
- Arc de Triomphe : seuls ses deux piliers bloquent, on passe dessous (les voitures IA évitent toujours ce carrefour).
- iPhone : la toile ne dépend plus de 100vh. Elle suit la zone visible réelle (`visualViewport`) et se recalcule plusieurs fois après une rotation. `viewport-fit=cover` et les marges de sécurité (encoche, Dynamic Island) sont appliqués au HUD et aux boutons. En paysage bas, les boutons passent en grille 2 × 2.
- Paris moins carré : 3 rues courbes en quart de cercle, bordées d'immeubles en arc (pâtés `CURVE_BLOCKS`), et une « Place ronde » avec anneau routier et fontaine (`ROUND_BLOCK`). Les voitures IA restent sur la grille, le joueur peut rouler partout.
- Restaurants où l'on entre et mange (50 €, santé au maximum) : bistrot Chez Lucien (Paris), pub The Red Lion (Londres), Sunset Diner (New York). Leurs portes sont placées automatiquement sur une façade (`facadeDoor`), et leurs intérieurs sont à x=-1600.
- Accès : la Lune n'est accessible qu'à partir de l'histoire 20 (après l'apocalypse de l'acte 3) et Mars à partir de 26, sauf en mode libre. Le menu des vols cache ces destinations et `fly()` les refuse.
- Les armes spatiales (blaster, fusil laser, canon rail) n'apparaissent à l'armurerie et dans le butin qu'à partir de l'acte 4.
- Les briefings des boss indiquent leurs nouveaux lieux.
- Musique : l'utilisateur trouvait l'ancienne radio synthwave trop lancinante. Elle est remplacée par 3 radios chill générées en direct (libres de droits) : Jazz Café Lutèce (piano électrique, contrebasse qui marche, balais, swing), Lo-fi Nuit et Bossa Néon. La touche R change de station en voiture.
- Flèche GPS : vraie flèche en SVG avec une tige, pour bien voir la direction.
- Mobile : jeu en paysage. Plein écran et verrouillage de l'orientation au premier bouton touché (Android). En portrait, un écran « Tourne ton téléphone » s'affiche (pour iOS).
- Hébergement prévu : GitHub `Slyzze/gla-city` puis Vercel. Le connecteur GitHub n'est pas encore relié, donc l'envoi se fait à la main pour l'instant.
- Détails graphiques : tout le décor ajouté utilise Math.random (pas `rnd()`) pour ne pas décaler la génération seedée. Le mobilier urbain reste sur les trottoirs ou au sol (marquages), jamais sur la chaussée. Les immeubles haussmanniens n'ont pas de volets, seules les maisons de village en ont.
- Nouveaux quartiers (hors grille, sans circulation IA) : Montmartre au nord de Paris (x -320..320, z -820..-473), butte en relief (`hillH`, sommet 22 m au Sacré-Cœur), place du Tertre, vignes, Moulin de la Galette qui tourne (`MILLS`). La Défense au nord-ouest (x -920..-473, z -620..-232) : tours de verre, Grande Arche, esplanade avec fontaine. Berges de Seine entre Paris et Versailles (rive sud jusqu'à z 110). Docklands à l'est de Londres (x 1773..2140, z -300..300) : Canary Wharf, bassin, grues, terminal à conteneurs. Le petit Sacré-Cœur du centre (90, -210) est remplacé par un square, le vrai est sur la butte. Il est posé sur une terrasse de 8 m (x -30..30, z -735..-688) avec un grand escalier solide de 20 m de large depuis le bout de la rue x=0, des rampes et des lampadaires. Un escalier latéral côté est (x 13..17) monte au toit-terrasse (`SC_TOP`) : balustrade, longues-vues, bancs, panneau PANORAMA. Premier passage sur le toit : +200 XP (`panoSeen`). Les marches sont des `addBox` de 0,3 m (le joueur monte car `groundAt` les prend comme sol).
- Relief et sauts : `terrH(x,z)` = butte + tremplins (`RAMPS`), utilisé par `groundAt`, les voitures et la caméra. `vehY(c,dt)` gère la hauteur, le tangage et les sauts du véhicule du joueur (gravité 18). Bonus « CASCADE » si saut > 18 m. 5 tremplins : 2 aux berges (sauter la Seine), 2 aux Docklands (sauter la Tamise), 1 sur l'esplanade de la Défense (au-dessus de la fontaine). Il faut environ 110 km/h.
- Stations de taxi (`TAXIS`, 24 au total : 9 Paris, 9 Londres, 6 New York), anneau jaune sur le trottoir. Touche E ou bouton VOITURE à pied, menu `#tmenu`, 20 € la course, fondu au noir.
- Découpage en zones (`chunkScene`, cellules de 420 m) après `initWorld` : les grands meshes fusionnés sont coupés par cellule, et les matériaux unis (Lambert sans texture) sont regroupés en un seul mesh à couleurs de sommets par cellule. Paris : environ 390 à 440 appels de dessin et 290k triangles visibles (avant : 355 et 380k, pour une carte plus petite). Dans les nouveaux quartiers : 60 à 230 appels.
- Grille de collision : OFF=940 (couvre z -940..940). Minicartes agrandies (`ext()`) : Paris bo=935, Londres bo=855, avec banlieues, quartiers et taxis. La grande carte adapte la taille du texte (`FS`).

- Mode Zombie (bouton « 🧟 MODE ZOMBIE » du menu) : labyrinthe généré (recursive backtracker + boucles + salles) en x 3700..4700 (trou dans la frontière, boîtes d'eau de New York raccourcies à OX2+640). Cases de 12 m, murs de 6,5 m fusionnés, 5 thèmes tous les 5 niveaux (égouts, hôpital, métro, laboratoire, base militaire). Trouver 3 fusibles puis la sortie, boss tous les 5 niveaux (exit fermée tant qu'il vit). Zombies = objets `enemies` avec `zomb:true` (tir, mêlée, roquettes, explosions marchent), IA dans `zUpdate` avec champ de flux BFS 2 fois par seconde. Types : marcheur, coureur, rampant, cracheur, explosif, costaud. Bonus : argent ×2, mort instantanée, munitions, soin, bombe, invincible. Armes en plus réservées au mode : lance-flammes (enflamme), minigun, arbalète (`zonly`). Améliorations à l'armurerie (`zPerks`) : dégâts, cœurs, régénération, deuxième chance. Profil séparé `gla_zombie` (argent, armes, niveau, record) ; en mode zombie, `store.set('lutece_save')` est redirigé vers `zSave`, la partie histoire n'est jamais touchée. Point de contrôle tous les 5 niveaux. « Menu principal » recharge la page.

- Cartes zombie faites à la main (`ZMAPS`, grilles ASCII de 40 × 30 à 34 cases de 4 m, générées par scripts/zmaps.py) : Ciné Lutèce (hall, grande salle avec écran et rangées de fauteuils, coulisses, loges, cabine, ruelle), Asile Sainte-Odile (accueil, cour avec arbre mort et fontaine, dortoirs, réfectoire, soins), Marais des Brumes (cabanes sur pilotis, pontons, eau profonde infranchissable, eau peu profonde qui ralentit). Codes : # mur, . sol, S départ, Z apparition, 1-9 portes à acheter (`zDoorTry`, touche E), c fauteuil, e écran, k table, p pilier, b lit, x caisses, f fontaine, T arbre, w eau profonde, s eau peu profonde, = ponton. Manches sans fin (`zMapWave`), Mastodonte toutes les 5 manches, record par carte `ZP.rec`. Menu de choix `#zsel` (labyrinthe ou une des 3 cartes). L'utilisateur avait demandé de copier des cartes d'un jeu existant : refusé, cartes originales à la place.

- Mode zombie à la 1re personne par défaut (`zFps`, caméra à hauteur d'yeux, arme visible `zViewModel` couleur de rareté, near 0,08). Ordinateur : clic = verrouillage de la souris, souris pour tourner, Q/D pour se décaler. Mobile : joystick qui tourne, visée assistée. Touche V ou bouton 👁 VUE pour la 3e personne (`ZP.fps`).
- Bonus aux murs (machines, pour la partie en cours, `ZPERKS`) : RAGE (dégâts ×2, 3 000 €), ANGE GARDIEN (te relève, à racheter, 2 500 €), ADRÉNALINE (cadence +40 %, 2 500 €), CŒUR DE FER (+1 cœur, jusqu'à +6, 1 500 € puis +1 000 €). Armes aux murs (`ZWALLW`) : uzi, fusil à pompe, arbalète, fusil d'assaut, lance-flammes, minigun, avec rareté tirée au hasard ; si déjà possédée, moitié prix pour munitions + nouveau tirage. Positions dans `stations` de chaque carte ; au hasard dans le labyrinthe.
- Armes uniques de boss (zonly, rareté UNIQUE) : Fusil putride, Bobine Tesla (chaîne), Faux de l'asile (mêlée enflammée), Canon du Général (roquettes), Rayon givrant. Les boss lâchent une unique non possédée + une épique + une rare. 2,5 % de chance d'arme normale sur les zombies.
- Couleurs de rareté (tout le jeu) : commune verte, rare bleue, épique violette, légendaire rouge, unique dorée.
- Cœurs permanents de l'armurerie zombie : jusqu'à +8.

- Nom du personnage : écran « COMMENT T'APPELLES-TU ? » après NOUVELLE PARTIE (`#nameov`, 14 caractères, lettres/chiffres/espace/'/-). `PNAME` est sauvegardé (`saveData().name`) et `nm()` remplace « Nino » à l'affichage (dialogues, cinématiques, toasts).
- Compte en ligne optionnel avec Supabase (`CLOUD={url,key}` vide par défaut = pas de bouton). E-mail + mot de passe, table `saves` (user_id, data jsonb, updated_at) avec RLS. Sauvegarde histoire + zombie envoyée 4 s après chaque `store.set`, la plus récente gagne au chargement. Étapes dans README.md.

- 6 missions ajoutées (38 au total), insérées par `STORY.splice` après l'application des cœurs et des alliés (indices d'origine) : Le racket du Néon (boss `neon` Gégé le Gérant dans le club, `until` = ne réapparaît plus après), La herse (goto `herse` sur les Champs, pose la herse `placeSpike`) puis Le fourgon blindé (fourgon crevé sur la herse, 3 convoyeurs `CONVOY`), La course à la mairie (type `cands` : 3 candidats inventés, E pour intimider ou les éliminer avec 2 étoiles ; tout intimider = récompense ×1,5), Le casse de la Banque Lutèce (type `heist`, réutilise le braquage, fin = `completeMission`), La mine d'hélium-3 (defend 60 s) et Le rover-citerne (fetch vers le spatioport) avant le vol pour Mars.
- Seuils d'histoire calculés : `S_NY`, `S_ACT4`, `S_MARS`, `S_ACT5`, `S_NEON` (plus de nombres en dur). Sauvegarde ver 3 ; les anciennes (ver 2) sont converties par `mapOldStory`. Le GPS vers une cible à l'intérieur d'un bâtiment pointe la porte (`routeT`).

- Véhicules ajoutés : Tout-terrain (style jeep, arceau, roue de secours) dans Paris, Supercar Furia et Supercar Taureau (aileron, prises d'air) à Londres et dans ses villas, 4x4 du FBI (noir, gyrophares, pare-buffle) envoyé par la police à New York ou à partir de 3 étoiles. Pas de vraies marques : l'utilisateur avait demandé Jeep, Ferrari et Lamborghini, remplacés par des modèles inventés.

- Grand Tour de Paris (hélico, type HM `tour`) : 9 anneaux `TOUR_PTS` (Opéra, Sacré-Cœur, Arc de Triomphe, Grande Arche, sous la tour Eiffel, Montparnasse, Notre-Dame, Louvre, héliport), 150 s, 6 000 €, record `tourBest`. Se lance tout seul la première fois qu'on prend un hélico à Paris (`tourSeen`, sauvegardé), puis rejouable depuis le menu HÉLICO (Paris seulement). Il met la mission d'histoire en pause au lieu de l'annuler (`keepM`).

- Zombie, réglages : tirs discrets (traînées à 40 % de luminosité et semi-transparentes, particules plus petites et plus sombres, pas d'éclair de bouche en vue 1re personne). Cartes : 12 manches puis boss final (`ZFINAL` : Le Projectionniste, La Directrice, La Bête des Brumes, 9 000 PV), victoire `zMapWin` = 15 000 € + arme unique, écran VICTOIRE, carte marquée « ✓ Terminée » (`ZP.won`). Le ralenti du temps (T ou bouton RALENTI) s'applique aux zombies (`dt*TS`).
- Katana renforcé partout (dégâts 110, portée 4,6, cadence 0,36) et katana enflammé (170, portée 5,2). En mode zombie, mêlée ×1,5 et les coups repoussent les zombies. Armurerie zombie : amélioration « 🔥 KATANA ENFLAMMÉ » (8 000 €, il faut le katana).

- Mobile (Safari sur Vercel) : `resize()` écrit --vw, --vh, --vt, --vl (taille et décalage de la zone visible `visualViewport`) ; canvas, HUD, overlays, cinématiques et boutons (#btns ancrés par le haut avec translateY(-100 %)) suivent ces variables, et le scroll du visualViewport relance le calcul. Balises « apple-mobile-web-app-capable » pour le plein écran depuis l'écran d'accueil.

- Anneaux hélico refaits : course aérienne = chemin vers l'avant (cap ±20°, 75 à 105 m entre anneaux, jamais de demi-tour), chaque anneau 12 m au-dessus des toits (`skyH`) ; Grand Tour = boucle Opéra, Sacré-Cœur, Grande Arche, Arc de Triomphe, tour Eiffel, Montparnasse, Notre-Dame, Louvre, héliport, tous en plein ciel, anneaux ×1,8. Pendant ces deux courses, l'hélico ne heurte plus les bâtiments.
- Montée en voiture avec la bande (`startBoard`/`updBD`, état 'board', 1 à 3 s, passable) : les alliés marchent jusqu'aux portières, caméra de côté. Aussi après un carjacking. À la descente, ils ressortent autour de la voiture.
- Niveaux jusqu'à 50 (`MAXLV`) : +1 cœur tous les 5 niveaux (max 30), dégâts +3 %/niveau jusqu'à 20 puis +1,5 %, primes aux niveaux 25 (gilet + 5 000 €), 30, 40, 50 (« LÉGENDE DE GLA CITY », 50 000 €).
- Circuit de Lutèce (Formule 1, gratuit, accès libre) : banlieue sud-est x 300..690, z 473..830 (boîtes de frontière ouvertes), tracé `CIRC` en spline fermée, vibreurs, damier, stands, 2 tribunes, pneus. 3 Formule 1 garées devant les stands (`F1_SPOTS`, réapparaissent). Chrono au tour (`lapUpdate`, 4 secteurs, record `LAP.best` sauvegardé, +500 € par record), affichage `#lapt`. Station de taxi « Circuit de Lutèce ».

- Graphismes zombie détaillés (version 1795347982-5181) : textures procédurales 256 px en cache (`zTex`, `ZTHK` par thème : briques avec mousse, carrelage, panneaux et blocs avec bandes de danger, velours, planches ; sols béton mouillé, carrelage, moquette, boue), sols texturés fusionnés (`zFloor`), plinthes, corniches, tuyaux et appliques avec halo additif le long des murs (`zWallExtras`, une seule géométrie par type). Zombies : plaies, côtes, pustules (cracheur), furoncles (kamikaze), armure (tank) fusionnées dans `zGeo`, yeux lumineux (`zEyeG`/`zEyes`). Armes en main par famille (`zWType`, `vbox` avec faces ombrées, 1 seul draw call, manche et main). Debug `__dbg.setW(k)`. Même nombre de triangles et FPS identiques au test.

- Mobile, mode zombie : le bouton VOITURE (`b1`) devient « ACHETER » près d'une station ou d'une porte, « ACTIVER » ailleurs, et les indices disent « Bouton ACHETER » (version 1795348470-d03c).

- Forge Néon (version 1795360281-f7a7) : machine d'amélioration d'arme (perk `forge`, 5 000 €, enclume et bandes de danger), une par niveau de labyrinthe et une par carte placée automatiquement (`zPlaceForge`, mur libre à ~60 % de la distance du départ, loin des portes et stations, `ZM.forgeAt`). Améliore l'arme en main pour la partie en cours (`ZM.up`, remis à zéro par `zResetRun`). Pistolet → PISTOLETS JUMEAUX : deux pistolets en main, tirent des roquettes alternées gauche/droite (110 dégâts, rayon 5 m, sans dégâts pour le joueur via `explosion(...,safe,edm)`). Autres armes : nom + 🔥, dégâts ×2, balles enflammées (brûlure 2,5 s), traînées orange, munitions remplies. Mêlée : ×1,6 et lame enflammée. Inspiré du principe de Call of Duty mais nom et design originaux.

## État actuel (version publiée 1795360281-f7a7)
- 5 zones : Paris, Londres, New York (Manhattan et île de l'Est), la Lune, Mars.
- Banlieues sud de Paris et de Londres : village, stade, tunnel sous une colline, villas VIP, une villa achetable.
- 38 missions sur 5 actes, avec des cinématiques avant chaque mission (plan final sur l'objectif).
- Boutons de test sur l'écran d'accueil : New York, acte 4 et acte 5. Ils sont à retirer plus tard, à la demande de l'utilisateur.
- Équipement : jetpack (30 000 €, niveau 8) et gilet pare-balles (5 000 €).
- Passe détails (version 1795280591-7b9d) :
  - Voitures : mesh détail en cache par type (`carDetail(kind)`, 1 draw call par voiture) avec calandre, pare-chocs, plaques, rétros, poignées, enjoliveurs, vitres teintées. Faisceaux de phares au sol la nuit (`carBeamM`, invisible le jour), feux arrière plus vifs la nuit. Exclus : moto, cheval, hélico, rover, voiture volante.
  - Personnages : blancs des yeux, sourcils, oreilles, semelles. Passants : coiffures `hs` (short, afro, bald, mohawk, bun, pony, fringe), sacs `bag`, montre, balancement à la marche.
  - Bâtiments : bucket `DET` (cheminées, antennes, portes, encadrements, enseignes) vidé dans `flushShared()`. Paris : corniche haute, balcon filant au 5e, cheminées (2 max par toit), antennes, portes cochères, enseignes en drapeau. Antennes à Londres et New York. Volets sur les maisons de village.
  - Rues : passages piétons à chaque carrefour, plaques d'égout, bancs, colonnes Morris, terrasses de café et kiosques à Paris. Tout fusionné par matériau dans `DETAILS`.
  - Coût : environ +220k triangles, FPS inchangés dans la marge de mesure. `__dbg.setDay(v)` ajouté pour tester la nuit.

## Bugs corrigés notables
- Le modèle du joueur restait au sol pendant les sauts (priorité d'opérateur dans `player.g.position.set`).
- Une ligne restante dans le son moteur provoquait une erreur à chaque image en voiture.
- Des lampadaires décalés de 17 m tombaient au milieu des rues perpendiculaires. Ils sont maintenant filtrés dans `makeLamps` et pour les pylônes lunaires.
- La mine lunaire et une épave en feu étaient posées sur des pistes.
- La caméra traversait les murs des intérieurs (lancer de rayon fin quand `INSIDE`).

## Historique des grandes étapes
1. Version Marseille, puis passage à Paris.
2. Systèmes RPG : XP, 20 niveaux, cœurs, cœurs cachés, rien de perdu à la mort.
3. Armurerie façon Call of Duty, raretés de butin, armes uniques, boss, katana enflammé.
4. Motos, hélicoptères, parachute, chevaux, bus, taxis, statistiques des voitures.
5. Histoire en 3 actes (Paris, Londres, New York), alliés, choix OUI/NON au début.
6. Visée à la souris, tir depuis les véhicules, police adoucie.
7. Acte 4 sur la Lune, puis refonte : vol d'une ogive, apocalypse, « 6 mois plus tard », aliens d'Orion, escorte du Président.
8. Acte 5 sur Mars : colonie Ares, Reine Nyx, Canon stellaire, missions de défense.
9. Ralenti du temps, hélicos de combat, missions hélico, intérieurs, braquage de banque.
10. Banlieues, stades, villas, tunnels, ronds-points, péniches, cinématiques de mission.
11. Carjacking cinématique, jetpack, gilet, sons moteur par véhicule, eau animée.
12. Panneaux publicitaires (cinéma à Paris, burgers à New York), nouveau nom GLA City.
13. Collisions limitées à 1 PV, explosions seulement par tirs, vol de voitures de police, plus d'arrestation en voiture.
14. Passe détails graphiques : voitures, personnages, façades, toits, mobilier urbain.
15. Cinématiques de mission avec un plan final sur l'objectif.
16. Carte agrandie : Montmartre, La Défense, berges de Seine, Docklands, tremplins, taxis, découpage en zones.
17. Sacré-Cœur avec grand escalier et toit-terrasse.
18. Mode Zombie dans un labyrinthe.
19. Trois cartes zombie originales avec manches et portes à acheter.
20. Zombie en vue 1re personne, bonus et armes aux murs, armes uniques de boss.
21. Nom du personnage, connexion Supabase optionnelle, 6 nouvelles missions.
22. Graphismes du mode zombie détaillés (sols, murs, monstres, armes) sans perte de fluidité.
23. Forge Néon : amélioration d'arme en mode zombie (pistolets jumeaux à roquettes, balles enflammées).

## Idées et demandes en attente
- Retirer les boutons de test quand l'utilisateur le demandera.
- Possible : circulation et passants dans les nouveaux quartiers.
- Possible : compteur de FPS affichable, intérieurs à Londres, New York et Mars, éclairage du club.
