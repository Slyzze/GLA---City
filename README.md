# GLA City (Galaxy City)

Jeu d'action en monde ouvert façon GTA, ambiance 1986, jouable dans le navigateur.
Paris (avec Montmartre, La Défense, Versailles), Londres (avec les Docklands), New York, la Lune et Mars, avec 38 missions, des tremplins et des taxis. Mode Zombie en labyrinthe depuis le menu principal.

## Jouer
Ouvre `index.html` dans un navigateur, ou utilise le lien Vercel.

## Hébergement
Site statique : un seul fichier `index.html`, sans build. Sur Vercel : « Add New », puis « Project », choisir ce dépôt, puis « Deploy ».

## Docs
- `DESIGN.md` : game design
- `MEMORY.md` : historique et décisions
- `CLAUDE.md` : consignes de développement

## Connexion et sauvegarde en ligne (optionnel)
Sans réglage, le jeu sauvegarde seulement sur l'appareil. Pour activer « Se connecter » :
1. Crée un projet gratuit sur supabase.com.
2. Dans SQL Editor, lance :
```sql
create table saves (
  user_id uuid primary key references auth.users on delete cascade,
  data jsonb,
  updated_at timestamptz default now()
);
alter table saves enable row level security;
create policy "chacun sa sauvegarde" on saves for all
  using (auth.uid() = user_id) with check (auth.uid() = user_id);
```
3. Dans Project Settings > API, copie le « Project URL » et la clé « anon public ».
4. Dans `index.html`, remplace `const CLOUD={url:'',key:''};` par tes deux valeurs (la clé anon est faite pour être publique, la règle ci-dessus protège les données).
5. Dans Authentication > URL Configuration, mets l'adresse Vercel du jeu dans « Site URL ».
