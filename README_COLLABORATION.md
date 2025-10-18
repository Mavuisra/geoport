# 🤝 Guide de collaboration - GeoPortail

## 📋 Pour le développeur Angular

### 🎯 Ce que vous devez savoir

Vous allez développer l'interface utilisateur (frontend) d'une application de géolocalisation et de suivi de véhicules. L'API backend Django est déjà prête et fonctionnelle.

### 📁 Fichiers de documentation

1. **`API_DOCUMENTATION.md`** - Documentation complète de l'API
2. **`ANGULAR_EXAMPLES.md`** - Exemples de code Angular prêts à utiliser
3. **`QUICK_START_GUIDE.md`** - Guide de démarrage rapide avec tests

### 🚀 Démarrage rapide

#### 1. Tester l'API
```bash
# Vérifier que l'API fonctionne
curl http://127.0.0.1:8000/api/

# Tester l'authentification
curl -X POST http://127.0.0.1:8000/api/auth/token/ \
  -H "Content-Type: application/json" \
  -d '{"username": "admin", "password": "admin123"}'
```

#### 2. Créer votre projet Angular
```bash
ng new geoportail-angular
cd geoportail-angular
npm install @angular/common @angular/core @angular/http
npm install rxjs websocket leaflet @types/leaflet
```

#### 3. Copier les services depuis `ANGULAR_EXAMPLES.md`

### 🔐 Authentification

**Utilisateurs de test disponibles :**
- `admin` / `admin123` (ADMIN)
- `supervisor` / `supervisor123` (SUPERVISOR)  
- `agent` / `agent123` (AGENT)

### 📊 Endpoints principaux

```
GET    /api/vehicles/          # Liste des véhicules
POST   /api/vehicle/assign/    # Assigner un véhicule
POST   /api/vehicle/position/  # Mettre à jour la position
GET    /api/missions/          # Liste des missions
POST   /api/anomaly/report/   # Signaler une anomalie
```

### 🔌 WebSocket temps réel

```
ws://127.0.0.1:8000/ws/vehicle-tracking/
```

### 🎨 Design et couleurs

**Couleur primaire :** `#46c28a` (vert)
**Couleur secondaire :** `#3aa876` (vert foncé)

### 📱 Fonctionnalités à développer

1. **Page de connexion** avec authentification
2. **Dashboard** avec statistiques
3. **Suivi des véhicules** en temps réel
4. **Carte interactive** avec Leaflet
5. **Assignation de véhicules**
6. **Signalement d'anomalies**
7. **Gestion des missions**

### 🛠️ Technologies recommandées

- **Angular** (version 15+)
- **RxJS** pour la gestion des observables
- **Leaflet** pour les cartes
- **Bootstrap** pour le design
- **WebSocket** pour le temps réel

### 📞 Support

- **Documentation complète** dans les fichiers .md
- **Exemples de code** prêts à utiliser
- **Tests** pour valider l'intégration
- **Support** de l'équipe backend

### 🎯 Objectifs

1. **Interface utilisateur** moderne et responsive
2. **Intégration API** complète
3. **Temps réel** avec WebSocket
4. **Expérience utilisateur** optimale
5. **Performance** et stabilité

### 🚀 Prochaines étapes

1. **Lire** la documentation complète
2. **Tester** l'API avec les exemples fournis
3. **Créer** le projet Angular
4. **Implémenter** l'authentification
5. **Développer** les composants principaux
6. **Intégrer** la carte et le temps réel
7. **Tester** et optimiser

### 💡 Conseils

- **Commencez** par l'authentification
- **Testez** chaque endpoint individuellement
- **Utilisez** les exemples de code fournis
- **Implémentez** le WebSocket pour le temps réel
- **Design** responsive et moderne
- **Gérez** les erreurs et les états de chargement

### 🎉 Résultat attendu

Une application Angular complète qui permet :
- De se connecter et s'authentifier
- De voir les véhicules sur une carte
- De suivre les mouvements en temps réel
- D'assigner des véhicules aux utilisateurs
- De signaler des anomalies
- De gérer les missions

**Bon développement !** 🚀

---

## 📋 Pour l'équipe backend

### 🎯 Ce qui est prêt

- ✅ **API REST** complète avec Django REST Framework
- ✅ **Authentification** par tokens
- ✅ **WebSocket** pour le temps réel
- ✅ **CORS** configuré
- ✅ **Documentation** complète
- ✅ **Exemples de code** Angular
- ✅ **Tests** de validation

### 🔧 Maintenance

- **Serveur** à maintenir sur le port 8000
- **Base de données** à surveiller
- **Logs** à vérifier régulièrement
- **Sécurité** à maintenir
- **Performance** à optimiser

### 📞 Support développeur Angular

- **Documentation** à jour
- **Exemples** fonctionnels
- **Tests** de validation
- **Support** technique
- **Évolution** de l'API

**L'équipe backend est prête à supporter le développement frontend !** 🎉
