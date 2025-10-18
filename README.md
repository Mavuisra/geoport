# 🚀 Documentation API GeoPortail

## 📋 Vue d'ensemble

Cette API Django REST Framework fournit toutes les fonctionnalités pour une application de géolocalisation et de suivi de véhicules en temps réel.

**Base URL:** `http://127.0.0.1:8000/api/`

## 🔐 Authentification

### Obtenir un token d'authentification

```http
POST /api/auth/token/
Content-Type: application/json

{
    "username": "admin",
    "password": "admin123"
}
```

**Réponse:**
```json
{
    "token": "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0"
}
```

### Utiliser le token

Ajoutez l'en-tête suivant à toutes vos requêtes:
```
Authorization: Token a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0
```

## 📊 Endpoints disponibles

### 1. Utilisateurs
```http
GET    /api/users/          # Liste des utilisateurs
POST   /api/users/          # Créer un utilisateur
GET    /api/users/{id}/     # Détails d'un utilisateur
PUT    /api/users/{id}/     # Modifier un utilisateur
DELETE /api/users/{id}/     # Supprimer un utilisateur
```

### 2. Véhicules
```http
GET    /api/vehicles/       # Liste des véhicules
POST   /api/vehicles/       # Créer un véhicule
GET    /api/vehicles/{id}/  # Détails d'un véhicule
PUT    /api/vehicles/{id}/  # Modifier un véhicule
DELETE /api/vehicles/{id}/  # Supprimer un véhicule
```

### 3. Missions
```http
GET    /api/missions/       # Liste des missions
POST   /api/missions/       # Créer une mission
GET    /api/missions/{id}/  # Détails d'une mission
PUT    /api/missions/{id}/  # Modifier une mission
DELETE /api/missions/{id}/  # Supprimer une mission
```

### 4. Anomalies
```http
GET    /api/anomalies/      # Liste des anomalies
POST   /api/anomalies/      # Créer une anomalie
GET    /api/anomalies/{id}/ # Détails d'une anomalie
PUT    /api/anomalies/{id}/ # Modifier une anomalie
DELETE /api/anomalies/{id}/ # Supprimer une anomalie
```

## 🚗 Endpoints spécialisés

### Assignation de véhicules
```http
POST /api/vehicle/assign/
Content-Type: application/json
Authorization: Token YOUR_TOKEN

{
    "vehicle_id": 1,
    "user_id": 2
}
```

### Désassignation de véhicules
```http
POST /api/vehicle/unassign/
Content-Type: application/json
Authorization: Token YOUR_TOKEN

{
    "user_id": 2
}
```

### Mise à jour de position
```http
POST /api/vehicle/position/
Content-Type: application/json
Authorization: Token YOUR_TOKEN

{
    "vehicle_id": 1,
    "latitude": -4.4419,
    "longitude": 15.2663,
    "speed": 45.5,
    "heading": 180
}
```

### Signalement d'anomalie
```http
POST /api/anomaly/report/
Content-Type: application/json
Authorization: Token YOUR_TOKEN

{
    "type": "POTHOLE",
    "description": "Nid de poule sur la route",
    "latitude": -4.4419,
    "longitude": 15.2663,
    "severity": "HIGH"
}
```

### Statistiques
```http
GET /api/stats/
Authorization: Token YOUR_TOKEN
```

## 🔌 WebSocket - Suivi temps réel

### Connexion WebSocket
```
ws://127.0.0.1:8000/ws/vehicle-tracking/
```

### Messages WebSocket

#### Connexion établie
```json
{
    "type": "connection_established",
    "message": "Connexion WebSocket établie avec succès",
    "timestamp": "2025-10-16T16:00:00Z"
}
```

#### Mise à jour de position
```json
{
    "type": "position_update",
    "vehicle_id": 1,
    "position": {
        "lat": -4.4419,
        "lng": 15.2663,
        "speed": 45.5,
        "heading": 180,
        "status": "MOVING",
        "timestamp": "2025-10-16T16:00:00Z"
    }
}
```

#### Mise à jour de statut
```json
{
    "type": "status_update",
    "vehicle_id": 1,
    "status": "ONLINE",
    "is_tracking": true
}
```

## 📝 Modèles de données

### Utilisateur
```json
{
    "id": 1,
    "username": "john_doe",
    "email": "john@example.com",
    "role": "AGENT",
    "phone": "+243123456789",
    "organization": "GeoPortail RDC",
    "is_active": true,
    "created_at": "2025-10-16T16:00:00Z"
}
```

### Véhicule
```json
{
    "id": 1,
    "identifier": "VH-001",
    "vehicle_type": "PICKUP",
    "company": "GeoPortail RDC",
    "driver": "john_doe",
    "active": true,
    "created_at": "2025-10-16T16:00:00Z"
}
```

### Mission
```json
{
    "id": 1,
    "title": "Collecte des déchets",
    "description": "Collecte dans le quartier Matonge",
    "status": "PENDING",
    "priority": "HIGH",
    "assigned_to": "john_doe",
    "vehicle": 1,
    "zone": 1,
    "scheduled_at": "2025-10-16T16:00:00Z",
    "created_at": "2025-10-16T16:00:00Z"
}
```

### Anomalie
```json
{
    "id": 1,
    "type": "POTHOLE",
    "description": "Nid de poule sur la route",
    "latitude": -4.4419,
    "longitude": 15.2663,
    "severity": "HIGH",
    "status": "REPORTED",
    "reported_by": "john_doe",
    "created_at": "2025-10-16T16:00:00Z"
}
```

## 🚨 Gestion des erreurs

### Codes d'erreur courants

#### 400 - Bad Request
```json
{
    "error": "Données JSON invalides"
}
```

#### 401 - Non authentifié
```json
{
    "detail": "Authentication credentials were not provided."
}
```

#### 403 - Accès refusé
```json
{
    "detail": "You do not have permission to perform this action."
}
```

#### 404 - Non trouvé
```json
{
    "error": "Véhicule non trouvé"
}
```

#### 500 - Erreur serveur
```json
{
    "error": "Erreur serveur: Message d'erreur détaillé"
}
```

## 🔧 Utilisateurs de test

```
Username: admin
Password: admin123
Role: ADMIN

Username: supervisor
Password: supervisor123
Role: SUPERVISOR

Username: agent
Password: agent123
Role: AGENT
```

## 📱 Exemples d'utilisation

### JavaScript/Angular
```javascript
// Authentification
const login = async (username, password) => {
  const response = await fetch('http://127.0.0.1:8000/api/auth/token/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ username, password })
  });
  return await response.json();
};

// Récupérer les véhicules
const getVehicles = async (token) => {
  const response = await fetch('http://127.0.0.1:8000/api/vehicles/', {
    headers: { 'Authorization': `Token ${token}` }
  });
  return await response.json();
};
```

### Python/Requests
```python
import requests

# Authentification
response = requests.post('http://127.0.0.1:8000/api/auth/token/', 
                        json={'username': 'admin', 'password': 'admin123'})
token = response.json()['token']

# Récupérer les véhicules
headers = {'Authorization': f'Token {token}'}
vehicles = requests.get('http://127.0.0.1:8000/api/vehicles/', headers=headers)
```

### cURL
```bash
# Authentification
curl -X POST http://127.0.0.1:8000/api/auth/token/ \
  -H "Content-Type: application/json" \
  -d '{"username": "admin", "password": "admin123"}'

# Récupérer les véhicules
curl -X GET http://127.0.0.1:8000/api/vehicles/ \
  -H "Authorization: Token YOUR_TOKEN"
```

## 🎯 Prochaines étapes

1. **Tester l'authentification** avec les utilisateurs de test
2. **Explorer les endpoints** avec un client REST (Postman, Insomnia)
3. **Implémenter l'authentification** dans votre application
4. **Développer les services** pour chaque endpoint
5. **Intégrer le WebSocket** pour le temps réel
6. **Tester avec des données réelles**

## 📞 Support

Pour toute question ou problème, contactez l'équipe de développement.

**Bonne intégration !** 🚀
