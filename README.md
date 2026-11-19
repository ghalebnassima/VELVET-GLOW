Site e-commerce de cosmétiques réalisé en équipe. Le front-end (HTML, CSS, JavaScript) communique avec une API en PHP (réponses JSON) reliée à une base MySQL via PDO.

Aperçu
Accueil	Boutique
Accueil	Boutique
Fonctionnalités
Comptes utilisateurs : inscription, connexion et déconnexion, avec sessions PHP.
Espace « Mon compte » : modification du nom et suppression du compte (confirmation par mot de passe).
Boutique : 30 produits répartis en 6 catégories, filtres par catégorie et recherche en direct (JavaScript).
Panier : ajout et suppression de produits, total calculé côté serveur à partir des prix de la base. Sans serveur (ouverture directe du fichier), le panier bascule sur localStorage.
Favoris sur les cartes produits.
Compteur de visites enregistré dans un fichier texte (une visite comptée par session).
Prix et noms des produits synchronisés depuis MySQL via l'API.
Technologies
Domaine	Outils
Front-end	HTML5, CSS3 (Flexbox, Grid, responsive), JavaScript (DOM, fetch)
Back-end	PHP 8, PDO, sessions
Base de données	MySQL (phpMyAdmin)
Environnement	WAMP
Architecture

Les pages HTML appellent l'API PHP avec fetch(). L'API interroge MySQL et renvoie du JSON (produits, panier, utilisateur connecté). Les formulaires de compte envoient leurs données en POST directement aux scripts PHP.

velvet-glow/
├── index.html            Redirige vers l'accueil
├── acceuil.html          Page d'accueil
├── boutique.html         Catalogue, filtres et recherche
├── shopping.html         Connexion et inscription
├── compte.php            Espace « Mon compte »
├── deco.css              Styles de l'accueil
├── boutique.css          Styles de la boutique
├── shopping.css          Styles de la connexion
├── velvet.js             Panier, filtres, recherche, favoris
├── shopping.js           Bascule connexion / inscription
├── api/
│   ├── connexion.php
│   ├── deconnexion.php
│   ├── inscription.php
│   ├── utilisateur.php
│   ├── produits.php
│   ├── panier.php
│   ├── modifier_compte.php
│   ├── supprimer_compte.php
│   └── visites.php
├── config/
│   ├── database.php      Connexion PDO
│   └── fonctions.php     Fonctions communes
└── database/
    └── velvet_glow.sql   Structure et données de la base
Base de données

Base velvet_glow, composée de deux tables.

Table	Colonnes
utilisateurs	id, nom, email (unique), mot_de_passe (haché), date_creation
produits	id, nom, categorie, prix, image_url
API
Fichier	Méthode	Rôle
api/inscription.php	POST	Crée un compte
api/connexion.php	POST	Connecte l'utilisateur
api/deconnexion.php	POST	Déconnecte l'utilisateur
api/utilisateur.php	GET	Indique si un utilisateur est connecté (JSON)
api/produits.php	GET	Liste des produits (JSON)
api/panier.php	GET / POST	Lit, ajoute ou supprime un article du panier (JSON)
api/modifier_compte.php	POST	Modifie le nom de l'utilisateur connecté
api/supprimer_compte.php	POST	Supprime le compte de l'utilisateur connecté
api/visites.php	GET	Incrémente et affiche le compteur de visites
Sécurité
Injections SQL : toutes les requêtes utilisent PDO avec des requêtes préparées.
Mots de passe : hachés avec password_hash(), vérifiés avec password_verify().
Sessions : session_regenerate_id() à la connexion.
CSRF : jetons à usage unique vérifiés avec hash_equals() pour la modification et la suppression du compte.
XSS : htmlspecialchars() côté PHP et textContent côté JavaScript.
Validation côté serveur : types, longueurs, format de l'email, doublons (409).
Codes HTTP adaptés (400, 401, 403, 404, 405, 409) et erreurs techniques écrites dans les logs sans être affichées au visiteur.
Installation
Prérequis
WAMP (Apache, MySQL, PHP 8)
Étapes
Copier le dossier du projet dans C:\wamp64\www\velvet-glow.
Démarrer WAMP (icône verte).
Ouvrir phpMyAdmin (http://localhost/phpmyadmin) et importer database/velvet_glow.sql. La base velvet_glow est créée automatiquement.
Vérifier les identifiants dans config/database.php (par défaut : utilisateur root, mot de passe vide).
Ouvrir http://localhost/velvet-glow/ dans le navigateur.

Le projet doit être ouvert via localhost. En ouvrant directement un fichier HTML, le panier fonctionne en mode local mais les comptes et l'API PHP ne sont pas disponibles.
