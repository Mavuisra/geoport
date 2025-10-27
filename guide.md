
# 📋 Guide d'Utilisation - Géoportail d'Assainissement

## 🎯 Vue d'ensemble

Le **Géoportail d'Assainissement** est une application web complète pour la gestion des services d'assainissement urbain. Elle permet de gérer les brigades, les points de collecte, les missions, les signalements et bien plus encore.

---

## 🚀 Démarrage Rapide

### 1. Accès à l'application
- **URL** : `http://localhost:3000`
- **Connexion** : Utilisez vos identifiants fournis par l'administrateur
- **Navigateur recommandé** : Chrome, Firefox, Safari ou Edge (version récente)

### 2. Première connexion
1. Ouvrez votre navigateur
2. Accédez à `http://localhost:3000`
3. Cliquez sur "Se connecter"
4. Entrez votre nom d'utilisateur et mot de passe
5. Cliquez sur "Connexion"

---

## 🏠 Tableau de Bord

Le tableau de bord est votre page d'accueil qui affiche :
- **Statistiques générales** : Missions, véhicules, points de collecte
- **Missions du jour** : Liste des missions programmées
- **Véhicules actifs** : État des véhicules en service
- **Notifications** : Alertes importantes
- **Actions rapides** : Accès direct aux fonctions principales

### Actions rapides disponibles :
- ➕ **Nouvelle mission** : Créer une mission
- 🚛 **Voir véhicules** : Consulter les véhicules
- 📍 **Ajouter point** : Ajouter un point de collecte
- 📊 **Rapports** : Générer des rapports

---

## 👥 Gestion des Brigades

### 📋 Brigades
**Accès** : Menu → Brigades → Brigades

**Fonctionnalités** :
- **Voir la liste** des brigades existantes
- **Créer une nouvelle brigade** avec le bouton "Nouvelle brigade"
- **Modifier** les informations d'une brigade
- **Supprimer** une brigade (si autorisé)

**Création d'une brigade** :
1. Cliquez sur "Nouvelle brigade"
2. Remplissez le formulaire :
   - Nom de la brigade
   - Description
   - Responsable
   - Zone d'intervention
3. Cliquez sur "Créer"

### ⏰ Horaires
**Accès** : Menu → Brigades → Horaires

**Fonctionnalités** :
- **Consulter** les plannings des brigades
- **Créer un nouveau planning** avec "Nouveau planning"
- **Modifier** les horaires existants
- **Filtrer** par brigade ou période

### 👤 Membres
**Accès** : Menu → Brigades → Membres

**Fonctionnalités** :
- **Gérer** les membres des brigades
- **Ajouter** de nouveaux membres
- **Assigner** des membres aux brigades
- **Modifier** les informations des membres

---

## 📍 Points de Collecte

### 🗺️ Gestion des Points
**Accès** : Menu → Points de Collecte → Points de Collecte

**Fonctionnalités** :
- **Voir la carte** des points de collecte
- **Ajouter un nouveau point** avec "Nouveau point"
- **Modifier** les informations d'un point
- **Supprimer** un point (si autorisé)
- **Filtrer** par type, commune, statut

**Création d'un point** :
1. Cliquez sur "Nouveau point"
2. Remplissez le formulaire :
   - Nom du point
   - Type (décharge, station, etc.)
   - Adresse complète
   - Coordonnées GPS (latitude/longitude)
   - Commune et quartier
   - Description
3. Cliquez sur "Créer"

### 📸 Photos des Points
**Accès** : Menu → Points de Collecte → Photos

**Fonctionnalités** :
- **Consulter** toutes les photos des points
- **Ajouter une photo** avec "Ajouter une photo"
- **Modifier** la description d'une photo
- **Supprimer** une photo
- **Filtrer** par commune ou point de collecte

**Ajout d'une photo** :
1. Cliquez sur "Ajouter une photo"
2. Sélectionnez le point de collecte
3. Choisissez une image (JPG, PNG, GIF - Max 5MB)
4. Ajoutez une description (optionnel)
5. Cliquez sur "Ajouter"

### 👁️ Visites des Points
**Accès** : Menu → Points de Collecte → Visites

**Fonctionnalités** :
- **Consulter** l'historique des visites
- **Enregistrer une visite** avec "Nouvelle visite"
- **Filtrer** par point, date, visiteur
- **Exporter** les données de visites

### ⚠️ Signalements (Décharges Illégales)
**Accès** : Menu → Points de Collecte → Signalements

**Fonctionnalités** :
- **Consulter** tous les signalements
- **Créer un signalement** avec "Nouveau signalement"
- **Filtrer** par statut, commune, gravité
- **Rechercher** par adresse ou description
- **Suivre** l'évolution des signalements

**Création d'un signalement** :
1. Cliquez sur "Nouveau signalement"
2. Remplissez le formulaire :
   - **Adresse** (obligatoire) : Lieu précis de la décharge
   - **Description** (obligatoire) : Détails du problème
   - **Commune** : Nom de la commune
   - **Quartier** : Nom du quartier
   - **Coordonnées GPS** (obligatoire) : Latitude et longitude
   - **Gravité** : Faible, Moyen, Élevé, Critique
   - **Photo** (optionnel) : Image de la décharge
3. Cliquez sur "Signaler"

**Statuts des signalements** :
- 🔴 **Signalé** : Nouveau signalement
- 🟡 **En cours d'investigation** : En cours de traitement
- 🟢 **Résolu** : Problème résolu
- ⚫ **Fermé** : Dossier clos

---

## 📋 Missions

### 📝 Gestion des Missions
**Accès** : Menu → Missions → Missions

**Fonctionnalités** :
- **Consulter** toutes les missions
- **Créer une mission** avec "Nouvelle mission"
- **Modifier** une mission existante
- **Assigner** des brigades et véhicules
- **Suivre** le progrès des missions

**Création d'une mission** :
1. Cliquez sur "Nouvelle mission"
2. Remplissez le formulaire :
   - **Titre** : Nom de la mission
   - **Description** : Détails de la mission
   - **Type** : Collecte, Maintenance, Inspection, etc.
   - **Priorité** : Faible, Moyenne, Haute, Critique
   - **Dates** : Date prévue et échéance
   - **Organisation** : Organisation responsable
   - **Brigade** : Brigade assignée
   - **Points de collecte** : Points à visiter
3. Cliquez sur "Créer"

### 📍 Points de Mission
**Accès** : Menu → Missions → Points de Mission

**Fonctionnalités** :
- **Consulter** les points de mission
- **Gérer** les points assignés aux missions
- **Suivre** l'état d'avancement

### 📊 Progrès des Missions
**Accès** : Menu → Missions → Progrès

**Fonctionnalités** :
- **Voir** l'avancement des missions
- **Filtrer** par statut, brigade, période
- **Exporter** les données de progrès

### 📅 Événements
**Accès** : Menu → Missions → Événements

**Fonctionnalités** :
- **Consulter** les événements de collecte
- **Créer** de nouveaux événements
- **Gérer** les événements programmés

---

## 🚛 Véhicules

### 🚗 Gestion des Véhicules
**Accès** : Menu → Véhicules

**Fonctionnalités** :
- **Consulter** la liste des véhicules
- **Ajouter un véhicule** avec "Nouveau véhicule"
- **Modifier** les informations d'un véhicule
- **Suivre** l'état des véhicules
- **Assigner** des véhicules aux missions

**Ajout d'un véhicule** :
1. Cliquez sur "Nouveau véhicule"
2. Remplissez le formulaire :
   - **Immatriculation** : Numéro de plaque
   - **Type** : Camion, Van, Voiture, etc.
   - **Marque et modèle**
   - **Capacité** : Capacité de charge
   - **État** : Opérationnel, En maintenance, Hors service
3. Cliquez sur "Créer"

---

## 📊 Rapports

### 📈 Génération de Rapports
**Accès** : Menu → Rapports

**Fonctionnalités** :
- **Consulter** les rapports existants
- **Créer un rapport** avec "Nouveau rapport"
- **Filtrer** par type, période, organisation
- **Télécharger** les rapports en PDF/Excel

**Types de rapports disponibles** :
- **Collecte** : Rapports de collecte des déchets
- **Maintenance** : Rapports de maintenance
- **Performance** : Rapports de performance
- **Financier** : Rapports financiers
- **Environnemental** : Rapports environnementaux
- **Personnalisé** : Rapports sur mesure

**Création d'un rapport** :
1. Cliquez sur "Nouveau rapport"
2. Sélectionnez le type de rapport
3. Choisissez la période
4. Sélectionnez les critères
5. Cliquez sur "Générer"

---

## 🔔 Notifications

### 📢 Gestion des Notifications
**Accès** : Menu → Notifications

**Fonctionnalités** :
- **Consulter** toutes les notifications
- **Marquer** comme lues/non lues
- **Filtrer** par type, priorité, date
- **Configurer** les préférences

**Types de notifications** :
- **Missions** : Nouvelles missions, rappels
- **Véhicules** : Maintenance, pannes
- **Points de collecte** : Visites, problèmes
- **Signalements** : Nouveaux signalements
- **Système** : Alertes système

---

## 🗺️ Carte Interactive

### 🌍 Vue Cartographique
**Accès** : Menu → Carte

**Fonctionnalités** :
- **Voir** tous les points de collecte sur la carte
- **Filtrer** par type, statut, commune
- **Zoomer** et naviguer sur la carte
- **Cliquer** sur un point pour voir les détails
- **Afficher** les missions en cours

**Utilisation** :
- **Zoom** : Molette de la souris ou boutons +/-
- **Navigation** : Clic-glisser pour déplacer
- **Détails** : Clic sur un point pour plus d'informations
- **Filtres** : Utilisez les filtres à gauche

---

## ⚙️ Paramètres et Profil

### 👤 Mon Profil
**Accès** : Menu → Profil (icône utilisateur en haut à droite)

**Fonctionnalités** :
- **Consulter** vos informations personnelles
- **Modifier** votre mot de passe
- **Changer** vos préférences
- **Gérer** vos notifications

### 🔧 Paramètres
**Accès** : Menu → Paramètres (si disponible)

**Fonctionnalités** :
- **Configurer** les paramètres de l'application
- **Gérer** les préférences d'affichage
- **Configurer** les notifications

---

## 🔍 Recherche et Filtres

### 🔎 Recherche Globale
- **Barre de recherche** : En haut de chaque page
- **Recherche par mots-clés** : Nom, description, adresse
- **Filtres avancés** : Par date, type, statut, etc.

### 🎛️ Filtres Communs
- **Par date** : Période spécifique
- **Par statut** : Actif, Inactif, En cours, etc.
- **Par organisation** : Filtrer par organisation
- **Par commune** : Filtrer par zone géographique

---

## 📱 Utilisation Mobile

### 📲 Interface Responsive
L'application s'adapte automatiquement aux écrans mobiles :
- **Navigation** : Menu hamburger sur mobile
- **Formulaires** : Optimisés pour le tactile
- **Cartes** : Navigation tactile intuitive
- **Photos** : Prise de photo directe depuis l'appareil

### 📸 Prise de Photos
1. **Sélectionnez** "Ajouter une photo"
2. **Choisissez** "Appareil photo" ou "Galerie"
3. **Prenez** ou sélectionnez la photo
4. **Ajoutez** une description
5. **Sauvegardez**

---

## ❓ Aide et Support

### 🆘 En cas de problème
1. **Vérifiez** votre connexion internet
2. **Actualisez** la page (F5)
3. **Videz** le cache du navigateur
4. **Contactez** l'administrateur système

### 📞 Contacts
- **Support technique** : [email du support]
- **Administrateur** : [email de l'admin]
- **Documentation** : [lien vers la documentation]

### 🔧 Résolution de problèmes courants

**Problème** : "Erreur de connexion"
- **Solution** : Vérifiez votre connexion internet et réessayez

**Problème** : "Page ne se charge pas"
- **Solution** : Actualisez la page (F5) ou videz le cache

**Problème** : "Formulaire ne se soumet pas"
- **Solution** : Vérifiez que tous les champs obligatoires sont remplis

**Problème** : "Photo ne s'upload pas"
- **Solution** : Vérifiez la taille (max 5MB) et le format (JPG, PNG, GIF)

---

## 🎯 Bonnes Pratiques

### ✅ Conseils d'utilisation
1. **Sauvegardez** régulièrement votre travail
2. **Vérifiez** les informations avant de soumettre
3. **Utilisez** des descriptions claires et précises
4. **Prenez** des photos de bonne qualité
5. **Respectez** les procédures de votre organisation

### 🔒 Sécurité
1. **Déconnectez-vous** après utilisation
2. **Ne partagez pas** vos identifiants
3. **Utilisez** des mots de passe forts
4. **Signalez** tout comportement suspect

---

## 📚 Glossaire

- **Brigade** : Équipe de travailleurs assignée à des missions
- **Point de collecte** : Lieu où les déchets sont collectés
- **Mission** : Tâche assignée à une brigade
- **Signalement** : Rapport d'un problème (décharge illégale, etc.)
- **Visite** : Contrôle effectué sur un point de collecte
- **Organisation** : Entité responsable (mairie, entreprise, etc.)

---

*Guide d'utilisation v1.0 - Géoportail d'Assainissement*
