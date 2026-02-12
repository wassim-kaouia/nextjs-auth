# 💰 Cost Estimation - DevOps Microservices On-Premise

## Vue d'ensemble

Estimation détaillée des coûts sur 3 ans pour un déploiement on-premise 100% avec stack open-source.

## 📊 Résumé Exécutif

| Phase | Durée | CAPEX | OPEX (An 1) | Total An 1 |
|-------|-------|-------|-------------|------------|
| **Phase 1 - POC** | 2-4 sem | €0-500 | €0 | €500 |
| **Phase 2 - MVP** | 8-12 sem | €1,500-2,500 | €500 | €3,000 |
| **Phase 3 - Production** | 12-16 sem | €4,000-6,000 | €3,000 | €9,000 |
| **TOTAL Année 1** | 22-32 sem | **€5,500-9,000** | **€3,500** | **€12,500** |
| **Année 2** | - | €500 | €3,500 | €4,000 |
| **Année 3** | - | €500 | €3,500 | €4,000 |
| **TOTAL 3 ans** | - | **€6,500-10,000** | **€10,500** | **€20,500** |

### Comparaison Cloud vs On-Premise (3 ans)

| Scénario | An 1 | An 2 | An 3 | Total 3 ans |
|----------|------|------|------|-------------|
| **On-Premise (proposé)** | €12,500 | €4,000 | €4,000 | **€20,500** |
| **AWS (équivalent)** | €24,000 | €30,000 | €36,000 | **€90,000** |
| **Azure (équivalent)** | €22,000 | €28,000 | €34,000 | **€84,000** |
| **GCP (équivalent)** | €20,000 | €26,000 | €32,000 | **€78,000** |
| **Économie vs Cloud** | €10,000 | €24,000 | €30,000 | **€60,000+** |

**ROI Break-even** : 8-10 mois

---

## 💻 CAPEX (Capital Expenditure)

### Phase 1 - POC (Semaines 1-4)

| Item | Quantité | Prix Unitaire | Total | Justification |
|------|----------|---------------|-------|---------------|
| **Serveur Dev** | 0 | €0 | **€0** | Serveur existant réutilisé |
| **Disques SSD additionnels** | 0 | - | **€0** | Disques existants suffisants |
| **Formation Docker/CI** | 24h | €0-200 | **€0-200** | Udemy en promo + doc gratuite |
| **Licences logicielles** | - | €0 | **€0** | 100% open-source |
| **Divers & imprévus** | - | - | **€200-300** | Câbles, adaptateurs, etc. |
| **TOTAL Phase 1** | | | **€200-500** | |

**Notes Phase 1** :
- Utilisation maximale de l'existant
- Aucun investissement hardware requis
- Formation gratuite via ressources en ligne
- GitLab CE, Prometheus, Grafana : gratuits

---

### Phase 2 - MVP (Semaines 5-16)

| Item | Quantité | Prix Unitaire | Total | Justification |
|------|----------|---------------|-------|---------------|
| **Serveurs Staging** | | | | |
| - Dell PowerEdge R240 (reconditionné) | 2 | €600 | **€1,200** | 8 cores, 32GB RAM, 2x500GB SSD |
| - ou équivalent HP ProLiant | 2 | €550 | **€1,100** | Alternative moins chère |
| **Storage NAS** | 1 | €400 | **€400** | Synology DS220+ 8TB (RAID1) |
| **Switch Gigabit** | 1 | €150 | **€150** | 24 ports manageable |
| **Câbles réseau** | 10 | €5 | **€50** | Cat6 pour inter-connexion |
| **Formation Kubernetes** | 24h | €0-500 | **€200** | CKAD prep course |
| **Divers & imprévus** | - | - | **€300** | UPS, etc. |
| **TOTAL Phase 2** | | | **€1,500-2,400** | |

**Notes Phase 2** :
- Serveurs reconditionnés = 60% économie vs neuf
- Storage NAS partagé pour backups
- Formation CKA/CKAD gratuite (docs officielles)

**Alternatives Budget Serré** :
- Mini-PCs Intel NUC : 3x €400 = €1,200 (8 cores, 32GB RAM chacun)
- Raspberry Pi Cluster : 4x €120 = €480 (mais performances limitées)

---

### Phase 3 - Production (Semaines 17-32)

| Item | Quantité | Prix Unitaire | Total | Justification |
|------|----------|---------------|-------|---------------|
| **Serveurs Production** | | | | |
| - Dell PowerEdge R340 (reconditionné) | 3 | €900 | **€2,700** | 12 cores, 64GB RAM, 2x1TB SSD RAID1 |
| - ou Dell R440 (plus puissant) | 3 | €1,200 | **€3,600** | 16 cores, 128GB RAM |
| **Storage Production** | | | | |
| - NAS Synology (2-bay) | 1 | €500 | **€500** | 16TB RAID1 pour backups |
| - ou Storage Server dédié | 1 | €1,000 | **€1,000** | FreeNAS avec 24TB |
| **Load Balancer Hardware** (optionnel) | 1 | €300 | **€0-300** | HAProxy sur VM suffit |
| **UPS** | 2 | €200 | **€400** | Protection coupures électriques |
| **Switch Core 10Gb** | 1 | €400 | **€400** | Pour inter-node K3s |
| **Câblage & Rack** | - | - | €200 | Rack 19", PDU, câbles |
| **Formation avancée** | - | - | **€500** | CKA certification, Security |
| **Divers & imprévus** | - | - | **€500** | |
| **TOTAL Phase 3** | | | **€4,000-6,000** | |

**Notes Phase 3** :
- 3 serveurs pour HA (K3s cluster)
- Production-grade hardware mais reconditionné
- UPS pour continuité électrique
- 10Gb networking optionnel mais recommandé

**Upgrade Path Année 2-3** :
- Ajout RAM si besoin : €200-400/serveur
- Remplacement disques : €200-300/serveur
- Ajout node K3s : €900-1,200

---

## 💸 OPEX (Operating Expenditure)

### Année 1 (Post Go-Live)

| Catégorie | Mensuel | Annuel | Justification |
|-----------|---------|--------|---------------|
| **Infrastructure** | | | |
| Électricité (4 serveurs @ 200W) | €60 | **€720** | 4 × 200W × 24h × €0.20/kWh × 30j |
| Climatisation datacenter | €40 | **€480** | Refroidissement serveurs |
| **Internet/Connectivité** | | | |
| Bande passante (1Gbps) | €50 | **€600** | Déjà disponible, coût marginal |
| VPN/Sécurité | €0 | **€0** | WireGuard open-source |
| **Monitoring & Alerting** | | | |
| PagerDuty | €0 | **€0** | Free tier (5 users) |
| Slack | €0 | **€0** | Free tier suffisant |
| **Backup & Storage** | | | |
| Offsite backup (cloud S3) | €20 | **€240** | Wasabi 2TB @ €5.99/TB |
| Bandes de sauvegarde | €10 | **€120** | Backup mensuel sur bande |
| **Maintenance & Support** | | | |
| Pièces détachées | €50 | **€600** | Disques, RAM de remplacement |
| Support GitLab CE | €0 | **€0** | Community support |
| **Certifications & Formation** | | | |
| CKA/CKAD certifications | - | **€600** | 2 × €300 |
| Conferences/Training | - | **€400** | KubeCon online, courses |
| **TOTAL OPEX Année 1** | **€230** | **€3,760** | |

---

### Année 2

| Catégorie | Annuel | Notes |
|-----------|--------|-------|
| Infrastructure | €1,200 | Électricité + climatisation |
| Internet | €600 | Stable |
| Monitoring | €0 | Free tiers maintenus |
| Backup | €360 | Storage augmente (3TB) |
| Maintenance | €800 | Remplacement disques préventif |
| Formation | €600 | Formation continue équipe |
| **TOTAL Année 2** | **€3,560** | |

### Année 3

| Catégorie | Annuel | Notes |
|-----------|--------|-------|
| Infrastructure | €1,200 | Stable |
| Internet | €600 | Stable |
| Monitoring | €240 | Upgrade PagerDuty si besoin |
| Backup | €480 | Storage 4TB |
| Maintenance | €800 | Remplacement RAM préventif |
| Formation | €400 | Optimisation/tuning |
| **TOTAL Année 3** | **€3,720** | |

---

## 📊 Détail par Composant

### Hardware (Serveurs)

#### Option 1 : Serveurs Reconditionnés (RECOMMANDÉ)
**Phase 2 (Staging)** :
- 2× Dell R240 reconditionné @ €600 = **€1,200**
  - CPU: Intel Xeon E-2224 (4c/4t @ 3.4GHz)
  - RAM: 32GB DDR4 ECC
  - Storage: 2× 500GB SSD RAID1
  - Garantie: 1 an
  
**Phase 3 (Production)** :
- 3× Dell R340 reconditionné @ €900 = **€2,700**
  - CPU: Intel Xeon E-2288G (8c/16t @ 3.7GHz)
  - RAM: 64GB DDR4 ECC
  - Storage: 2× 1TB NVMe RAID1
  - Garantie: 1 an

**Total Hardware** : €3,900

#### Option 2 : Mini-PCs Haute Performance
**Phase 2+3** :
- 5× Intel NUC 11 Pro @ €800 = **€4,000**
  - CPU: Intel Core i7-1165G7 (4c/8t @ 2.8GHz)
  - RAM: 64GB DDR4
  - Storage: 1TB NVMe
  - Consommation: 65W (vs 200W serveurs)
  
**Avantages** :
- ✅ Consommation électrique réduite (50% économie)
- ✅ Silencieux
- ✅ Faible encombrement
  
**Inconvénients** :
- ❌ Pas de redondance PSU
- ❌ Moins de slots d'extension
- ❌ Support hardware limité

#### Option 3 : Neuf (Budget Confortable)
**Phase 3** :
- 3× Dell R340 neuf @ €2,500 = **€7,500**
  - Garantie: 3 ans ProSupport
  - Configuration sur-mesure

---

### Software & Licenses

| Logiciel | Edition | Coût | Alternative Payante |
|----------|---------|------|---------------------|
| **GitLab** | CE | **€0** | Ultimate: €99/user/mois |
| **Kubernetes** | K3s | **€0** | OpenShift: €50-100/core/an |
| **ArgoCD** | Open-source | **€0** | - |
| **Harbor** | Open-source | **€0** | - |
| **Vault** | Open-source | **€0** | Enterprise: €1,500+/an |
| **Prometheus** | Open-source | **€0** | Datadog: €15-23/host/mois |
| **Grafana** | OSS | **€0** | Cloud: €49-299/mois |
| **Loki** | Open-source | **€0** | - |
| **Tempo** | Open-source | **€0** | - |
| **Linkerd** | Open-source | **€0** | Buoyant Cloud: €50/cluster/mois |
| **PostgreSQL** | Open-source | **€0** | AWS RDS: €100-500/mois |
| **Redis** | Open-source | **€0** | Redis Enterprise: €500+/mois |
| **MongoDB** | Community | **€0** | Atlas: €57-708/mois |
| **Nginx** | Open-source | **€0** | Plus: €2,500/an |
| **Linux** | Ubuntu Server | **€0** | RHEL: €349-1,299/an |
| **TOTAL** | | **€0/an** | **Cloud équivalent: €30,000+/an** |

**Économie Logicielle** : **€30,000+/an** grâce à l'open-source

---

### Ressources Humaines

| Rôle | FTE | Coût Interne | Externe (consultant) |
|------|-----|--------------|----------------------|
| **Tech Lead DevOps** | 1.0 | Interne | €600-800/jour |
| **DevOps Engineer** | 2.0 | Interne | €500-600/jour |
| **Développeurs** | 3.0 | Interne | €400-500/jour |
| **Security Specialist** | 0.25 | Interne/Externe | €700-900/jour |

**Notes** :
- Coûts RH exclus (déjà budgétés dans l'équipe)
- Consultants externes uniquement si manque compétences critiques
- Formation interne privilégiée (< €2,000/an vs €50,000+ consultant)

---

### Formation & Certifications

| Formation | Durée | Coût | Public |
|-----------|-------|------|--------|
| **Docker Fundamentals** | 16h | €0 | Tous (Udemy €20 en promo) |
| **Kubernetes (CKAD)** | 24h | €395 | DevOps (exam inclus) |
| **GitLab CI/CD** | 8h | €0 | Tous (doc officielle) |
| **Security Best Practices** | 16h | €0-200 | Tous (OWASP, SANS gratuit) |
| **Observability (Grafana)** | 8h | €0 | DevOps (doc + labs) |
| **ArgoCD GitOps** | 8h | €0 | DevOps (doc + demos) |
| **TOTAL An 1** | 80h | **€795-1,000** | |
| **TOTAL An 2-3** | 40h/an | **€400-600/an** | Formation continue |

**Formation interne** (knowledge sharing) :
- Brown bag lunches : 1h/semaine
- Tech talks : 1x/mois
- Pair programming : quotidien

---

## 💡 Optimisations Coûts

### Phase 1 - Réduction Maximale
1. ✅ **Serveurs existants** : Réutiliser hardware disponible
2. ✅ **Formation gratuite** : Udemy promos, docs officielles, YouTube
3. ✅ **Open-source only** : 0 licence payante
4. ✅ **Cloud dev** : GitLab.com gratuit pour POC (migrer après)

**Économie Phase 1** : **€2,000+** (vs setup from scratch)

### Phase 2 - Hardware Reconditionné
1. ✅ **Serveurs refurb** : 60% économie vs neuf (€600 vs €1,500)
2. ✅ **NAS consumer-grade** : Synology vs SAN enterprise (€400 vs €5,000)
3. ✅ **Switch générique** : TP-Link/Netgear vs Cisco (€150 vs €2,000)

**Économie Phase 2** : **€8,000+** (vs hardware neuf enterprise)

### Phase 3 - Éviter Pitfalls
1. ✅ **Pas de cloud hybrid** : Éviter coûts double (on-prem + cloud)
2. ✅ **Pas de commercial software** : Resist upsell vers éditions payantes
3. ✅ **Auto-scaling prudent** : Provisionner pour charge réelle, pas pic théorique
4. ✅ **Monitoring costs** : PagerDuty free tier (vs €29/user/mois)

**Économie Phase 3** : **€15,000+/an** (vs éditions commerciales)

---

## 📈 Projection 5 Ans

| Année | CAPEX | OPEX | Total | Cumulé |
|-------|-------|------|-------|--------|
| **Année 1** | €9,000 | €3,760 | €12,760 | €12,760 |
| **Année 2** | €500 | €3,560 | €4,060 | €16,820 |
| **Année 3** | €500 | €3,720 | €4,220 | €21,040 |
| **Année 4** | €2,000 | €3,900 | €5,900 | €26,940 |
| **Année 5** | €500 | €4,100 | €4,600 | €31,540 |

### Investissements Année 4
- Refresh hardware (serveurs 3 ans) : €1,500
- Upgrade storage : €500

### Vs Cloud (5 ans)
- **On-Premise** : €31,540
- **AWS** : €150,000+ (croissance 20%/an)
- **Économie** : **€118,000+** sur 5 ans

---

## 🎯 Recommandations Budgétaires

### Budget Minimum (Serré)
- **Phase 1** : €200 (existant + formation gratuite)
- **Phase 2** : €1,500 (serveurs refurb basiques)
- **Phase 3** : €4,000 (3 serveurs + storage minimum)
- **OPEX An 1** : €2,500 (minimal viable)
- **TOTAL An 1** : **€8,200**

### Budget Recommandé (Optimal)
- **Phase 1** : €500 (confort + formation)
- **Phase 2** : €2,400 (hardware fiable + NAS)
- **Phase 3** : €6,000 (hardware performant + redondance)
- **OPEX An 1** : €3,760 (complet)
- **TOTAL An 1** : **€12,660**

### Budget Confort (Si disponible)
- **Phase 1** : €800
- **Phase 2** : €4,000 (hardware neuf)
- **Phase 3** : €9,000 (hardware neuf + extras)
- **OPEX An 1** : €5,000 (support + formations premium)
- **TOTAL An 1** : **€18,800**

---

## ✅ Critères de Décision

| Critère | Poids | Score Cloud | Score On-Prem |
|---------|-------|-------------|---------------|
| **Coût 3 ans** | 30% | 3/10 | **9/10** |
| **Contrôle** | 20% | 5/10 | **10/10** |
| **Performance** | 15% | 8/10 | **8/10** |
| **Sécurité/Privacy** | 15% | 6/10 | **10/10** |
| **Scalabilité** | 10% | 10/10 | **7/10** |
| **Maintenance** | 10% | 9/10 | **6/10** |
| **TOTAL** | 100% | **6.3/10** | **8.7/10** |

**Décision** : ✅ **On-Premise recommandé** pour ce cas d'usage

---

**Document Version** : 1.0  
**Dernière Mise à Jour** : 2026-02-12  
**Prochaine Revue** : Trimestrielle  
**Auteur** : Équipe Finance & DevOps
