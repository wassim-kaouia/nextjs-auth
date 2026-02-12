# 🔧 CI/CD Tools Comparison

## Vue d'ensemble

Comparaison détaillée des solutions CI/CD pour un déploiement on-premise avec contrainte budgétaire forte.

## Synthèse Rapide

| Outil | Score Global | Recommandation | Phase |
|-------|--------------|----------------|-------|
| **GitLab CI** | ⭐⭐⭐⭐⭐ 5/5 | ✅ **RECOMMANDÉ** | Toutes |
| **Jenkins** | ⭐⭐⭐⭐ 4/5 | ⚠️ Alternative viable | Toutes |
| **Drone CI** | ⭐⭐⭐⭐ 4/5 | ⚠️ Option lightweight | POC/MVP |
| **GitHub Actions (Self-Hosted)** | ⭐⭐⭐ 3/5 | 🔄 POC uniquement | POC |
| **CircleCI (Self-Hosted)** | ⭐⭐ 2/5 | ❌ Pas adapté | - |
| **TeamCity** | ⭐⭐ 2/5 | ❌ Coût élevé | - |

## Comparaison Détaillée

### 1. GitLab CI (Community Edition)

#### ✅ Avantages
- **Intégration totale** : Git + CI/CD + Registry + Wiki dans une seule plateforme
- **100% Open-source** : Community Edition gratuite et complète
- **Mature et stable** : Utilisé par des millions de projets
- **Écosystème riche** : Templates, Auto DevOps, Security scanning
- **Container-native** : Docker-in-Docker natif
- **GitOps-ready** : Compatible ArgoCD, Flux
- **Documentation excellente** : Très complète
- **UI moderne** : Interface intuitive

#### ❌ Inconvénients
- **Ressources** : Gourmand en RAM (4GB minimum recommandé)
- **Complexité initiale** : Courbe d'apprentissage moyenne
- **Updates fréquentes** : Nécessite maintenance mensuelle

#### 💰 Coût
| Édition | Coût | Features |
|---------|------|----------|
| **Community Edition (CE)** | **€0** | CI/CD, Registry, Wiki, Issues |
| **Starter** | €4/user/mois | + Support |
| **Premium** | €19/user/mois | + Advanced features |
| **Ultimate** | €99/user/mois | + Security, Compliance |

**Pour on-premise budget minimal** : **GitLab CE (€0)**

#### 🔧 Configuration Type

```yaml
# .gitlab-ci.yml
stages:
  - build
  - test
  - deploy

build:
  stage: build
  image: node:20-alpine
  script:
    - npm ci
    - npm run build
  artifacts:
    paths:
      - dist/
    expire_in: 1 day

test:
  stage: test
  image: node:20-alpine
  script:
    - npm ci
    - npm run test
  coverage: '/Coverage: \d+\.\d+%/'

deploy:
  stage: deploy
  image: bitnami/kubectl
  script:
    - kubectl apply -f k8s/
  only:
    - main
```

#### 📊 Scoring Détaillé

| Critère | Score | Note |
|---------|-------|------|
| **Coût** | 5/5 | 100% gratuit |
| **Facilité setup** | 4/5 | Docker Compose simple |
| **Facilité d'utilisation** | 5/5 | UI intuitive |
| **Performance** | 4/5 | Rapide avec cache |
| **Intégration** | 5/5 | Tout-en-un |
| **Community** | 5/5 | Énorme |
| **Documentation** | 5/5 | Excellente |
| **Maintenance** | 4/5 | Updates régulières |
| **TOTAL** | **37/40** | **92%** |

#### 🎯 Recommandation
✅ **CHOIX OPTIMAL pour toutes les phases**
- POC : GitLab CI basique
- MVP : GitLab CI + Auto DevOps
- Production : GitLab CI + ArgoCD (GitOps)

---

### 2. Jenkins

#### ✅ Avantages
- **Open-source** : 100% gratuit
- **Très mature** : 15+ ans d'existence
- **Extensible** : 1800+ plugins
- **Flexible** : Supporte tout type de workflow
- **Grande communauté** : Beaucoup de ressources
- **On-premise friendly** : Conçu pour self-hosting

#### ❌ Inconvénients
- **Configuration complexe** : Groovy DSL peu intuitif
- **UI datée** : Interface vieillotte
- **Maintenance lourde** : Plugins à gérer
- **Pas de Git intégré** : Besoin GitLab/GitHub séparé
- **Pas de Registry intégré** : Besoin Harbor/Docker Registry
- **Sécurité** : Historique de CVE

#### 💰 Coût
- **Jenkins Open Source** : €0
- **CloudBees CI** (Commercial) : €€€€ (non recommandé pour budget limité)

#### 🔧 Configuration Type

```groovy
// Jenkinsfile
pipeline {
    agent {
        docker {
            image 'node:20-alpine'
        }
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'npm ci'
                sh 'npm run build'
            }
        }
        
        stage('Test') {
            steps {
                sh 'npm run test'
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}
```

#### 📊 Scoring Détaillé

| Critère | Score | Note |
|---------|-------|------|
| **Coût** | 5/5 | 100% gratuit |
| **Facilité setup** | 3/5 | Configuration manuelle |
| **Facilité d'utilisation** | 2/5 | Groovy DSL complexe |
| **Performance** | 4/5 | Bon avec optimisation |
| **Intégration** | 3/5 | Via plugins |
| **Community** | 5/5 | Très large |
| **Documentation** | 4/5 | Complète mais dispersée |
| **Maintenance** | 2/5 | Gestion plugins lourde |
| **TOTAL** | **28/40** | **70%** |

#### 🎯 Recommandation
⚠️ **ALTERNATIVE** si compétences Jenkins existantes dans l'équipe
- Préférer GitLab CI pour projet from scratch
- Acceptable si migration depuis Jenkins existant

---

### 3. Drone CI

#### ✅ Avantages
- **Open-source** : Apache License 2.0
- **Lightweight** : Très léger (< 50MB)
- **Container-native** : Tout run dans Docker
- **Simple** : Configuration YAML straightforward
- **Performant** : Rapide, peu de overhead
- **Intégration Git** : GitHub, GitLab, Gitea

#### ❌ Inconvénients
- **Moins mature** : Plus récent que GitLab/Jenkins
- **Communauté plus petite** : Moins de ressources
- **Features limitées** : Pas de Registry, Wiki, etc.
- **UI minimaliste** : Fonctionnel mais basique
- **Plugins limités** : Écosystème plus restreint

#### 💰 Coût
- **Drone OSS** : €0
- **Drone Enterprise** : €€€ (pas nécessaire)

#### 🔧 Configuration Type

```yaml
# .drone.yml
kind: pipeline
type: docker
name: default

steps:
  - name: build
    image: node:20-alpine
    commands:
      - npm ci
      - npm run build

  - name: test
    image: node:20-alpine
    commands:
      - npm run test

  - name: deploy
    image: bitnami/kubectl
    commands:
      - kubectl apply -f k8s/
    when:
      branch:
        - main
```

#### 📊 Scoring Détaillé

| Critère | Score | Note |
|---------|-------|------|
| **Coût** | 5/5 | 100% gratuit |
| **Facilité setup** | 5/5 | Très simple |
| **Facilité d'utilisation** | 4/5 | YAML simple |
| **Performance** | 5/5 | Très rapide |
| **Intégration** | 3/5 | Basique |
| **Community** | 3/5 | Croissante |
| **Documentation** | 4/5 | Bonne |
| **Maintenance** | 5/5 | Minimale |
| **TOTAL** | **34/40** | **85%** |

#### 🎯 Recommandation
⚠️ **BON CHOIX** pour POC/MVP si besoin légèreté
- Excellente alternative à GitLab CI
- Manque features "all-in-one" pour production

---

### 4. GitHub Actions (Self-Hosted Runners)

#### ✅ Avantages
- **UI moderne** : Interface GitHub familière
- **Marketplace** : 10,000+ actions prêtes
- **Syntaxe claire** : YAML intuitif
- **Intégration GitHub** : Parfaite si déjà sur GitHub
- **Matrix builds** : Tests multi-versions faciles

#### ❌ Inconvénients
- **Dépendance cloud** : GitHub.com nécessaire
- **Self-hosted limité** : Fonctionnalités réduites
- **Pas de Registry** : Besoin solution externe
- **Coût caché** : Minutes gratuites limitées
- **On-premise** : GitHub Enterprise Server €€€€

#### 💰 Coût
- **GitHub.com + Self-Hosted Runners** : €0 minutes illimitées
- **GitHub Enterprise Server** : $21/user/mois (❌ trop cher)

#### 🔧 Configuration Type

```yaml
# .github/workflows/main.yml
name: CI/CD

on:
  push:
    branches: [main]
  pull_request:

jobs:
  build:
    runs-on: self-hosted
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: 20
      
      - name: Build
        run: |
          npm ci
          npm run build
      
      - name: Test
        run: npm run test
      
      - name: Deploy
        if: github.ref == 'refs/heads/main'
        run: kubectl apply -f k8s/
```

#### 📊 Scoring Détaillé

| Critère | Score | Note |
|---------|-------|------|
| **Coût** | 4/5 | Gratuit mais dépendance cloud |
| **Facilité setup** | 4/5 | Runner simple |
| **Facilité d'utilisation** | 5/5 | Très intuitif |
| **Performance** | 4/5 | Bon |
| **Intégration** | 3/5 | GitHub uniquement |
| **Community** | 5/5 | Énorme |
| **Documentation** | 5/5 | Excellente |
| **Maintenance** | 4/5 | Minimale |
| **TOTAL** | **34/40** | **85%** |

#### 🎯 Recommandation
🔄 **ACCEPTABLE pour POC** si déjà sur GitHub
- Non recommandé pour on-premise strict
- GitLab CI préférable pour autonomie complète

---

### 5. CircleCI (Self-Hosted)

#### ❌ Pourquoi Non Recommandé
- **Coût élevé** : Server edition €€€€
- **Cloud-first** : Conçu pour cloud
- **On-premise limité** : Features réduites
- **Setup complexe** : Nécessite Nomad cluster

#### 💰 Coût
- **CircleCI Cloud** : €0-€€ (limité)
- **CircleCI Server** : $15,000/an minimum (❌)

#### 🎯 Recommandation
❌ **NON RECOMMANDÉ** pour on-premise budget limité

---

### 6. TeamCity

#### ❌ Pourquoi Non Recommandé
- **Coût** : €299/an (10 agents)
- **Complexité** : Setup lourd
- **Ressources** : Gourmand
- **Overkill** : Features inutiles pour notre cas

#### 💰 Coût
- **Professional** : €299/an (10 agents)
- **Enterprise** : €1,999/an (100 agents)

#### 🎯 Recommandation
❌ **NON RECOMMANDÉ** pour budget limité

---

## Tableau Comparatif Global

| Critère | GitLab CI | Jenkins | Drone CI | GitHub Actions | CircleCI | TeamCity |
|---------|-----------|---------|----------|----------------|----------|----------|
| **Coût On-Premise** | ✅ €0 | ✅ €0 | ✅ €0 | ⚠️ Dépendance cloud | ❌ €€€€ | ❌ €€€ |
| **Facilité Setup** | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐ |
| **Facilité Utilisation** | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Performance** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Features Intégrées** | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Communauté** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Documentation** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Maintenance** | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| **On-Premise Friendly** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐ | ⭐⭐⭐ |
| **SCORE TOTAL** | **37/40** | **28/40** | **34/40** | **34/40** | **25/40** | **25/40** |
| **Recommandation** | ✅ | ⚠️ | ⚠️ | 🔄 | ❌ | ❌ |

## Critères de Décision

### Budget Minimal (€0)
1. **GitLab CI** ✅
2. Drone CI ⚠️
3. Jenkins ⚠️

### Facilité d'Utilisation
1. **GitLab CI** ✅
2. GitHub Actions
3. Drone CI

### All-in-One Platform
1. **GitLab CI** ✅ (Git + CI/CD + Registry + Wiki + Issues)
2. TeamCity (mais €€€)
3. Jenkins (mais morcelé)

### Performance & Légèreté
1. Drone CI ✅
2. **GitLab CI**
3. GitHub Actions

### Communauté & Support
1. **GitLab CI** ✅
2. Jenkins
3. GitHub Actions

## Décision Finale

### ✅ RECOMMANDATION : GitLab CI (Community Edition)

#### Justification
1. **Coût** : €0 pour toutes les features nécessaires
2. **All-in-One** : Git + CI/CD + Registry dans une plateforme
3. **Mature** : Production-ready, utilisé par des millions
4. **GitOps-ready** : Compatible ArgoCD pour Phase 2+
5. **Documentation** : Excellente et complète
6. **Communauté** : Énorme, beaucoup de ressources
7. **UI** : Moderne et intuitive
8. **Performance** : Bonne avec cache npm/Docker
9. **Maintenance** : Raisonnable (updates mensuelles)
10. **On-Premise** : Parfaitement adapté

#### Stack CI/CD Retenue
```
Phase 1 (POC):     GitLab CE + GitLab CI
Phase 2 (MVP):     GitLab CE + GitLab CI + ArgoCD
Phase 3 (PROD):    GitLab CE + GitLab CI + ArgoCD + Vault
```

#### Alternatives Acceptables
- **Drone CI** : Si besoin légèreté extrême (POC/MVP uniquement)
- **Jenkins** : Si compétences Jenkins existantes (mais pas recommandé from scratch)

#### À Éviter
- ❌ CircleCI Server (trop cher)
- ❌ TeamCity (trop cher)
- ❌ GitHub Actions (dépendance cloud pour on-premise strict)

## Setup GitLab CI - Quick Start

### Installation GitLab CE (Docker Compose)

```yaml
version: '3.9'

services:
  gitlab:
    image: gitlab/gitlab-ce:latest
    container_name: gitlab
    hostname: gitlab.example.com
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'https://gitlab.example.com'
        gitlab_rails['gitlab_shell_ssh_port'] = 2222
    ports:
      - '80:80'
      - '443:443'
      - '2222:22'
    volumes:
      - gitlab_config:/etc/gitlab
      - gitlab_logs:/var/log/gitlab
      - gitlab_data:/var/opt/gitlab
    shm_size: '256m'

  gitlab-runner:
    image: gitlab/gitlab-runner:latest
    container_name: gitlab-runner
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - gitlab_runner_config:/etc/gitlab-runner
    depends_on:
      - gitlab

volumes:
  gitlab_config:
  gitlab_logs:
  gitlab_data:
  gitlab_runner_config:
```

### Enregistrement Runner

```bash
# Obtenir le token depuis GitLab UI
# Settings > CI/CD > Runners > New project runner

# Enregistrer le runner
docker exec -it gitlab-runner gitlab-runner register \
  --url https://gitlab.example.com \
  --registration-token $REGISTRATION_TOKEN \
  --executor docker \
  --docker-image docker:24-dind \
  --docker-privileged
```

### Premier Pipeline

```yaml
# .gitlab-ci.yml
stages:
  - build
  - test

build:
  stage: build
  image: node:20-alpine
  script:
    - npm ci
    - npm run build
  cache:
    key: ${CI_COMMIT_REF_SLUG}
    paths:
      - node_modules/

test:
  stage: test
  image: node:20-alpine
  script:
    - npm ci
    - npm run test
```

---

**Document Version** : 1.0  
**Dernière Mise à Jour** : 2026-02-12  
**Auteur** : Équipe DevOps
