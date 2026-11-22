# CLAUDE.md · GLA City (Galaxy City)

Instructions pour Claude quand il travaille sur ce jeu.

## Le projet en bref
- Jeu d'action en monde ouvert façon GTA, ambiance Vice City 1986, jouable dans le navigateur (PC et mobile).
- **Un seul fichier** : `index.html` (environ 2 400 lignes, 310 Ko). Three.js r128 chargé depuis cdnjs. Aucun build, aucune autre dépendance.
- Publié comme artefact claude.ai : slug `d9159ae9-0a32-40a4-b096-8da7bc24937f`, titre « GLA City ». Toujours republier sur ce même slug avec le `base_version` de la dernière publication.
- Ancien nom : Lutèce City. Les lieux parisiens gardent le mot « Lutèce » (Banque Lutèce, Ciné Lutèce, Villa Lutèce).

## Règles absolues
1. **La fluidité passe avant tout.** Géométries fusionnées par matériau et par zone (`addMerged`, `mergeGeos`), `InstancedMesh` pour les objets répétés, aucune lumière dynamique en plus, peu de meshes individuels.
2. **Rester dans le navigateur**, un seul fichier HTML.
3. **Rien au milieu des routes** : pas de poteau, lampadaire, épave ni décor sur la chaussée. Vérifier par un scan automatique après chaque ajout de décor.
4. Pas de marques, logos, clubs ni personnes réelles. Films, burgers et marques sont inventés. Les lieux réels (Big Ben, Parc des Princes) sont autorisés comme repères.
5. Le Président est fictif, sans nom ni visage réel.

## Style des réponses à l'utilisateur
- Français, écrire au masculin (« ravi », jamais « ravi(e) »).
- **Jamais de tiret long (—)** : utiliser des virgules ou des points.
- Pour les résumés, des tableaux texte simples, pas de widgets interactifs.
- L'utilisateur joue surtout sur mobile : réponses courtes.
- Expliquer honnêtement ce que le moteur ne permet pas (par exemple les voitures roulent toujours à plat).

## Méthode de travail
1. Sauvegarder avant chaque modification : `cp index.html /tmp/lutece_vNN.html`.
2. Modifier avec des scripts Python de remplacement exact (`R(ancien, nouveau, nb)` avec une assertion sur le nombre d'occurrences), rangés dans `/home/claude/scripts/`.
3. Vérifier la syntaxe : extraire le plus gros `<script>` dans `/tmp/g.js`, puis `node --check /tmp/g.js`.
4. Tester en headless avec Playwright Python + Chrome swiftshader (`/usr/bin/google-chrome`, three.min.js servi en local depuis `/home/claude/test/package/build/`). Capturer les erreurs de page, faire des captures d'écran et les regarder.
5. Publier avec `publish_files` sur le slug ci-dessus.
6. Les FPS du serveur de test (8 à 12) ne reflètent pas un vrai appareil : ils servent seulement à comparer avant et après.

## Accès debug
`window.__dbg` expose : `player`, `cars`, `peds`, `enemies()`, `allies`, `city()`, `travel(k)`, `story()`, `setStory(n)`, `startMission(n)`, `clearMission()`, `STORY`, `M`, `give(arme)`, `setMoney(n)`, `addXP(n)`, `addHeat(n)`, `openShop()`, `scene`, `camera`, `insideSolid`, `roadX()`, `roadZ()`, `CITIES`. `window.__jv=0|1|2` force la variante de carjacking.

## Repères techniques
- Constantes de monde : `OX=1300` (Londres), `OX2=3000` (New York), `IW=262` (demi-largeur de Manhattan), `OX3=5200` (Lune), `OX4=7400` (Mars), `PH=473` (demi-taille Paris/Londres), `MH=400` (demi-taille Lune/Mars).
- Villes : objet `CITIES` (paris, london, ny, moon, mars) avec `ox`, `half`, `rx`, `rz`, `hw(z)`, `left`, `base` (canvas de minicarte 960 px). `setCity()`, `cityOf(x)`, `SPACE()` (Lune ou Mars).
- Collisions 2,5D sur grille (`CELL=40`, `OFF=520`) : `addBox` (renvoie la boîte), `addCircle`, `collide`, `insideSolid`, `groundAt`, `los`, `los3`. Les grandes boîtes (`f=1` eau, `f=2` bords) sont ignorées quand `x<-1000` (zone des intérieurs).
- Intérieurs à `x=-1600` (banque, club, cantine) : `DOORS`, `nearDoor`, `useDoor`, `INSIDE`.
- Missions : tableau `STORY` (32 missions). Types : fetch, pickdrop, destroy, heat, collect, boss, goto, fly, escort, defend. Champs : `brief`, `outro`, `allies`, `give`, `final`, `cine`. Systèmes parallèles : `HM` (missions hélico), `HEIST` (braquage).
- Cinématiques : `startMC` (avant mission, environ 10 s), `apocalypse()` (fin de l'acte 3), `startJack` (carjacking, 3 variantes).
- Voitures : `CT` (types), `makeCar`, rôles ai, parked, police, player, wreck, target, eheli. Les collisions laissent au moins 1 PV ; seuls les tirs (`SH(car)` marque `shotAt`) détruisent une voiture.
- Sauvegarde : `localStorage`, `saveData()` / `load()`, champ `ver:2`.
- Fichiers liés : `MEMORY.md` (historique et décisions) et `DESIGN.md` (game design).
