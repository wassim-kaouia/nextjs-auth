# 🔒 Security Checklist - Production Readiness

## Vue d'ensemble

Checklist complète de sécurité pour validation avant mise en production. Basée sur les best practices OWASP, CIS Benchmarks et NIST.

## ✅ Infrastructure & Network Security

### Firewall & Network Segmentation
- [ ] **Firewall configuré** : Pare-feu réseau actif avec règles strictes
- [ ] **Zones isolées** : DMZ, Application, Data, Management séparées
- [ ] **Default deny** : Politique par défaut "tout bloquer"
- [ ] **Port minimaux** : Seuls les ports nécessaires ouverts
- [ ] **VPN/Bastion** : Accès admin uniquement via VPN ou bastion host
- [ ] **IDS/IPS** : Système de détection d'intrusion actif (Snort/Suricata)

### Kubernetes Network Policies
- [ ] **Default deny ingress** : Politique par défaut pour tous les namespaces
- [ ] **Isolation pods** : Network policies entre services
- [ ] **DNS policy** : Restriction accès DNS externe
- [ ] **Egress control** : Contrôle trafic sortant des pods
- [ ] **Service mesh** : mTLS automatique (Linkerd) entre services

### TLS/SSL Configuration
- [ ] **TLS 1.3 minimum** : Protocoles obsolètes désactivés (TLS 1.0, 1.1, SSL)
- [ ] **Strong ciphers** : Ciphers faibles désactivés
- [ ] **Certificats valides** : Certificats signés (Let's Encrypt ou CA interne)
- [ ] **HSTS enabled** : HTTP Strict Transport Security activé
- [ ] **Certificate rotation** : Renouvellement automatique < 30 jours avant expiration
- [ ] **mTLS entre services** : Communication chiffrée et authentifiée

## ✅ Authentication & Authorization

### Identity & Access Management
- [ ] **SSO/OIDC** : Single Sign-On implémenté
- [ ] **MFA enabled** : Multi-factor authentication pour admins
- [ ] **Password policy** : Complexité minimale 12 caractères, rotation 90 jours
- [ ] **Account lockout** : Verrouillage après 5 tentatives échouées
- [ ] **Session timeout** : Expiration après 30 min inactivité
- [ ] **JWT signed** : Tokens signés avec algorithme RS256 minimum

### Kubernetes RBAC
- [ ] **Least privilege** : Principe du moindre privilège appliqué
- [ ] **ServiceAccounts** : Comptes de service dédiés par application
- [ ] **No default SA** : Service account par défaut non utilisé
- [ ] **ClusterRole limité** : Pas de cluster-admin sauf nécessaire
- [ ] **RoleBinding** : Permissions au niveau namespace uniquement
- [ ] **Audit logs** : Logs d'accès K8s activés

### Secret Management
- [ ] **Vault deployed** : HashiCorp Vault opérationnel
- [ ] **No secrets in code** : 0 secrets hardcodés dans le code
- [ ] **No secrets in env** : Secrets injectés dynamiquement, pas en variables d'env
- [ ] **Encryption at rest** : Secrets chiffrés au repos (Vault)
- [ ] **Auto-rotation** : Rotation automatique tous les 90 jours
- [ ] **Audit trail** : Logs d'accès aux secrets

## ✅ Container & Image Security

### Image Scanning
- [ ] **Trivy integration** : Scan automatique dans CI/CD
- [ ] **0 Critical CVE** : Aucune vulnérabilité critique tolérée en production
- [ ] **Base images** : Images officielles et minimales (alpine, distroless)
- [ ] **Image signing** : Signatures Cosign/Notary pour validation
- [ ] **Registry security** : Harbor avec RBAC et scan activés
- [ ] **Vulnerability DB** : Base de données CVE à jour quotidiennement

### Container Runtime Security
- [ ] **Non-root user** : Conteneurs exécutés en tant que non-root
- [ ] **Read-only filesystem** : FS en lecture seule quand possible
- [ ] **No privileged** : Pas de conteneurs privileged
- [ ] **Capabilities drop** : Capabilities Linux minimales
- [ ] **Seccomp profile** : Profil seccomp appliqué
- [ ] **AppArmor/SELinux** : LSM activé et configuré

### Pod Security Standards
```yaml
# Enforce restricted Pod Security Standard
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: restricted
spec:
  privileged: false
  allowPrivilegeEscalation: false
  requiredDropCapabilities:
    - ALL
  volumes:
    - 'configMap'
    - 'emptyDir'
    - 'projected'
    - 'secret'
    - 'downwardAPI'
    - 'persistentVolumeClaim'
  hostNetwork: false
  hostIPC: false
  hostPID: false
  runAsUser:
    rule: 'MustRunAsNonRoot'
  seLinux:
    rule: 'RunAsAny'
  fsGroup:
    rule: 'RunAsAny'
  readOnlyRootFilesystem: true
```

- [ ] **PSP enabled** : PodSecurityPolicy enforced
- [ ] **No host network** : hostNetwork: false
- [ ] **No host PID** : hostPID: false
- [ ] **Resource limits** : CPU/Memory limits définis
- [ ] **Liveness probes** : Health checks configurés

## ✅ Application Security

### Code Security
- [ ] **SAST** : Static Analysis Security Testing dans CI (SonarQube, Semgrep)
- [ ] **Dependency scan** : npm audit, pip-audit, OWASP Dependency-Check
- [ ] **Code review** : Revue de code obligatoire (2 reviewers minimum)
- [ ] **Security linting** : ESLint security plugins, Bandit (Python)
- [ ] **No secrets committed** : Git hooks pour bloquer secrets
- [ ] **Branch protection** : Main/Production branches protégées

### OWASP Top 10 Mitigation

#### A01:2021 - Broken Access Control
- [ ] **Authorization checks** : Vérification permissions sur tous endpoints
- [ ] **Object-level auth** : Vérification propriété ressources
- [ ] **CORS configuration** : CORS restreint aux origines autorisées

#### A02:2021 - Cryptographic Failures
- [ ] **Data encryption** : Données sensibles chiffrées au repos et en transit
- [ ] **Strong algorithms** : AES-256, RSA-2048+
- [ ] **No weak crypto** : MD5, SHA1 désactivés

#### A03:2021 - Injection
- [ ] **Prepared statements** : Requêtes SQL paramétrées (no string concat)
- [ ] **ORM usage** : Utilisation ORM sécurisé
- [ ] **Input validation** : Validation et sanitization de tous les inputs
- [ ] **Output encoding** : Encodage HTML/JS des outputs

#### A04:2021 - Insecure Design
- [ ] **Threat modeling** : Modélisation des menaces effectuée
- [ ] **Security requirements** : Exigences de sécurité documentées
- [ ] **Rate limiting** : Limitation du taux de requêtes

#### A05:2021 - Security Misconfiguration
- [ ] **Default credentials** : Tous les mots de passe par défaut changés
- [ ] **Error messages** : Pas de stack traces en production
- [ ] **Unused features** : Features inutilisées désactivées
- [ ] **Security headers** : Headers HTTP sécurisés configurés

```javascript
// Express security headers
const helmet = require('helmet');
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      scriptSrc: ["'self'"],
      imgSrc: ["'self'", "data:", "https:"],
    }
  },
  hsts: {
    maxAge: 31536000,
    includeSubDomains: true,
    preload: true
  },
  frameguard: { action: 'deny' },
  xssFilter: true,
  noSniff: true,
  referrerPolicy: { policy: 'no-referrer' }
}));
```

#### A06:2021 - Vulnerable Components
- [ ] **Dependency updates** : Dépendances à jour (Dependabot)
- [ ] **Version pinning** : Versions exactes spécifiées
- [ ] **License compliance** : Vérification licences open-source

#### A07:2021 - Authentication Failures
- [ ] **Strong passwords** : Complexité minimale enforced
- [ ] **Brute force protection** : Rate limiting login
- [ ] **Session management** : Tokens JWT sécurisés
- [ ] **Logout** : Invalidation propre des sessions

#### A08:2021 - Software & Data Integrity
- [ ] **CI/CD security** : Pipelines sécurisés et auditables
- [ ] **Image signing** : Images Docker signées
- [ ] **Checksum verification** : Vérification intégrité artefacts

#### A09:2021 - Logging Failures
- [ ] **Security logs** : Événements de sécurité loggés
- [ ] **Log protection** : Logs non modifiables
- [ ] **Log monitoring** : Alertes sur événements suspicieux
- [ ] **No sensitive data** : Pas de données sensibles dans logs

#### A10:2021 - Server-Side Request Forgery
- [ ] **URL validation** : Validation URLs externes
- [ ] **Whitelist** : Liste blanche domaines autorisés
- [ ] **Network segmentation** : Services isolés

## ✅ Data Security

### Database Security
- [ ] **Encryption at rest** : Données chiffrées sur disque (LUKS, TDE)
- [ ] **Encryption in transit** : SSL/TLS pour connexions DB
- [ ] **Strong passwords** : Mots de passe complexes (16+ caractères)
- [ ] **Principle of least privilege** : Permissions DB minimales par service
- [ ] **Backup encryption** : Backups chiffrés
- [ ] **Audit logging** : Logs d'accès base de données
- [ ] **No default accounts** : Comptes par défaut désactivés

### PostgreSQL Hardening
```sql
-- Disable unnecessary extensions
DROP EXTENSION IF EXISTS plpythonu;

-- Enforce SSL connections
ALTER SYSTEM SET ssl = on;
ALTER SYSTEM SET ssl_cert_file = '/path/to/cert.pem';
ALTER SYSTEM SET ssl_key_file = '/path/to/key.pem';
ALTER SYSTEM SET ssl_min_protocol_version = 'TLSv1.3';

-- Strong password policy
ALTER ROLE authuser WITH PASSWORD 'strong_password_here' VALID UNTIL 'infinity';
ALTER ROLE authuser CONNECTION LIMIT 20;

-- Audit logging
ALTER SYSTEM SET logging_collector = on;
ALTER SYSTEM SET log_connections = on;
ALTER SYSTEM SET log_disconnections = on;
ALTER SYSTEM SET log_statement = 'ddl';
ALTER SYSTEM SET log_line_prefix = '%t [%p]: user=%u,db=%d,app=%a,client=%h ';
```

- [ ] **SSL enforced** : Connexions SSL obligatoires
- [ ] **Row-level security** : RLS activé quand applicable
- [ ] **Connection limits** : Limite de connexions par role
- [ ] **pg_audit extension** : Audit avancé installé

### Sensitive Data Handling
- [ ] **PII identification** : Données personnelles identifiées et mappées
- [ ] **Data classification** : Classification (Public, Internal, Confidential, Restricted)
- [ ] **Encryption** : PII chiffré au repos
- [ ] **Access logging** : Accès aux données sensibles loggé
- [ ] **Data retention** : Politique de rétention appliquée
- [ ] **GDPR compliance** : Conformité RGPD si applicable

## ✅ Monitoring & Alerting

### Security Monitoring
- [ ] **Security dashboard** : Dashboard Grafana dédié sécurité
- [ ] **Failed login alerts** : Alerte sur tentatives échouées répétées
- [ ] **Privilege escalation** : Alerte sur sudo, kubectl exec
- [ ] **Config changes** : Alerte sur modifications config critique
- [ ] **CVE alerts** : Notification nouvelles CVE
- [ ] **Unusual activity** : Détection comportements anormaux

### Alerting Rules
```yaml
groups:
  - name: security_alerts
    rules:
      # Failed login attempts
      - alert: HighFailedLoginRate
        expr: |
          rate(auth_attempts_total{status="failure"}[5m]) > 10
        for: 5m
        labels:
          severity: warning
          category: security
        annotations:
          summary: "High rate of failed login attempts"
          
      # Privilege escalation
      - alert: PrivilegeEscalation
        expr: |
          increase(kubernetes_audit_event_total{verb="create",
            objectRef_resource="pods/exec"}[5m]) > 5
        labels:
          severity: critical
          category: security
        annotations:
          summary: "Potential privilege escalation detected"
          
      # Critical CVE detected
      - alert: CriticalCVEDetected
        expr: |
          harbor_scanner_vulnerabilities{severity="critical"} > 0
        labels:
          severity: critical
          category: security
        annotations:
          summary: "Critical CVE detected in image"
```

## ✅ Incident Response

### Incident Response Plan
- [ ] **IR plan documented** : Plan de réponse aux incidents documenté
- [ ] **IR team** : Équipe désignée et formée
- [ ] **Escalation path** : Chaîne d'escalade définie
- [ ] **Contact list** : Liste de contacts à jour
- [ ] **Communication plan** : Plan de communication interne/externe
- [ ] **DR tested** : Plan testé au moins 1x/an

### Forensics Readiness
- [ ] **Log retention** : Logs conservés 90 jours minimum
- [ ] **Log immutability** : Logs non modifiables (WORM)
- [ ] **Audit trail** : Audit complet des actions
- [ ] **Backup logs** : Logs sauvegardés hors ligne

## ✅ Compliance & Governance

### Documentation
- [ ] **Security policy** : Politique de sécurité documentée
- [ ] **Architecture diagram** : Diagrammes à jour
- [ ] **Data flow diagram** : Flux de données documenté
- [ ] **Risk register** : Registre des risques maintenu
- [ ] **Runbooks** : Procédures d'incident documentées

### Audit & Compliance
- [ ] **Regular audits** : Audits sécurité trimestriels
- [ ] **Penetration testing** : Pentest annuel
- [ ] **Vulnerability scanning** : Scan automatique hebdomadaire
- [ ] **Compliance check** : Vérification conformité mensuelle
- [ ] **Security training** : Formation équipe annuelle

### Standards Compliance
- [ ] **ISO 27001** : Conformité ou roadmap vers conformité
- [ ] **OWASP ASVS** : Application Security Verification Standard appliqué
- [ ] **CIS Benchmarks** : Benchmarks CIS Kubernetes appliqués
- [ ] **NIST Framework** : Framework NIST suivi

## ✅ Backup & Disaster Recovery

### Backup Security
- [ ] **Encrypted backups** : Backups chiffrés (AES-256)
- [ ] **Offsite storage** : Backups stockés hors site
- [ ] **Access control** : Accès backups restreint
- [ ] **Restore tested** : Tests de restore mensuels
- [ ] **Backup monitoring** : Alertes sur échecs backup
- [ ] **Immutable backups** : Backups protégés contre suppression

### Disaster Recovery
- [ ] **DR plan** : Plan de DR documenté
- [ ] **RTO defined** : Recovery Time Objective < 4h
- [ ] **RPO defined** : Recovery Point Objective < 1h
- [ ] **DR tested** : Tests DR semestriels
- [ ] **Failover tested** : Basculement testé

## ✅ Supply Chain Security

### CI/CD Security
- [ ] **Pipeline as Code** : Pipelines versionnés dans Git
- [ ] **Signed commits** : Commits signés GPG
- [ ] **Protected branches** : Branches protégées
- [ ] **Pipeline secrets** : Secrets isolés et rotationnés
- [ ] **Build reproducibility** : Builds reproductibles
- [ ] **SBOM generation** : Software Bill of Materials généré

### Third-Party Security
- [ ] **Vendor assessment** : Évaluation sécurité vendors
- [ ] **SLA security** : Clauses sécurité dans SLA
- [ ] **Dependency review** : Revue dépendances tierces
- [ ] **License compliance** : Conformité licences

## 📋 Pre-Production Security Checklist

### Critical (Must-Have)
- [ ] 0 Critical CVE en images de production
- [ ] Tous les secrets dans Vault (0 en clear)
- [ ] TLS 1.3 activé partout
- [ ] RBAC K8s configuré et testé
- [ ] Network Policies en place
- [ ] Backups testés avec succès
- [ ] Monitoring et alerting opérationnels
- [ ] Incident response plan documenté

### Important (Should-Have)
- [ ] mTLS entre services (Linkerd)
- [ ] WAF configuré
- [ ] IDS/IPS actif
- [ ] SAST dans CI/CD
- [ ] Pentest effectué
- [ ] Security training complété
- [ ] Audit logs configurés

### Nice-to-Have
- [ ] ISO 27001 roadmap
- [ ] Bug bounty program
- [ ] Red team exercise
- [ ] Security champions program

## 🎯 Score de Sécurité

### Calcul du Score
- **Critical items** : 8 points chacun (8 items × 8 = 64 points)
- **Important items** : 2 points chacun (7 items × 2 = 14 points)
- **Nice-to-Have items** : 0.5 points chacun (4 items × 0.5 = 2 points)
- **Total Maximum** : 80 points

### Niveaux
- **80+ points** : ✅ Production-Ready
- **65-79 points** : ⚠️ Acceptable avec plan de remédiation
- **< 65 points** : ❌ Non prêt pour production

## 📞 Contacts Sécurité

- **Security Lead** : [À définir]
- **Incident Response** : [À définir]
- **Security Email** : security@example.com
- **PagerDuty** : +33 X XX XX XX XX

---

**Document Version** : 1.0  
**Dernière Mise à Jour** : 2026-02-12  
**Prochaine Revue** : Avant chaque mise en production  
**Auteur** : Équipe Security
