# Sortir

Plateforme web permettant aux stagiaires et anciens stagiaires de l'ENI d'organiser des sorties entre eux.
Projet de groupe réalisé pendant la formation ENI.

## Fonctionnalités

- Inscription avec vérification par email, connexion, réinitialisation du mot de passe
- Création, modification, publication et annulation de sorties
- Inscription / désinscription aux sorties
- Gestion des lieux (avec carte Leaflet) et des sites ENI
- Groupes de participants
- Administration : liste des utilisateurs, activation / désactivation, suppression, import CSV (`csv/users.csv`)

## Stack

- **Backend** : Symfony 6.4, PHP 8.3, Doctrine
- **Frontend** : Twig, Vue 3, Vite, Tailwind CSS, Flowbite, Leaflet
- **Base de données** : PostgreSQL 16
- **Conteneurisation** : Docker + Docker Compose

## Lancer le projet avec Docker (recommandé)

Prérequis : Docker, Docker Compose et Make.

```bash
make install
```

Cette commande construit les images, démarre les services, crée la base, installe les dépendances PHP / Node et exécute les migrations. Pour charger les données de démo :

```bash
make symfony-fixtures
```

| Service           | URL                                            |
| ----------------- | ---------------------------------------------- |
| Application       | <http://localhost>                             |
| Serveur Vite      | <http://localhost:5173>                        |
| PgAdmin           | <http://localhost:8181> (`admin@admin.com` / `password`) |

Connexion à la base depuis PgAdmin : serveur `postgres`, utilisateur `symfony`, mot de passe `symfony`, base `sortir`.

`make help` liste toutes les commandes disponibles (`up`, `down`, `logs`, `shell`, `db-reset`, `npm-build`…).

## Lancer le projet sans Docker

Prérequis : PHP ≥ 8.3, Composer, Node ≥ 20, la [CLI Symfony](https://symfony.com/download) et un PostgreSQL local.

```bash
composer install
npm install
```

Renseigner `DATABASE_URL` dans un fichier `.env.local`, par exemple :

```bash
DATABASE_URL="postgresql://user:password@127.0.0.1:5432/sortir?serverVersion=16&charset=utf8"
```

Puis créer la base et charger les données :

```bash
symfony console doctrine:database:create
symfony console doctrine:migrations:migrate
symfony console doctrine:fixtures:load
```

Lancer le serveur Symfony et Vite (dans deux terminaux, ou en une commande avec `concurrently`) :

```bash
symfony serve
npm run dev
# ou : npm i -g concurrently && npm run serve
```

## Compte de démo

Après chargement des fixtures : `alex` / `password` (administrateur).

## Structure

```
src/
├── Controller/     # Contrôleurs (sorties, groupes, lieux, utilisateurs, sécurité…)
├── Entity/         # Entités Doctrine (Event, Group, Location, Site, User)
├── Form/           # Formulaires Symfony
├── Repository/     # Requêtes Doctrine
├── Service/        # Logique métier
├── DataFixtures/   # Données de démo
└── Security/       # Authentification
assets/             # Front (Vue, Stimulus, Tailwind)
templates/          # Vues Twig
migrations/         # Migrations Doctrine
docker/             # Configuration Nginx
```

## Équipe

Projet réalisé par Antoine Coulon, Alex Rovere, Ghislain et Justine.
