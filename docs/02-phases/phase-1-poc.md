# 🚀 Phase 1 : POC (Proof of Concept) - 2-4 Semaines

## Vue d'ensemble

La Phase 1 vise à valider la faisabilité technique avec un sous-ensemble minimal de fonctionnalités, en utilisant exclusivement des outils open-source et des ressources existantes.

## 📊 Résumé de Phase

| Critère | Détail |
|---------|--------|
| **Durée** | 2-4 semaines (2 sprints) |
| **Objectif Principal** | Valider la faisabilité et établir les fondations |
| **Périmètre Fonctionnel** | 1 microservice + frontend basique + CI/CD minimal |
| **Stack Technique** | Docker Compose, GitLab CI, Prometheus, Grafana |
| **Coût Estimé** | €0-500 (serveurs existants réutilisés) |
| **Équipe** | 1 Tech Lead + 1-2 Développeurs |
| **Critères de Succès** | Application déployée automatiquement avec monitoring basique |

## 🎯 Objectifs de la Phase

### Objectifs Techniques
- ✅ Containeriser au moins 1 microservice (auth-service)
- ✅ Mettre en place un pipeline CI/CD fonctionnel
- ✅ Déployer localement avec Docker Compose
- ✅ Implémenter monitoring basique (Prometheus + Grafana)
- ✅ Établir les patterns et conventions

### Objectifs Business
- ✅ Démontrer la viabilité de l'approche
- ✅ Valider les choix technologiques
- ✅ Identifier les risques précoces
- ✅ Obtenir le go/no-go pour Phase 2

## 📋 Périmètre Fonctionnel

### Inclus dans POC
- **1 Microservice** : auth-service (Node.js + Express)
  - Login/Logout
  - JWT token generation
  - Basic user management
- **Frontend** : React SPA basique
  - Login page
  - Dashboard simple
- **Base de données** : PostgreSQL (1 instance)
- **Cache** : Redis (1 instance)
- **CI/CD** : GitLab CI avec stages basiques
- **Monitoring** : Prometheus + Grafana avec dashboards préconçus

### Exclu du POC (Phase 2+)
- ❌ Kubernetes / K3s
- ❌ Multiple microservices
- ❌ Haute disponibilité
- ❌ Service mesh
- ❌ Observabilité complète (logs, traces)
- ❌ Secrets management avancé (Vault)
- ❌ Production deployment

## 🛠️ Stack Technologique - Phase 1

| Composant | Outil Choisi | Justification | Coût |
|-----------|--------------|---------------|------|
| **Orchestration** | Docker Compose | Simple, rapide à setup | €0 |
| **CI/CD** | GitLab CI (Community Edition) | Open-source, complet | €0 |
| **Registry** | GitLab Container Registry | Intégré à GitLab | €0 |
| **Monitoring** | Prometheus | Standard de facto | €0 |
| **Visualization** | Grafana | Compatible Prometheus | €0 |
| **Database** | PostgreSQL 15 | Mature, performant | €0 |
| **Cache** | Redis 7 | Rapide, simple | €0 |
| **Load Balancer** | Nginx | Léger, efficace | €0 |
| **Backend** | Node.js 20 + Express | Écosystème riche | €0 |
| **Frontend** | React 18 + Vite | Modern, performant | €0 |

**Total Phase 1** : **€0** (utilisation serveurs existants)

## 📐 Architecture POC

```mermaid
graph TB
    subgraph "Developer Workstation"
        DEV[Developer]
        VSCODE[VS Code]
    end
    
    subgraph "GitLab Server"
        GIT[Git Repository]
        CI[GitLab CI Runner]
        REG[Container Registry]
    end
    
    subgraph "Docker Compose Stack - Single Server"
        NGINX[Nginx<br/>Reverse Proxy]
        
        subgraph "Application"
            FRONT[Frontend<br/>React]
            AUTH[Auth Service<br/>Node.js:3001]
        end
        
        subgraph "Data"
            PG[(PostgreSQL<br/>:5432)]
            REDIS[(Redis<br/>:6379)]
        end
        
        subgraph "Monitoring"
            PROM[Prometheus<br/>:9090]
            GRAFANA[Grafana<br/>:3000]
        end
    end
    
    DEV -->|1. Code| VSCODE
    VSCODE -->|2. Push| GIT
    GIT -->|3. Trigger| CI
    CI -->|4. Build & Test| CI
    CI -->|5. Push Image| REG
    CI -->|6. Deploy| NGINX
    
    NGINX -->|/| FRONT
    NGINX -->|/api/auth| AUTH
    
    AUTH --> PG
    AUTH --> REDIS
    
    AUTH -.->|Metrics| PROM
    PROM --> GRAFANA
    
    style DEV fill:#e1f5ff
    style CI fill:#fff4e1
    style AUTH fill:#e8f5e9
    style PROM fill:#fce4ec
    style GRAFANA fill:#fce4ec
```

## 🗂️ Structure du Projet

```
devops-microservices-poc/
├── services/
│   ├── auth-service/
│   │   ├── src/
│   │   │   ├── controllers/
│   │   │   ├── models/
│   │   │   ├── routes/
│   │   │   ├── middleware/
│   │   │   └── server.js
│   │   ├── tests/
│   │   ├── Dockerfile
│   │   ├── package.json
│   │   └── .env.example
│   └── frontend/
│       ├── src/
│       ├── public/
│       ├── Dockerfile
│       └── package.json
│
├── infrastructure/
│   ├── docker-compose.yml
│   ├── docker-compose.prod.yml
│   ├── nginx/
│   │   └── nginx.conf
│   └── monitoring/
│       ├── prometheus.yml
│       └── grafana/
│           ├── dashboards/
│           └── datasources.yml
│
├── .gitlab-ci.yml
├── README.md
└── docs/
```

## 🔧 Configuration Docker Compose

### docker-compose.yml

```yaml
version: '3.9'

services:
  # PostgreSQL Database
  postgres:
    image: postgres:15-alpine
    container_name: poc-postgres
    environment:
      POSTGRES_DB: authdb
      POSTGRES_USER: authuser
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U authuser"]
      interval: 10s
      timeout: 5s
      retries: 5

  # Redis Cache
  redis:
    image: redis:7-alpine
    container_name: poc-redis
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 3s
      retries: 5

  # Auth Service
  auth-service:
    build:
      context: ./services/auth-service
      dockerfile: Dockerfile
    container_name: poc-auth-service
    environment:
      NODE_ENV: production
      PORT: 3001
      DATABASE_URL: postgresql://authuser:${POSTGRES_PASSWORD}@postgres:5432/authdb
      REDIS_URL: redis://redis:6379
      JWT_SECRET: ${JWT_SECRET}
    ports:
      - "3001:3001"
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3001/health"]
      interval: 30s
      timeout: 10s
      retries: 3

  # Frontend
  frontend:
    build:
      context: ./services/frontend
      dockerfile: Dockerfile
    container_name: poc-frontend
    ports:
      - "80:80"
    depends_on:
      - auth-service

  # Nginx Reverse Proxy
  nginx:
    image: nginx:alpine
    container_name: poc-nginx
    ports:
      - "80:80"
    volumes:
      - ./infrastructure/nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - frontend
      - auth-service

  # Prometheus
  prometheus:
    image: prom/prometheus:latest
    container_name: poc-prometheus
    volumes:
      - ./infrastructure/monitoring/prometheus.yml:/etc/prometheus/prometheus.yml:ro
      - prometheus_data:/prometheus
    ports:
      - "9090:9090"
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--storage.tsdb.retention.time=30d'

  # Grafana
  grafana:
    image: grafana/grafana:latest
    container_name: poc-grafana
    environment:
      GF_SECURITY_ADMIN_PASSWORD: ${GRAFANA_PASSWORD}
      GF_USERS_ALLOW_SIGN_UP: false
    volumes:
      - ./infrastructure/monitoring/grafana/datasources.yml:/etc/grafana/provisioning/datasources/datasources.yml:ro
      - ./infrastructure/monitoring/grafana/dashboards:/etc/grafana/provisioning/dashboards:ro
      - grafana_data:/var/lib/grafana
    ports:
      - "3000:3000"
    depends_on:
      - prometheus

volumes:
  postgres_data:
  redis_data:
  prometheus_data:
  grafana_data:

networks:
  default:
    name: poc-network
```

## 🔄 Pipeline CI/CD - Phase 1

### .gitlab-ci.yml (Simplifié)

```yaml
stages:
  - lint
  - test
  - build
  - deploy

variables:
  DOCKER_DRIVER: overlay2

# Lint Stage
lint:auth-service:
  stage: lint
  image: node:20-alpine
  script:
    - cd services/auth-service
    - npm ci
    - npm run lint
  only:
    - merge_requests
    - main

# Test Stage
test:auth-service:
  stage: test
  image: node:20-alpine
  services:
    - postgres:15-alpine
    - redis:7-alpine
  variables:
    POSTGRES_DB: test_db
    POSTGRES_USER: test_user
    POSTGRES_PASSWORD: test_pass
  script:
    - cd services/auth-service
    - npm ci
    - npm run test
  coverage: '/Coverage: \d+\.\d+%/'

# Build Stage
build:auth-service:
  stage: build
  image: docker:24-dind
  services:
    - docker:24-dind
  before_script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
  script:
    - cd services/auth-service
    - docker build -t $CI_REGISTRY_IMAGE/auth-service:$CI_COMMIT_SHA .
    - docker build -t $CI_REGISTRY_IMAGE/auth-service:latest .
    - docker push $CI_REGISTRY_IMAGE/auth-service:$CI_COMMIT_SHA
    - docker push $CI_REGISTRY_IMAGE/auth-service:latest
  only:
    - main

build:frontend:
  stage: build
  image: docker:24-dind
  services:
    - docker:24-dind
  before_script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
  script:
    - cd services/frontend
    - docker build -t $CI_REGISTRY_IMAGE/frontend:$CI_COMMIT_SHA .
    - docker build -t $CI_REGISTRY_IMAGE/frontend:latest .
    - docker push $CI_REGISTRY_IMAGE/frontend:$CI_COMMIT_SHA
    - docker push $CI_REGISTRY_IMAGE/frontend:latest
  only:
    - main

# Deploy Stage (manual for POC)
deploy:dev:
  stage: deploy
  image: alpine:latest
  before_script:
    - apk add --no-cache openssh-client docker-compose
  script:
    - ssh deploy@$DEV_SERVER "cd /opt/poc && docker-compose pull && docker-compose up -d"
  environment:
    name: development
    url: http://dev.example.com
  when: manual
  only:
    - main
```

## 📊 Monitoring - Phase 1

### Prometheus Configuration

```yaml
# infrastructure/monitoring/prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  # Auth Service Metrics
  - job_name: 'auth-service'
    static_configs:
      - targets: ['auth-service:3001']
    metrics_path: '/metrics'

  # Prometheus self-monitoring
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  # Node Exporter (system metrics)
  - job_name: 'node'
    static_configs:
      - targets: ['node-exporter:9100']

  # PostgreSQL Exporter
  - job_name: 'postgres'
    static_configs:
      - targets: ['postgres-exporter:9187']

  # Redis Exporter
  - job_name: 'redis'
    static_configs:
      - targets: ['redis-exporter:9121']
```

### Métriques Essentielles

#### Application Metrics (Custom)
```javascript
// services/auth-service/src/metrics.js
const promClient = require('prom-client');

// Register
const register = new promClient.Registry();

// Default metrics (CPU, Memory)
promClient.collectDefaultMetrics({ register });

// Custom metrics
const httpRequestDuration = new promClient.Histogram({
  name: 'http_request_duration_seconds',
  help: 'Duration of HTTP requests in seconds',
  labelNames: ['method', 'route', 'status_code'],
  buckets: [0.1, 0.3, 0.5, 0.7, 1, 3, 5, 7, 10]
});

const httpRequestsTotal = new promClient.Counter({
  name: 'http_requests_total',
  help: 'Total number of HTTP requests',
  labelNames: ['method', 'route', 'status_code']
});

const activeUsers = new promClient.Gauge({
  name: 'active_users_total',
  help: 'Total number of active users'
});

const authAttempts = new promClient.Counter({
  name: 'auth_attempts_total',
  help: 'Total authentication attempts',
  labelNames: ['status'] // success, failure
});

register.registerMetric(httpRequestDuration);
register.registerMetric(httpRequestsTotal);
register.registerMetric(activeUsers);
register.registerMetric(authAttempts);

module.exports = { register, httpRequestDuration, httpRequestsTotal, activeUsers, authAttempts };
```

### Grafana Dashboards

#### Dashboard 1: Application Overview
- **Panels** :
  - Request rate (req/s)
  - Request latency (p50, p95, p99)
  - Error rate (%)
  - Active users

#### Dashboard 2: System Resources
- **Panels** :
  - CPU usage (%)
  - Memory usage (MB)
  - Disk I/O
  - Network traffic

#### Dashboard 3: Database
- **Panels** :
  - Connections count
  - Query duration
  - Transaction rate
  - Cache hit ratio

## 📅 Timeline Détaillée - 4 Semaines

### Semaine 1 : Setup & Fondations

#### Sprint 1.1 (Jours 1-2)
**Epic** : Setup Infrastructure de Base

**Tâches** :
- [ ] **TASK-001** : Setup GitLab CE self-hosted (4h)
  - Installation sur serveur existant
  - Configuration initiale
  - Création groupe/projet

- [ ] **TASK-002** : Configuration Docker sur serveur dev (2h)
  - Installation Docker + Docker Compose
  - Configuration réseau
  - Tests basiques

- [ ] **TASK-003** : Initialisation repository Git (2h)
  - Structure dossiers
  - README initial
  - .gitignore approprié

#### Sprint 1.2 (Jours 3-5)
**Epic** : Développement Auth Service

**Tâches** :
- [ ] **TASK-004** : Setup projet Node.js auth-service (4h)
  - Initialisation npm
  - Structure de base
  - Dependencies (Express, pg, redis, jsonwebtoken)

- [ ] **TASK-005** : Implémentation endpoints auth (8h)
  - POST /api/auth/register
  - POST /api/auth/login
  - POST /api/auth/logout
  - GET /api/auth/me

- [ ] **TASK-006** : Connexion PostgreSQL + Redis (4h)
  - Models User
  - Session management
  - Password hashing (bcrypt)

- [ ] **TASK-007** : Tests unitaires auth-service (6h)
  - Setup Jest
  - Tests controllers
  - Tests models
  - Coverage > 70%

### Semaine 2 : Containerisation & CI/CD

#### Sprint 2.1 (Jours 6-8)
**Epic** : Containerisation

**Tâches** :
- [ ] **TASK-008** : Dockerfile auth-service (3h)
  - Multi-stage build
  - Optimisation image size
  - Security best practices

- [ ] **TASK-009** : docker-compose.yml (4h)
  - Tous les services
  - Volumes persistence
  - Health checks

- [ ] **TASK-010** : Setup frontend React basique (6h)
  - Create React App
  - Login page
  - Dashboard simple
  - API integration

- [ ] **TASK-011** : Dockerfile frontend (2h)
  - Build optimisé
  - Nginx serving

#### Sprint 2.2 (Jours 9-10)
**Epic** : CI/CD Pipeline

**Tâches** :
- [ ] **TASK-012** : Configuration GitLab Runner (3h)
  - Installation runner
  - Enregistrement avec GitLab
  - Tests basiques

- [ ] **TASK-013** : Pipeline .gitlab-ci.yml (6h)
  - Stages lint, test, build
  - Cache npm
  - Artifacts

- [ ] **TASK-014** : Intégration GitLab Container Registry (2h)
  - Configuration
  - Push automatique images
  - Tagging stratégie

### Semaine 3 : Monitoring & Tests

#### Sprint 3.1 (Jours 11-13)
**Epic** : Monitoring Basique

**Tâches** :
- [ ] **TASK-015** : Setup Prometheus (3h)
  - Configuration yaml
  - Scrape configs
  - Ajout au docker-compose

- [ ] **TASK-016** : Instrumentation auth-service (4h)
  - prom-client integration
  - Custom metrics
  - /metrics endpoint

- [ ] **TASK-017** : Setup Grafana (3h)
  - Configuration
  - Datasource Prometheus
  - User authentication

- [ ] **TASK-018** : Dashboards Grafana (6h)
  - Dashboard application
  - Dashboard système
  - Dashboard database

#### Sprint 3.2 (Jours 14-15)
**Epic** : Tests & Validation

**Tâches** :
- [ ] **TASK-019** : Tests d'intégration (6h)
  - Tests end-to-end avec supertest
  - Tests database
  - Tests Redis

- [ ] **TASK-020** : Tests de charge basiques (4h)
  - k6 ou Artillery
  - Scénario login/logout
  - Métriques performance

### Semaine 4 : Documentation & Demo

#### Sprint 4.1 (Jours 16-18)
**Epic** : Documentation

**Tâches** :
- [ ] **TASK-021** : Documentation technique (6h)
  - README détaillé
  - API documentation
  - Setup guide

- [ ] **TASK-022** : Runbook basique (4h)
  - Procédures déploiement
  - Troubleshooting commun
  - Commandes utiles

- [ ] **TASK-023** : Architecture diagrams (3h)
  - Mermaid diagrams
  - Infrastructure overview
  - Data flow

#### Sprint 4.2 (Jours 19-20)
**Epic** : Préparation Demo & Go/No-Go

**Tâches** :
- [ ] **TASK-024** : Préparation démo (4h)
  - Scénario démo
  - Données de test
  - Slides présentation

- [ ] **TASK-025** : Rétrospective Phase 1 (3h)
  - What went well
  - What to improve
  - Action items Phase 2

- [ ] **TASK-026** : Décision Go/No-Go Phase 2 (1h)
  - Présentation stakeholders
  - Validation budget Phase 2
  - Planning Phase 2

## ✅ Critères de Succès POC

### Critères Techniques

| Critère | Objectif | Validation |
|---------|----------|------------|
| **Build automatique** | CI/CD fonctionnel | ✅ Pipeline green sur main |
| **Déploiement** | docker-compose up réussit | ✅ Tous containers healthy |
| **Tests** | Coverage > 70% | ✅ Tests passent en CI |
| **Monitoring** | Métriques visibles | ✅ Dashboards Grafana opérationnels |
| **Performance** | < 200ms latence | ✅ p95 sous 200ms |
| **Documentation** | Complète et claire | ✅ Nouveau dev peut setup en < 30min |

### Critères Business

| Critère | Objectif | Validation |
|---------|----------|------------|
| **Coût** | < €500 | ✅ Budget respecté |
| **Délai** | 4 semaines max | ✅ Livré à temps |
| **Qualité** | Pas de bugs critiques | ✅ Demo réussie |
| **Équipe** | Montée en compétence | ✅ Équipe autonome sur Docker/CI/CD |

## ⚠️ Risques Phase 1

| Risque | Probabilité | Impact | Mitigation |
|--------|-------------|--------|------------|
| **Compétences Docker insuffisantes** | Moyenne | Moyen | Formation 2 jours avant démarrage |
| **Problèmes hardware serveur** | Faible | Élevé | Backup sur autre serveur disponible |
| **Complexité sous-estimée** | Moyenne | Moyen | Buffer 1 semaine inclus |
| **GitLab CE bugs** | Faible | Faible | Fallback GitHub Actions possible |

## 💰 Coûts Phase 1

### Infrastructure (CAPEX)

| Item | Quantité | Coût Unitaire | Total |
|------|----------|---------------|-------|
| **Serveur dev** | 1 | €0 (existant) | €0 |
| **Disques additionnels** | 0 | - | €0 |
| **Licences** | 0 | €0 (open-source) | €0 |
| **TOTAL CAPEX** | | | **€0** |

### Ressources Humaines

| Rôle | FTE | Semaines | Coût |
|------|-----|----------|------|
| **Tech Lead** | 1.0 | 4 | Interne |
| **Développeur** | 1.0 | 4 | Interne |
| **TOTAL RH** | 2.0 | 4 | Interne |

### Formation

| Formation | Durée | Coût |
|-----------|-------|------|
| **Docker Fundamentals** | 16h | €0 (Udemy en promo) |
| **GitLab CI/CD** | 8h | €0 (doc officielle) |
| **TOTAL Formation** | 24h | **€0-200** |

### Total Phase 1
**Budget Total** : **€0-500** (avec marge sécurité)

## 📈 Métriques Phase 1

### Développement

| Métrique | Objectif |
|----------|----------|
| **Commits/jour** | > 5 |
| **Pipeline success rate** | > 85% |
| **Test coverage** | > 70% |
| **Build time** | < 5 min |

### Opérations

| Métrique | Objectif |
|----------|----------|
| **Deploy time** | < 5 min |
| **Uptime** | > 95% (POC) |
| **P95 latency** | < 200ms |
| **Error rate** | < 1% |

## 🎓 Formation Équipe - Phase 1

### Semaine 0 (Pré-Phase 1)
- **Docker & Containers** (16h)
  - Docker fundamentals
  - Docker Compose
  - Networking
  - Volumes

- **Git Advanced** (4h)
  - GitFlow
  - Rebasing
  - Conflict resolution

### Durant Phase 1 (On-the-job)
- **CI/CD with GitLab** (8h)
  - Pipeline syntax
  - Runners
  - Cache & artifacts

- **Prometheus & Grafana** (6h)
  - Metrics collection
  - PromQL basics
  - Dashboard creation

## 🚦 Go/No-Go Décision

### Critères Go (Minimum pour Phase 2)
- ✅ Application fonctionne end-to-end
- ✅ Pipeline CI/CD opérationnel
- ✅ Monitoring basique en place
- ✅ Tests > 70% coverage
- ✅ Documentation complète
- ✅ Budget Phase 1 respecté
- ✅ Équipe compétente sur stack
- ✅ Pas de blockers techniques majeurs

### Critères No-Go (Arrêt ou Pivot)
- ❌ Bugs critiques non résolus
- ❌ Performance inacceptable (> 1s latency)
- ❌ Dépassement budget > 200%
- ❌ Équipe incompétente malgré formation
- ❌ Architecture fondamentalement flawed

## 📚 Livrables Phase 1

### Code
- ✅ auth-service fonctionnel et testé
- ✅ Frontend React basique
- ✅ docker-compose.yml complet
- ✅ .gitlab-ci.yml fonctionnel

### Infrastructure
- ✅ GitLab CE configuré
- ✅ GitLab Runner opérationnel
- ✅ Container Registry utilisé
- ✅ Prometheus + Grafana

### Documentation
- ✅ README complet avec quick start
- ✅ API documentation
- ✅ Architecture diagrams
- ✅ Runbook basique
- ✅ Lessons learned

### Présentation
- ✅ Slides démo
- ✅ Vidéo enregistrée (backup)
- ✅ Rapport Phase 1
- ✅ Recommandations Phase 2

## 🔄 Transition vers Phase 2

### Semaine 5 - Planning Phase 2
- ✅ Rétrospective approfondie
- ✅ Commande hardware Phase 2 (lead time!)
- ✅ Budget Phase 2 validé
- ✅ Équipe renforcée (si besoin)
- ✅ Formation Kubernetes planifiée

### Évolutions attendues
- **Orchestration** : Docker Compose → K3s
- **CI/CD** : Ajout ArgoCD pour GitOps
- **Monitoring** : Ajout Loki (logs)
- **Services** : 1 → 3+ microservices
- **Environnements** : Dev → Dev + Staging
- **Sécurité** : Ajout Vault, Network Policies

---

**Document Version** : 1.0  
**Dernière Mise à Jour** : 2026-02-12  
**Prochaine Revue** : Fin Semaine 4 (Go/No-Go)  
**Auteur** : Équipe DevOps
