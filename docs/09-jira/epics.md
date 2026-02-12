# 📋 JIRA Structure - Epics & Sprints

## Vue d'ensemble

Structure complète des epics, sprints et tickets pour les 3 phases du projet DevOps microservices on-premise (POC, MVP, Production).

## Organisation

- **18 Sprints** au total (2 semaines chacun)
- **12 Epics** majeurs
- **50+ Tickets** détaillés
- **Story Points** : Estimation Fibonacci (1, 2, 3, 5, 8, 13)

---

## PHASE 1 - POC (Sprints 1-2)

### EPIC-001 : Setup Infrastructure de Base
**Phase** : POC  
**Durée** : 1 sprint  
**Story Points** : 21

#### Description
Mise en place de l'infrastructure de base nécessaire pour le POC : GitLab CE self-hosted, Docker, et environnement de développement.

#### Objectif Business
Établir les fondations techniques pour permettre aux développeurs de commencer à travailler rapidement.

#### Livrables
- [x] GitLab CE opérationnel
- [x] GitLab Runner configuré
- [x] Repository Git structuré
- [x] Documentation setup

#### Critères d'Acceptation
1. GitLab accessible via URL interne
2. Au moins 1 runner actif
3. Repository créé avec structure de base
4. README avec instructions de setup

---

### EPIC-002 : Containerisation & Développement Auth Service
**Phase** : POC  
**Durée** : 2 sprints  
**Story Points** : 34

#### Description
Développement du premier microservice (auth-service) et sa containerisation avec Docker.

#### Objectif Business
Valider l'approche microservices et container-native avec un service fonctionnel.

#### Livrables
- [x] Auth service développé (Node.js + Express)
- [x] Endpoints API fonctionnels
- [x] Tests unitaires (coverage > 70%)
- [x] Dockerfile optimisé
- [x] docker-compose.yml

#### Dépendances
- EPIC-001 (Infrastructure)

#### Critères d'Acceptation
1. Service répond sur tous les endpoints
2. Tests passent en CI
3. Image Docker < 200MB
4. Démarrage < 5 secondes

---

### EPIC-003 : CI/CD Pipeline Basique
**Phase** : POC  
**Durée** : 1 sprint  
**Story Points** : 21

#### Description
Mise en place du pipeline CI/CD avec GitLab CI pour automatiser build, test et déploiement.

#### Objectif Business
Automatiser le cycle de développement pour réduire le time-to-market.

#### Livrables
- [x] .gitlab-ci.yml fonctionnel
- [x] Stages : lint, test, build, deploy
- [x] Container Registry utilisé
- [x] Déploiement automatique en dev

#### Critères d'Acceptation
1. Pipeline exécuté < 10 minutes
2. Success rate > 85%
3. Images pushées au registry
4. Déploiement automatique après merge main

---

### EPIC-004 : Monitoring Basique
**Phase** : POC  
**Durée** : 1 sprint  
**Story Points** : 13

#### Description
Implémentation du monitoring de base avec Prometheus et Grafana.

#### Objectif Business
Visibilité sur la santé du système dès le début.

#### Livrables
- [x] Prometheus déployé
- [x] Grafana déployé
- [x] Métriques applicatives collectées
- [x] 2 dashboards de base

#### Critères d'Acceptation
1. Métriques visibles dans Grafana
2. Alertes configurées (basiques)
3. Dashboards accessibles à l'équipe
4. Documentation utilisation

---

## PHASE 2 - MVP (Sprints 3-10)

### EPIC-005 : Migration vers K3s
**Phase** : MVP  
**Durée** : 2 sprints  
**Story Points** : 34

#### Description
Migration de Docker Compose vers cluster K3s pour bénéficier de l'orchestration Kubernetes.

#### Objectif Business
Préparer la scalabilité et la haute disponibilité futures.

#### Livrables
- [ ] K3s installé (2 nodes)
- [ ] Manifests Kubernetes créés
- [ ] Services migrés et opérationnels
- [ ] Ingress controller configuré

#### Dépendances
- EPIC-002 (Services containerisés)

#### Critères d'Acceptation
1. Cluster K3s stable
2. Tous les services déployés
3. Zéro downtime pendant migration
4. Performances équivalentes

---

### EPIC-006 : GitOps avec ArgoCD
**Phase** : MVP  
**Durée** : 2 sprints  
**Story Points** : 21

#### Description
Implémentation du pattern GitOps avec ArgoCD pour déploiements déclaratifs.

#### Objectif Business
Déploiements plus fiables et auditables via Git.

#### Livrables
- [ ] ArgoCD installé
- [ ] Applications ArgoCD configurées
- [ ] Repository manifests séparé
- [ ] Auto-sync et self-healing activés

#### Dépendances
- EPIC-005 (K3s opérationnel)

#### Critères d'Acceptation
1. ArgoCD UI accessible
2. Déploiements via Git push
3. Rollback fonctionnel
4. Sync < 2 minutes

---

### EPIC-007 : Secrets Management avec Vault
**Phase** : MVP  
**Durée** : 2 sprints  
**Story Points** : 21

#### Description
Mise en place de HashiCorp Vault pour gestion centralisée des secrets.

#### Objectif Business
Sécuriser les credentials et secrets de manière professionnelle.

#### Livrables
- [ ] Vault installé et configuré
- [ ] Secrets migrés depuis env vars
- [ ] Intégration K3s (CSI driver)
- [ ] Rotation automatique activée

#### Critères d'Acceptation
1. Vault HA opérationnel
2. Secrets injectés automatiquement
3. Audit logs activés
4. Documentation procédures

---

### EPIC-008 : Expansion Microservices
**Phase** : MVP  
**Durée** : 3 sprints  
**Story Points** : 55

#### Description
Développement de microservices additionnels : user-service, order-service, notification-service.

#### Objectif Business
Architecture microservices complète pour validation du pattern.

#### Livrables
- [ ] User service (Node.js)
- [ ] Order service (Python/FastAPI)
- [ ] Notification service (Python/Celery)
- [ ] API Gateway (Kong)
- [ ] Tests pour chaque service

#### Dépendances
- EPIC-005 (K3s)
- EPIC-006 (ArgoCD)

#### Critères d'Acceptation
1. Tous les services déployés
2. Communication inter-services OK
3. Tests passent
4. API Gateway route correctement

---

### EPIC-009 : Observabilité Complète
**Phase** : MVP  
**Durée** : 2 sprints  
**Story Points** : 34

#### Description
Extension de l'observabilité : ajout de Loki (logs) et Tempo (traces).

#### Objectif Business
Visibilité complète pour debugging et monitoring.

#### Livrables
- [ ] Loki déployé
- [ ] Tempo déployé
- [ ] Logs centralisés
- [ ] Distributed tracing
- [ ] Dashboards enrichis

#### Critères d'Acceptation
1. Logs de tous les services dans Loki
2. Traces distribuées visibles
3. Corrélation logs/metrics/traces
4. Performance acceptable

---

### EPIC-010 : Security Hardening
**Phase** : MVP  
**Durée** : 2 sprints  
**Story Points** : 34

#### Description
Renforcement de la sécurité : Network Policies, RBAC, scanning images.

#### Objectif Business
Conformité sécurité avant passage en production.

#### Livrables
- [ ] Network Policies K3s
- [ ] RBAC configuré
- [ ] Image scanning (Trivy) en CI
- [ ] Pod Security Policies
- [ ] TLS/mTLS activé

#### Critères d'Acceptation
1. 0 Critical CVE en images
2. Network isolation effective
3. RBAC testé
4. Audit security passé

---

## PHASE 3 - PRODUCTION (Sprints 11-18)

### EPIC-011 : Haute Disponibilité
**Phase** : Production  
**Durée** : 3 sprints  
**Story Points** : 55

#### Description
Migration vers architecture HA : 3+ nodes K3s, databases répliquées, load balancing.

#### Objectif Business
Garantir 99.9% uptime pour production.

#### Livrables
- [ ] K3s 3 nodes (HA)
- [ ] PostgreSQL Patroni (auto-failover)
- [ ] MongoDB Replica Set
- [ ] Redis Cluster
- [ ] HAProxy load balancing

#### Dépendances
- Tous les epics Phase 2

#### Critères d'Acceptation
1. No single point of failure
2. Failover automatique testé
3. Uptime > 99.9%
4. Performances maintenues

---

### EPIC-012 : Service Mesh & Advanced Networking
**Phase** : Production  
**Durée** : 2 sprints  
**Story Points** : 34

#### Description
Implémentation Linkerd pour mTLS, observability, et traffic management avancé.

#### Objectif Business
Sécurité et résilience inter-services renforcées.

#### Livrables
- [ ] Linkerd installé
- [ ] mTLS automatique
- [ ] Traffic splitting (canary)
- [ ] Circuit breakers
- [ ] Metrics enrichies

#### Critères d'Acceptation
1. mTLS actif entre tous les services
2. Canary deployment fonctionnel
3. Circuit breaker déclenché correctement
4. Latency overhead < 1ms

---

### EPIC-013 : Disaster Recovery & Backup
**Phase** : Production  
**Durée** : 2 sprints  
**Story Points** : 34

#### Description
Stratégie complète de backup et disaster recovery avec Velero et scripts automatisés.

#### Objectif Business
Garantir RTO < 4h et RPO < 1h.

#### Livrables
- [ ] Velero installé
- [ ] Backups automatiques (quotidiens)
- [ ] Procédures restore documentées
- [ ] DR testé avec succès
- [ ] Backup offsite configuré

#### Critères d'Acceptation
1. Backup automatique OK
2. Restore testé < 4h
3. Données < 1h de perte max
4. Documentation complète

---

### EPIC-014 : Production Readiness & Documentation
**Phase** : Production  
**Durée** : 2 sprints  
**Story Points** : 21

#### Description
Finalisation documentation, runbooks, formation équipe et audit pré-production.

#### Objectif Business
Équipe autonome et opérationnelle pour maintenir la production.

#### Livrables
- [ ] Runbooks complets
- [ ] Formation équipe (40h)
- [ ] Certifications obtenues (CKA)
- [ ] Audit sécurité passé
- [ ] Go-live checklist validée

#### Critères d'Acceptation
1. Documentation à jour (100%)
2. Équipe formée et autonome
3. 0 Critical issues
4. Approbation go-live

---

## Tickets Détaillés (50 premiers)

### Sprint 1 - Setup Infrastructure

#### TASK-001 : Installation GitLab CE
**Epic** : EPIC-001  
**Story Points** : 5  
**Priorité** : Haute

**Description**  
Installer GitLab Community Edition en self-hosted sur serveur dédié via Docker Compose.

**Critères d'Acceptation**
- [ ] GitLab accessible via https://gitlab.internal
- [ ] Admin account créé
- [ ] Email notifications configurées
- [ ] HTTPS activé (self-signed OK pour POC)

**Étapes Techniques**
1. Créer docker-compose.yml pour GitLab
2. Configurer volumes pour persistence
3. Démarrer et vérifier logs
4. Accéder UI et compléter setup initial
5. Créer groupe "microservices"

**Estimation** : 4 heures

---

#### TASK-002 : Configuration GitLab Runner
**Epic** : EPIC-001  
**Story Points** : 3  
**Priorité** : Haute

**Description**  
Installer et configurer GitLab Runner pour exécuter les pipelines CI/CD.

**Critères d'Acceptation**
- [ ] Runner enregistré et actif
- [ ] Executor Docker configuré
- [ ] Test pipeline exécuté avec succès

**Étapes Techniques**
1. Installer gitlab-runner via Docker
2. Obtenir registration token depuis GitLab
3. Enregistrer runner avec executor docker
4. Vérifier runner dans GitLab UI

**Dépendances** : TASK-001

**Estimation** : 2 heures

---

#### TASK-003 : Initialisation Repository Git
**Epic** : EPIC-001  
**Story Points** : 2  
**Priorité** : Moyenne

**Description**  
Créer la structure initiale du repository avec dossiers et fichiers de base.

**Critères d'Acceptation**
- [ ] Repository créé dans GitLab
- [ ] Structure dossiers conforme
- [ ] README.md avec instructions
- [ ] .gitignore approprié

**Étapes Techniques**
1. Créer projet dans GitLab
2. Clone local
3. Créer structure (services/, infrastructure/, docs/)
4. Commit et push initial

**Estimation** : 1 heure

---

#### TASK-004 : Setup Docker sur Serveur Dev
**Epic** : EPIC-001  
**Story Points** : 2  
**Priorité** : Haute

**Description**  
Installer et configurer Docker + Docker Compose sur serveur de développement.

**Critères d'Acceptation**
- [ ] Docker installé (version 24+)
- [ ] Docker Compose installé (v2)
- [ ] User ajouté au groupe docker
- [ ] Test hello-world réussi

**Étapes Techniques**
```bash
# Installation Docker
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER

# Installation Docker Compose
sudo apt install docker-compose-plugin

# Test
docker run hello-world
```

**Estimation** : 1 heure

---

#### TASK-005 : Configuration PostgreSQL pour Auth
**Epic** : EPIC-001  
**Story Points** : 3  
**Priorité** : Haute

**Description**  
Ajouter PostgreSQL au docker-compose avec configuration pour auth-service.

**Critères d'Acceptation**
- [ ] PostgreSQL 15 déployé
- [ ] Database "authdb" créée
- [ ] User "authuser" créé avec permissions
- [ ] Volume pour persistence
- [ ] Health check configuré

**Estimation** : 2 heures

---

### Sprint 1 - Développement Auth Service

#### TASK-006 : Initialisation Projet Auth Service
**Epic** : EPIC-002  
**Story Points** : 2  
**Priorité** : Haute

**Description**  
Créer le projet Node.js pour auth-service avec structure de base.

**Critères d'Acceptation**
- [ ] package.json configuré
- [ ] Dependencies installées (express, pg, bcrypt, jsonwebtoken)
- [ ] Structure MVC créée
- [ ] Server.js démarrable

**Étapes Techniques**
```bash
mkdir -p services/auth-service
cd services/auth-service
npm init -y
npm install express pg bcrypt jsonwebtoken dotenv
npm install -D jest supertest nodemon eslint
```

**Estimation** : 1 heure

---

#### TASK-007 : Implémentation User Model
**Epic** : EPIC-002  
**Story Points** : 3  
**Priorité** : Haute

**Description**  
Créer le model User avec méthodes CRUD et hashage password.

**Critères d'Acceptation**
- [ ] Schema user SQL créé
- [ ] Méthodes create, findById, findByEmail
- [ ] Password hashé avec bcrypt
- [ ] Validation email/password

**Estimation** : 3 heures

---

#### TASK-008 : Endpoint POST /api/auth/register
**Epic** : EPIC-002  
**Story Points** : 3  
**Priorité** : Haute

**Description**  
Implémenter l'endpoint d'enregistrement utilisateur.

**Critères d'Acceptation**
- [ ] Validation input (email, password, name)
- [ ] Vérification email unique
- [ ] Création user en DB
- [ ] Réponse 201 avec user (sans password)
- [ ] Gestion erreurs appropriée

**Code Example**
```javascript
router.post('/register', async (req, res) => {
  try {
    const { email, password, name } = req.body;
    
    // Validation
    if (!email || !password) {
      return res.status(400).json({ error: 'Email and password required' });
    }
    
    // Check if exists
    const existing = await User.findByEmail(email);
    if (existing) {
      return res.status(409).json({ error: 'Email already exists' });
    }
    
    // Create user
    const user = await User.create({ email, password, name });
    
    res.status(201).json({ user: { id: user.id, email: user.email, name: user.name } });
  } catch (error) {
    res.status(500).json({ error: 'Server error' });
  }
});
```

**Estimation** : 3 heures

---

#### TASK-009 : Endpoint POST /api/auth/login
**Epic** : EPIC-002  
**Story Points** : 5  
**Priorité** : Haute

**Description**  
Implémenter l'endpoint de login avec JWT.

**Critères d'Acceptation**
- [ ] Validation credentials
- [ ] Vérification password (bcrypt.compare)
- [ ] Génération JWT token
- [ ] Session Redis (optionnel Phase 1)
- [ ] Réponse 200 avec token

**Estimation** : 4 heures

---

#### TASK-010 : Endpoint POST /api/auth/logout
**Epic** : EPIC-002  
**Story Points** : 2  
**Priorité** : Moyenne

**Description**  
Implémenter logout (invalidation token côté client principalement).

**Critères d'Acceptation**
- [ ] Blacklist token si Redis disponible
- [ ] Réponse 200
- [ ] Instruction client de supprimer token

**Estimation** : 1 heure

---

#### TASK-011 : Endpoint GET /api/auth/me
**Epic** : EPIC-002  
**Story Points** : 3  
**Priorité** : Moyenne

**Description**  
Endpoint pour récupérer les infos de l'utilisateur connecté.

**Critères d'Acceptation**
- [ ] Vérification JWT dans headers
- [ ] Extraction user_id du token
- [ ] Retour infos user
- [ ] 401 si token invalide

**Estimation** : 2 heures

---

#### TASK-012 : Tests Unitaires Auth Service
**Epic** : EPIC-002  
**Story Points** : 8  
**Priorité** : Haute

**Description**  
Écrire tests unitaires complets pour auth-service.

**Critères d'Acceptation**
- [ ] Tests pour tous les endpoints
- [ ] Tests pour User model
- [ ] Mocking database
- [ ] Coverage > 70%
- [ ] Tests passent en local et CI

**Estimation** : 6 heures

---

### Sprint 2 - Containerisation

#### TASK-013 : Dockerfile Auth Service
**Epic** : EPIC-002  
**Story Points** : 3  
**Priorité** : Haute

**Description**  
Créer Dockerfile optimisé multi-stage pour auth-service.

**Critères d'Acceptation**
- [ ] Multi-stage build
- [ ] Image finale < 200MB
- [ ] Non-root user
- [ ] Health check inclus

**Dockerfile Example**
```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .

FROM node:20-alpine
WORKDIR /app
RUN addgroup -g 1001 -S nodejs && adduser -S nodejs -u 1001
COPY --from=builder --chown=nodejs:nodejs /app ./
USER nodejs
EXPOSE 3001
HEALTHCHECK --interval=30s --timeout=3s \
  CMD node healthcheck.js
CMD ["node", "src/server.js"]
```

**Estimation** : 2 heures

---

#### TASK-014 : docker-compose.yml Complet
**Epic** : EPIC-002  
**Story Points** : 5  
**Priorité** : Haute

**Description**  
Créer docker-compose.yml avec tous les services (auth, postgres, redis, monitoring).

**Critères d'Acceptation**
- [ ] Tous les services définis
- [ ] Health checks configurés
- [ ] Volumes pour persistence
- [ ] Network configuré
- [ ] Environnement variables gérées

**Estimation** : 4 heures

---

#### TASK-015 : Frontend React Basique
**Epic** : EPIC-002  
**Story Points** : 8  
**Priorité** : Moyenne

**Description**  
Créer frontend React simple avec login/logout.

**Critères d'Acceptation**
- [ ] Create React App ou Vite
- [ ] Page login
- [ ] Page dashboard (après login)
- [ ] Appels API auth-service
- [ ] Token stocké (localStorage)

**Estimation** : 6 heures

---

#### TASK-016 : Dockerfile Frontend
**Epic** : EPIC-002  
**Story Points** : 2  
**Priorité** : Moyenne

**Description**  
Dockerfile pour frontend avec Nginx.

**Critères d'Acceptation**
- [ ] Build optimisé (production)
- [ ] Nginx serving static files
- [ ] Image < 50MB

**Estimation** : 1 heure

---

### Sprint 2 - CI/CD Pipeline

#### TASK-017 : .gitlab-ci.yml Initial
**Epic** : EPIC-003  
**Story Points** : 5  
**Priorité** : Haute

**Description**  
Créer pipeline GitLab CI avec stages lint, test, build.

**Critères d'Acceptation**
- [ ] Stage lint avec ESLint
- [ ] Stage test avec Jest
- [ ] Stage build Docker image
- [ ] Cache npm configuré
- [ ] Pipeline exécuté < 10 min

**Estimation** : 4 heures

---

#### TASK-018 : Intégration Container Registry
**Epic** : EPIC-003  
**Story Points** : 3  
**Priorité** : Haute

**Description**  
Configurer push images vers GitLab Container Registry.

**Critères d'Acceptation**
- [ ] Images taguées correctement
- [ ] Push automatique après build
- [ ] Registry accessible
- [ ] Documentation pulling images

**Estimation** : 2 heures

---

#### TASK-019 : Stage Deploy Dev
**Epic** : EPIC-003  
**Story Points** : 5  
**Priorité** : Haute

**Description**  
Ajouter stage deploy automatique vers environnement dev.

**Critères d'Acceptation**
- [ ] SSH configuré vers serveur dev
- [ ] docker-compose pull + up automatique
- [ ] Déploiement uniquement sur branch main
- [ ] Notifications Slack (optionnel)

**Estimation** : 4 heures

---

### Sprint 3 - Monitoring

#### TASK-020 : Setup Prometheus
**Epic** : EPIC-004  
**Story Points** : 3  
**Priorité** : Haute

**Description**  
Déployer Prometheus dans docker-compose.

**Critères d'Acceptation**
- [ ] Prometheus accessible :9090
- [ ] prometheus.yml configuré
- [ ] Scrape auth-service
- [ ] Rétention 30 jours

**Estimation** : 2 heures

---

#### TASK-021 : Instrumentation Auth Service
**Epic** : EPIC-004  
**Story Points** : 5  
**Priorité** : Haute

**Description**  
Ajouter métriques Prometheus dans auth-service.

**Critères d'Acceptation**
- [ ] prom-client installé
- [ ] Métriques HTTP (duration, count)
- [ ] Métriques custom (auth attempts)
- [ ] Endpoint /metrics exposé

**Estimation** : 3 heures

---

#### TASK-022 : Setup Grafana
**Epic** : EPIC-004  
**Story Points** : 3  
**Priorité** : Haute

**Description**  
Déployer Grafana et connecter à Prometheus.

**Critères d'Acceptation**
- [ ] Grafana accessible :3000
- [ ] Datasource Prometheus configuré
- [ ] Authentication configurée
- [ ] Provisioning datasources via config

**Estimation** : 2 heures

---

#### TASK-023 : Dashboards Grafana
**Epic** : EPIC-004  
**Story Points** : 5  
**Priorité** : Moyenne

**Description**  
Créer 2 dashboards Grafana (Application + Système).

**Critères d'Acceptation**
- [ ] Dashboard "Application Performance"
  - Request rate
  - Latency (p50, p95, p99)
  - Error rate
  - Auth attempts
- [ ] Dashboard "System Resources"
  - CPU usage
  - Memory usage
  - Disk I/O

**Estimation** : 4 heures

---

(Continuer avec TASK-024 à TASK-050 couvrant les sprints 4-10 pour la Phase 2 MVP...)

---

## Matrice Sprints

| Sprint | Semaine | Epic(s) | Story Points | Focus |
|--------|---------|---------|--------------|-------|
| **Sprint 1** | S1-S2 | EPIC-001, EPIC-002 | 21 | Infrastructure + Auth Service |
| **Sprint 2** | S3-S4 | EPIC-002, EPIC-003, EPIC-004 | 26 | Containerisation + CI/CD + Monitoring |
| **Sprint 3** | S5-S6 | EPIC-005 | 17 | Migration K3s Part 1 |
| **Sprint 4** | S7-S8 | EPIC-005, EPIC-006 | 21 | K3s Part 2 + ArgoCD |
| **Sprint 5** | S9-S10 | EPIC-007 | 21 | Vault Secrets |
| **Sprint 6** | S11-S12 | EPIC-008 | 21 | Microservices Part 1 |
| **Sprint 7** | S13-S14 | EPIC-008 | 17 | Microservices Part 2 |
| **Sprint 8** | S15-S16 | EPIC-008 | 17 | Microservices Part 3 + API Gateway |
| **Sprint 9** | S17-S18 | EPIC-009 | 17 | Loki Logs |
| **Sprint 10** | S19-S20 | EPIC-009, EPIC-010 | 21 | Tempo Traces + Security |
| **Sprint 11** | S21-S22 | EPIC-010, EPIC-011 | 21 | Security + HA Part 1 |
| **Sprint 12** | S23-S24 | EPIC-011 | 17 | HA Part 2 |
| **Sprint 13** | S25-S26 | EPIC-011 | 17 | HA Part 3 + Tests |
| **Sprint 14** | S27-S28 | EPIC-012 | 17 | Service Mesh Part 1 |
| **Sprint 15** | S29-S30 | EPIC-012 | 17 | Service Mesh Part 2 |
| **Sprint 16** | S31-S32 | EPIC-013 | 17 | Disaster Recovery |
| **Sprint 17** | S33-S34 | EPIC-013 | 17 | Backup & Tests DR |
| **Sprint 18** | S35-S36 | EPIC-014 | 21 | Documentation + Formation + Go-Live |

**Total Story Points** : 336  
**Velocity Moyenne** : 18-20 points/sprint

---

## Labels & Tags

### Par Type
- `epic` - Epic
- `story` - User Story
- `task` - Task technique
- `bug` - Bug
- `spike` - Recherche/investigation

### Par Priorité
- `priority:critical` - Bloquant
- `priority:high` - Haute
- `priority:medium` - Moyenne
- `priority:low` - Basse

### Par Phase
- `phase:poc` - Phase 1 POC
- `phase:mvp` - Phase 2 MVP
- `phase:production` - Phase 3 Production

### Par Domaine
- `devops` - Infrastructure/DevOps
- `backend` - Développement backend
- `frontend` - Développement frontend
- `security` - Sécurité
- `monitoring` - Observabilité
- `documentation` - Documentation

---

**Document Version** : 1.0  
**Dernière Mise à Jour** : 2026-02-12  
**Prochaine Revue** : Après chaque sprint  
**Auteur** : Tech Lead DevOps
