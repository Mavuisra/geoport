# 🅰️ Exemples Angular pour GeoPortail

## 📋 Configuration initiale

### 1. Installation des dépendances
```bash
npm install @angular/common @angular/core @angular/http
npm install rxjs websocket
npm install leaflet @types/leaflet
```

### 2. Configuration du module principal
```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule } from '@angular/common/http';
import { FormsModule } from '@angular/forms';

import { AppComponent } from './app.component';
import { AuthService } from './services/auth.service';
import { VehicleService } from './services/vehicle.service';
import { WebSocketService } from './services/websocket.service';

@NgModule({
  declarations: [
    AppComponent,
    // ... autres composants
  ],
  imports: [
    BrowserModule,
    HttpClientModule,
    FormsModule,
    // ... autres modules
  ],
  providers: [
    AuthService,
    VehicleService,
    WebSocketService
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## 🔐 Service d'authentification

```typescript
// services/auth.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { Observable, BehaviorSubject } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  private apiUrl = 'http://127.0.0.1:8000/api';
  private tokenSubject = new BehaviorSubject<string | null>(null);
  public token$ = this.tokenSubject.asObservable();

  constructor(private http: HttpClient) {
    // Récupérer le token stocké au démarrage
    const token = localStorage.getItem('authToken');
    if (token) {
      this.tokenSubject.next(token);
    }
  }

  login(username: string, password: string): Observable<any> {
    return this.http.post(`${this.apiUrl}/auth/token/`, {
      username,
      password
    }).pipe(
      tap(response => {
        const token = response.token;
        localStorage.setItem('authToken', token);
        this.tokenSubject.next(token);
      })
    );
  }

  logout(): void {
    localStorage.removeItem('authToken');
    this.tokenSubject.next(null);
  }

  getAuthHeaders(): HttpHeaders {
    const token = this.tokenSubject.value;
    return new HttpHeaders({
      'Authorization': `Token ${token}`,
      'Content-Type': 'application/json'
    });
  }

  isAuthenticated(): boolean {
    return this.tokenSubject.value !== null;
  }

  getCurrentToken(): string | null {
    return this.tokenSubject.value;
  }
}
```

## 🚗 Service pour les véhicules

```typescript
// services/vehicle.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpParams } from '@angular/common/http';
import { Observable } from 'rxjs';
import { AuthService } from './auth.service';

@Injectable({
  providedIn: 'root'
})
export class VehicleService {
  private apiUrl = 'http://127.0.0.1:8000/api';

  constructor(
    private http: HttpClient,
    private authService: AuthService
  ) {}

  // Récupérer tous les véhicules
  getVehicles(page: number = 1, pageSize: number = 20): Observable<any> {
    const params = new HttpParams()
      .set('page', page.toString())
      .set('page_size', pageSize.toString());

    return this.http.get(`${this.apiUrl}/vehicles/`, {
      headers: this.authService.getAuthHeaders(),
      params: params
    });
  }

  // Récupérer un véhicule par ID
  getVehicle(id: number): Observable<any> {
    return this.http.get(`${this.apiUrl}/vehicles/${id}/`, {
      headers: this.authService.getAuthHeaders()
    });
  }

  // Assigner un véhicule
  assignVehicle(vehicleId: number, userId: number): Observable<any> {
    return this.http.post(`${this.apiUrl}/vehicle/assign/`, {
      vehicle_id: vehicleId,
      user_id: userId
    }, {
      headers: this.authService.getAuthHeaders()
    });
  }

  // Désassigner un véhicule
  unassignVehicle(userId: number): Observable<any> {
    return this.http.post(`${this.apiUrl}/vehicle/unassign/`, {
      user_id: userId
    }, {
      headers: this.authService.getAuthHeaders()
    });
  }

  // Mettre à jour la position
  updatePosition(vehicleId: number, lat: number, lng: number, speed: number, heading: number = 0): Observable<any> {
    return this.http.post(`${this.apiUrl}/vehicle/position/`, {
      vehicle_id: vehicleId,
      latitude: lat,
      longitude: lng,
      speed: speed,
      heading: heading
    }, {
      headers: this.authService.getAuthHeaders()
    });
  }
}
```

## 🔌 Service WebSocket

```typescript
// services/websocket.service.ts
import { Injectable } from '@angular/core';
import { Observable, Subject, BehaviorSubject } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class WebSocketService {
  private socket: WebSocket | null = null;
  private messageSubject = new Subject<any>();
  private connectionSubject = new BehaviorSubject<boolean>(false);
  
  public messages$ = this.messageSubject.asObservable();
  public isConnected$ = this.connectionSubject.asObservable();

  connect(): void {
    if (this.socket?.readyState === WebSocket.OPEN) {
      return; // Déjà connecté
    }

    this.socket = new WebSocket('ws://127.0.0.1:8000/ws/vehicle-tracking/');
    
    this.socket.onopen = () => {
      console.log('WebSocket connecté');
      this.connectionSubject.next(true);
    };
    
    this.socket.onmessage = (event) => {
      try {
        const data = JSON.parse(event.data);
        this.messageSubject.next(data);
      } catch (error) {
        console.error('Erreur parsing WebSocket message:', error);
      }
    };
    
    this.socket.onclose = (event) => {
      console.log('WebSocket fermé:', event.code, event.reason);
      this.connectionSubject.next(false);
      
      // Tentative de reconnexion après 5 secondes
      setTimeout(() => {
        if (this.connectionSubject.value === false) {
          this.connect();
        }
      }, 5000);
    };
    
    this.socket.onerror = (error) => {
      console.error('Erreur WebSocket:', error);
      this.connectionSubject.next(false);
    };
  }

  disconnect(): void {
    if (this.socket) {
      this.socket.close();
      this.socket = null;
      this.connectionSubject.next(false);
    }
  }

  sendMessage(message: any): void {
    if (this.socket?.readyState === WebSocket.OPEN) {
      this.socket.send(JSON.stringify(message));
    } else {
      console.warn('WebSocket non connecté, impossible d\'envoyer le message');
    }
  }

  // Méthodes spécialisées
  startTracking(vehicleId: number): void {
    this.sendMessage({
      type: 'start_tracking',
      vehicle_id: vehicleId
    });
  }

  stopTracking(vehicleId: number): void {
    this.sendMessage({
      type: 'stop_tracking',
      vehicle_id: vehicleId
    });
  }

  updateLocation(lat: number, lng: number, speed: number, heading: number): void {
    this.sendMessage({
      type: 'location_update',
      latitude: lat,
      longitude: lng,
      speed: speed,
      heading: heading
    });
  }
}
```

## 🗺️ Service pour la carte

```typescript
// services/map.service.ts
import { Injectable } from '@angular/core';
import * as L from 'leaflet';

@Injectable({
  providedIn: 'root'
})
export class MapService {
  private map: L.Map | null = null;
  private vehicleMarkers: Map<number, L.Marker> = new Map();

  initMap(containerId: string, center: [number, number] = [-4.4419, 15.2663], zoom: number = 13): L.Map {
    this.map = L.map(containerId).setView(center, zoom);
    
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© OpenStreetMap contributors',
      maxZoom: 19
    }).addTo(this.map);

    return this.map;
  }

  addVehicleMarker(vehicle: any): L.Marker {
    if (!this.map) return null as any;

    const icon = this.createVehicleIcon(vehicle.vehicle_type, vehicle.status);
    
    const marker = L.marker([vehicle.latitude, vehicle.longitude], { icon })
      .bindPopup(this.createVehiclePopup(vehicle))
      .addTo(this.map);

    this.vehicleMarkers.set(vehicle.id, marker);
    return marker;
  }

  updateVehiclePosition(vehicleId: number, lat: number, lng: number): void {
    const marker = this.vehicleMarkers.get(vehicleId);
    if (marker) {
      marker.setLatLng([lat, lng]);
    }
  }

  removeVehicleMarker(vehicleId: number): void {
    const marker = this.vehicleMarkers.get(vehicleId);
    if (marker && this.map) {
      this.map.removeLayer(marker);
      this.vehicleMarkers.delete(vehicleId);
    }
  }

  private createVehicleIcon(type: string, status: string): L.Icon {
    const color = this.getStatusColor(status);
    const iconUrl = this.getVehicleIconUrl(type);
    
    return L.icon({
      iconUrl: iconUrl,
      iconSize: [32, 32],
      iconAnchor: [16, 16],
      popupAnchor: [0, -16]
    });
  }

  private getStatusColor(status: string): string {
    const colors = {
      'ONLINE': '#28a745',
      'OFFLINE': '#6c757d',
      'MOVING': '#007bff',
      'STOPPED': '#ffc107',
      'MAINTENANCE': '#dc3545'
    };
    return colors[status as keyof typeof colors] || '#6c757d';
  }

  private getVehicleIconUrl(type: string): string {
    const icons = {
      'PICKUP': 'assets/icons/pickup.png',
      'VAN': 'assets/icons/van.png',
      'MOTORCYCLE': 'assets/icons/motorcycle.png'
    };
    return icons[type as keyof typeof icons] || 'assets/icons/vehicle.png';
  }

  private createVehiclePopup(vehicle: any): string {
    return `
      <div class="vehicle-popup">
        <h6>${vehicle.identifier}</h6>
        <p><strong>Type:</strong> ${vehicle.vehicle_type}</p>
        <p><strong>Conducteur:</strong> ${vehicle.driver || 'Non assigné'}</p>
        <p><strong>Statut:</strong> <span class="badge badge-${this.getStatusColor(vehicle.status)}">${vehicle.status}</span></p>
        <p><strong>Vitesse:</strong> ${vehicle.speed || 0} km/h</p>
      </div>
    `;
  }
}
```

## 📱 Composant de connexion

```typescript
// components/login/login.component.ts
import { Component } from '@angular/core';
import { Router } from '@angular/router';
import { AuthService } from '../../services/auth.service';

@Component({
  selector: 'app-login',
  template: `
    <div class="login-container">
      <div class="login-card">
        <h2 class="text-center mb-4">
          <i class="fas fa-map-marker-alt me-2"></i>GeoPortail
        </h2>
        
        <form (ngSubmit)="onLogin()" #loginForm="ngForm">
          <div class="mb-3">
            <label for="username" class="form-label">Nom d'utilisateur</label>
            <input 
              type="text" 
              class="form-control" 
              id="username"
              [(ngModel)]="username" 
              name="username"
              required
              #usernameInput="ngModel">
            <div *ngIf="usernameInput.invalid && usernameInput.touched" class="text-danger">
              Le nom d'utilisateur est requis
            </div>
          </div>
          
          <div class="mb-3">
            <label for="password" class="form-label">Mot de passe</label>
            <input 
              type="password" 
              class="form-control" 
              id="password"
              [(ngModel)]="password" 
              name="password"
              required
              #passwordInput="ngModel">
            <div *ngIf="passwordInput.invalid && passwordInput.touched" class="text-danger">
              Le mot de passe est requis
            </div>
          </div>
          
          <div *ngIf="errorMessage" class="alert alert-danger">
            {{ errorMessage }}
          </div>
          
          <button 
            type="submit" 
            class="btn btn-primary w-100"
            [disabled]="loginForm.invalid || isLoading">
            <span *ngIf="isLoading" class="spinner-border spinner-border-sm me-2"></span>
            {{ isLoading ? 'Connexion...' : 'Se connecter' }}
          </button>
        </form>
      </div>
    </div>
  `,
  styles: [`
    .login-container {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      background: linear-gradient(135deg, #46c28a 0%, #3aa876 100%);
    }
    .login-card {
      background: white;
      padding: 2rem;
      border-radius: 12px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.1);
      width: 100%;
      max-width: 400px;
    }
  `]
})
export class LoginComponent {
  username: string = '';
  password: string = '';
  isLoading: boolean = false;
  errorMessage: string = '';

  constructor(
    private authService: AuthService,
    private router: Router
  ) {}

  onLogin(): void {
    this.isLoading = true;
    this.errorMessage = '';

    this.authService.login(this.username, this.password).subscribe({
      next: (response) => {
        this.isLoading = false;
        this.router.navigate(['/dashboard']);
      },
      error: (error) => {
        this.isLoading = false;
        this.errorMessage = 'Nom d\'utilisateur ou mot de passe incorrect';
        console.error('Erreur de connexion:', error);
      }
    });
  }
}
```

## 🚗 Composant de suivi des véhicules

```typescript
// components/vehicle-tracking/vehicle-tracking.component.ts
import { Component, OnInit, OnDestroy } from '@angular/core';
import { VehicleService } from '../../services/vehicle.service';
import { WebSocketService } from '../../services/websocket.service';
import { MapService } from '../../services/map.service';
import { Subscription } from 'rxjs';

@Component({
  selector: 'app-vehicle-tracking',
  template: `
    <div class="tracking-container">
      <div class="tracking-header">
        <h2><i class="fas fa-satellite-dish me-2"></i>Suivi des véhicules</h2>
        <div class="connection-status">
          <span class="badge" [class]="isConnected ? 'bg-success' : 'bg-danger'">
            <i class="fas fa-wifi me-1"></i>
            {{ isConnected ? 'Connecté' : 'Déconnecté' }}
          </span>
        </div>
      </div>

      <div class="row">
        <!-- Liste des véhicules -->
        <div class="col-md-4">
          <div class="card">
            <div class="card-header">
              <h5><i class="fas fa-truck me-2"></i>Véhicules disponibles</h5>
            </div>
            <div class="card-body">
              <div *ngIf="isLoading" class="text-center">
                <div class="spinner-border" role="status"></div>
                <p class="mt-2">Chargement...</p>
              </div>
              
              <div *ngFor="let vehicle of vehicles" class="vehicle-item mb-3">
                <div class="d-flex justify-content-between align-items-center">
                  <div>
                    <h6 class="mb-1">{{ vehicle.identifier }}</h6>
                    <small class="text-muted">{{ vehicle.get_vehicle_type_display }}</small>
                    <br>
                    <small class="text-muted">Conducteur: {{ vehicle.driver || 'Non assigné' }}</small>
                  </div>
                  <div>
                    <button 
                      class="btn btn-sm btn-primary"
                      (click)="assignVehicle(vehicle.id)"
                      [disabled]="vehicle.driver">
                      <i class="fas fa-check me-1"></i>Assigner
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Carte -->
        <div class="col-md-8">
          <div class="card">
            <div class="card-header">
              <h5><i class="fas fa-map me-2"></i>Carte de suivi</h5>
            </div>
            <div class="card-body p-0">
              <div id="trackingMap" class="map-container"></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .tracking-container {
      padding: 1rem;
    }
    .tracking-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 1rem;
    }
    .map-container {
      height: 500px;
      width: 100%;
    }
    .vehicle-item {
      border: 1px solid #dee2e6;
      border-radius: 8px;
      padding: 1rem;
      background: #f8f9fa;
    }
  `]
})
export class VehicleTrackingComponent implements OnInit, OnDestroy {
  vehicles: any[] = [];
  isLoading: boolean = false;
  isConnected: boolean = false;
  private subscriptions: Subscription[] = [];

  constructor(
    private vehicleService: VehicleService,
    private webSocketService: WebSocketService,
    private mapService: MapService
  ) {}

  ngOnInit(): void {
    this.loadVehicles();
    this.initWebSocket();
    this.initMap();
  }

  ngOnDestroy(): void {
    this.subscriptions.forEach(sub => sub.unsubscribe());
    this.webSocketService.disconnect();
  }

  loadVehicles(): void {
    this.isLoading = true;
    this.vehicleService.getVehicles().subscribe({
      next: (response) => {
        this.vehicles = response.results;
        this.isLoading = false;
      },
      error: (error) => {
        console.error('Erreur lors du chargement des véhicules:', error);
        this.isLoading = false;
      }
    });
  }

  assignVehicle(vehicleId: number): void {
    const userId = 1; // ID de l'utilisateur connecté
    this.vehicleService.assignVehicle(vehicleId, userId).subscribe({
      next: (response) => {
        console.log('Véhicule assigné:', response);
        this.loadVehicles(); // Recharger la liste
      },
      error: (error) => {
        console.error('Erreur lors de l\'assignation:', error);
      }
    });
  }

  private initWebSocket(): void {
    this.webSocketService.connect();
    
    // Écouter l'état de connexion
    const connectionSub = this.webSocketService.isConnected$.subscribe(
      connected => this.isConnected = connected
    );
    this.subscriptions.push(connectionSub);

    // Écouter les messages WebSocket
    const messageSub = this.webSocketService.messages$.subscribe(message => {
      this.handleWebSocketMessage(message);
    });
    this.subscriptions.push(messageSub);
  }

  private initMap(): void {
    setTimeout(() => {
      this.mapService.initMap('trackingMap');
    }, 100);
  }

  private handleWebSocketMessage(message: any): void {
    switch (message.type) {
      case 'position_update':
        this.updateVehiclePositionOnMap(message);
        break;
      case 'status_update':
        this.updateVehicleStatusOnMap(message);
        break;
      case 'initial_data':
        this.loadInitialVehicles(message.vehicles);
        break;
    }
  }

  private updateVehiclePositionOnMap(message: any): void {
    const { vehicle_id, position } = message;
    this.mapService.updateVehiclePosition(vehicle_id, position.lat, position.lng);
  }

  private updateVehicleStatusOnMap(message: any): void {
    // Mettre à jour le statut du véhicule sur la carte
    console.log('Statut mis à jour:', message);
  }

  private loadInitialVehicles(vehicles: any[]): void {
    vehicles.forEach(vehicle => {
      if (vehicle.position) {
        this.mapService.addVehicleMarker(vehicle);
      }
    });
  }
}
```

## 🎯 Configuration des routes

```typescript
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { LoginComponent } from './components/login/login.component';
import { VehicleTrackingComponent } from './components/vehicle-tracking/vehicle-tracking.component';
import { DashboardComponent } from './components/dashboard/dashboard.component';
import { AuthGuard } from './guards/auth.guard';

const routes: Routes = [
  { path: '', redirectTo: '/login', pathMatch: 'full' },
  { path: 'login', component: LoginComponent },
  { path: 'dashboard', component: DashboardComponent, canActivate: [AuthGuard] },
  { path: 'vehicle-tracking', component: VehicleTrackingComponent, canActivate: [AuthGuard] },
  { path: '**', redirectTo: '/login' }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

## 🔒 Guard d'authentification

```typescript
// guards/auth.guard.ts
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { AuthService } from '../services/auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  
  constructor(
    private authService: AuthService,
    private router: Router
  ) {}

  canActivate(): boolean {
    if (this.authService.isAuthenticated()) {
      return true;
    } else {
      this.router.navigate(['/login']);
      return false;
    }
  }
}
```

## 🎨 Styles CSS globaux

```css
/* styles.css */
:root {
  --primary-color: #46c28a;
  --primary-dark: #3aa876;
  --secondary-color: #6c757d;
  --success-color: #28a745;
  --danger-color: #dc3545;
  --warning-color: #ffc107;
  --info-color: #17a2b8;
}

.btn-primary {
  background-color: var(--primary-color);
  border-color: var(--primary-color);
}

.btn-primary:hover {
  background-color: var(--primary-dark);
  border-color: var(--primary-dark);
}

.card {
  border: none;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.card-header {
  background: linear-gradient(135deg, var(--primary-color) 0%, var(--primary-dark) 100%);
  color: white;
  border-radius: 12px 12px 0 0 !important;
}

.badge {
  border-radius: 20px;
  padding: 0.5rem 1rem;
}

.map-container {
  border-radius: 8px;
  overflow: hidden;
}

.vehicle-item {
  transition: all 0.3s ease;
}

.vehicle-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(0,0,0,0.1);
}
```

## 🚀 Démarrage rapide

1. **Installer les dépendances** : `npm install`
2. **Configurer l'API URL** dans les services
3. **Tester l'authentification** avec les utilisateurs de test
4. **Développer les composants** selon vos besoins
5. **Intégrer la carte** avec Leaflet
6. **Tester le WebSocket** pour le temps réel

**voici tout ce qu'il faut pour développer l'application coté Angular !** 🎉
