# DESIGN.md · GLA City (Galaxy City)

Document de game design.

## Concept
Jeu d'action en monde ouvert à la troisième personne, style GTA Vice City, en 1986. Graphismes low-poly aux couleurs néon (violet, rose, cyan, coucher de soleil). Le joueur incarne **Nino**, un petit voyou de Pigalle qui veut monter dans la mafia parisienne. Son ascension le mène de Paris à Londres, New York, puis la Lune et Mars, à cause de ses propres actes.

## Direction artistique
- Low-poly avec couleurs en dégradé : ciel de coucher de soleil, cycle jour et nuit, néons qui s'allument la nuit.
- Personnages en blocs fusionnés (vertex colors) : vestes, rayures, cravates, chaînes, lunettes, casquettes, cheveux longs, casques d'astronaute, aliens.
- Bâtiments haussmanniens à Paris, briques à Londres, gratte-ciel à New York, colonie futuriste sur la Lune, colonie orangée sur Mars.
- Interface : polices Bungee et Rajdhani, accents rose, cyan et jaune, bandes noires pour les cinématiques.

## Les mondes
| Zone | Contenu |
|---|---|
| Paris | Seine et péniches, Montmartre, Arc de Triomphe, Opéra, Pigalle, banque, club Le Néon, aéroport CDG. Banlieue sud : village de Saint-Lutèce, Parc des Princes, tunnel, villas VIP |
| Corridor A1 | Autoroute avec tunnel sous la Manche entre Paris et Londres |
| Londres | Conduite à gauche, Big Ben, Tower Bridge, Hyde Park, Primrose Hill, Heathrow. Banlieue sud : Little Kent, stade de Londres (arche), villas |
| New York | Manhattan, Times Square, Central Park, Brooklyn Bridge, JFK, panneaux de burgers. Île de l'Est : Long Island, stade de Manhattan, villas Hamptons, tunnel |
| La Lune | Pistes de poussière avec balises, dômes, tours néon, radars, conteneurs, navettes, épaves de soucoupes, base Luna-1 (cantine), site Apollo, mine d'hélium-3, tour Orion, drapeaux américains, gravité réduite |
| Mars | Sol rouge, ciel orangé, Phobos et Déimos, colonie Ares, serres, tours de terraformation, ruches aliens, Canon stellaire |

On voyage par la route (Paris et Londres), en avion entre les aéroports, et en fusée vers la Lune et Mars.

## Histoire (5 actes, 32 missions)
1. **Acte I, Paris** : entrée dans la Famille de Don Armand avec Lucien et Josy. Combats contre les Marseillais et les Corses, puis Raymond le Rat.
2. **Acte II, Londres** : avec Nigel, contre les Dragons (triade de Maître Long) et les Anglais de Lord Ashby.
3. **Acte III, New York** : avec Frankie, contre les Yakuzas de Kuroda et le clan Moretti. Dernière mission : voler une ogive nucléaire. Elle explose, la guerre mondiale éclate, la Terre devient inhabitable.
4. **Acte IV, la Lune, 6 mois plus tard** : « Les actes ont des conséquences. » Don Armand est mort. Avec Stella, il faut protéger le Président de la République contre les aliens d'Orion, jusqu'au combat contre l'Empereur Vega.
5. **Acte V, Mars** : avec Kaïa, on défend la colonie Ares et on récupère des cœurs d'énergie, jusqu'au combat contre la Reine Nyx, qui veut détruire la Terre avec le Canon stellaire. À la fin, les vols vers la Terre se rouvrent.

Chaque mission commence par une cinématique d'environ 10 s (caméra autour du lieu, personnages sur place, 2 ou 3 répliques, bouton Passer).

## Systèmes de jeu
- **Progression RPG** : XP, 20 niveaux, cœurs de vie, cœurs cachés et cœurs de mission. Rien n'est perdu à la mort ou à l'arrestation.
- **Armes** : armurerie façon Call of Duty accessible à tout moment, raretés (commune, rare, épique, légendaire), armes uniques sur les boss (katana enflammé, Tommy, plasma, sceptre de Nyx), armes laser dans l'espace.
- **Équipement** : jetpack (vol limité par le carburant), gilet pare-balles (100 points d'armure).
- **Pouvoir** : ralenti du temps (touche T), 6 s puis recharge.
- **Véhicules** : voitures, sport, fourgons, bus, taxis, police, blindés, motos, chevaux, hélicoptères, hélicos de combat, rovers, voitures volantes. Sons moteur différents par type.
- **Carjacking** : cinématique en 3 variantes (éjection, menace, vitre brisée). On peut aussi voler les voitures de police.
- **Dégâts** : les collisions laissent toujours au moins 1 PV. Seuls les tirs font exploser une voiture.
- **Police** : de 1 à 5 étoiles, poursuites courtes. Arrestation possible seulement à pied, après environ 3 s collé à une voiture de police, avec un avertissement.
- **Activités** : missions hélico (chasse, course d'anneaux, appui aérien), braquage de la Banque Lutèce, villas achetables (sauvegarde, soin, point de réapparition), petits boulots.
- **Intérieurs** : Banque Lutèce, club Le Néon, cantine Luna-1.

## Commandes
- PC : ZQSD/WASD pour bouger, souris pour viser, clic ou Espace pour tirer, E pour voiture, porte et action, X pour changer d'arme, B pour l'armurerie, T pour le ralenti, J pour le jetpack, H pour les missions hélico, R pour la radio, M pour la carte, Échap pour la pause.
- Mobile : joystick et boutons contextuels (VOITURE, TIRER, ARME, RALENTI, JETPACK, et TIRER en hélico).

## Contraintes techniques
- Fluidité avant tout : géométries fusionnées et instanciées, culling par zone.
- Un seul fichier HTML jouable dans le navigateur.
- Voitures en 2D sur un sol plat, circulation sur une grille de rues.
