# Customer Clinic Deployment Guide

**Project:** Clinic EMR - Patient Module  
**Document Type:** Deployment Planning  
**Date:** 2026-11-18  
**Author:** ANSVK, Project Manager

---

## Executive Summary

This document outlines how to deploy and host the Clinic EMR application at a customer's clinic after development is complete. It covers deployment models, infrastructure requirements, cost considerations, and operational responsibilities.

**Key Decision:** Choose between **on-premises hosting** (at clinic) vs **cloud hosting** (you manage remotely).

---

## Table of Contents

1. Deployment Models Overview
2. On-Premises Deployment (At Customer Clinic)
3. Cloud Deployment (Remote Hosting)
4. Hybrid Deployment
5. Cost Comparison
6. Security and Compliance
7. Maintenance and Support
8. Scalability Considerations
9. Decision Framework
10. Implementation Roadmap

---

## 1. Deployment Models Overview

### Model 1: On-Premises at Clinic (Customer's Hardware)
**You deploy software → Clinic provides hardware**

### Model 2: Cloud Hosting (Your Infrastructure)
**You manage everything → Clinic accesses via internet**

### Model 3: Hybrid (Mixed Approach)
**Database at clinic → Application in cloud**

---

## 2. On-Premises Deployment (At Customer Clinic)

### 2.1 What It Means

**You install the EMR software on a computer/server physically located at the customer's clinic.**

**Ownership:**
- Customer owns the hardware
- You provide the software
- They have physical access to everything

---

### 2.2 Hardware Requirements at Clinic

#### Minimum Server Specifications:

**Option A: Small Clinic (1-5 users)**
- **Computer:** Desktop PC or mini server
- **CPU:** Intel Core i5 or equivalent (4 cores)
- **RAM:** 8 GB minimum, 16 GB recommended
- **Storage:** 256 GB SSD minimum, 512 GB recommended
- **OS:** Ubuntu 22.04 LTS Server
- **Network:** Gigabit Ethernet
- **UPS:** Uninterruptible Power Supply (battery backup)
- **Cost:** $800-$1,500

**Option B: Medium Clinic (5-20 users)**
- **Server:** Entry-level business server
- **CPU:** Intel Xeon or AMD EPYC (8+ cores)
- **RAM:** 16-32 GB
- **Storage:** 512 GB SSD + 1 TB HDD (database + backups)
- **OS:** Ubuntu 22.04 LTS Server
- **Network:** Gigabit Ethernet, dual NICs
- **RAID:** RAID 1 for data redundancy
- **UPS:** 1500VA minimum
- **Cost:** $2,000-$4,000

**Option C: Large Clinic (20+ users)**
- **Server:** Business-class rack server
- **CPU:** Dual Xeon or EPYC (16+ cores)
- **RAM:** 64 GB+
- **Storage:** 1 TB NVMe SSD + RAID array
- **Network:** 10 Gigabit, redundant connections
- **Backup:** Dedicated backup server
- **UPS:** 3000VA with generator backup
- **Cost:** $5,000-$10,000+

---

### 2.3 Network Requirements

**Essential:**
- ✅ Stable internet connection (for updates, remote support)
- ✅ Local network (LAN) for clinic workstations
- ✅ Router/firewall
- ✅ Backup internet (mobile hotspot as failover)

**Recommended Speed:**
- Small clinic: 50 Mbps
- Medium clinic: 100 Mbps
- Large clinic: 500 Mbps+

---

### 2.4 Installation Process

**Steps to Deploy at Clinic:**

1. **Pre-Installation (Your Office)**
   - Test complete system on identical hardware
   - Prepare installation scripts
   - Create deployment documentation
   - Prepare backup procedures

2. **On-Site Installation (At Clinic)**
   - **Day 1:** Hardware setup and OS installation
     - Install Ubuntu Server
     - Configure network
     - Set up firewall
     - Install PostgreSQL
     - Install Go runtime
     - Deploy Clinic EMR application
     - Configure HTTPS/SSL
     - Set up automated backups
     - Configure monitoring

   - **Day 2:** Testing and training
     - Run smoke tests
     - Create initial user accounts
     - Import/create initial data
     - Train clinic staff
     - Document admin procedures
     - Verify backups working

3. **Post-Installation**
   - Remote monitoring setup
   - Support contact established
   - Maintenance schedule agreed

**Total Time:** 2-3 days on-site

---

### 2.5 Advantages of On-Premises

✅ **Data Control:** Customer has physical control of their data  
✅ **No Monthly Hosting Fees:** One-time hardware investment  
✅ **Works Without Internet:** Local network access still works  
✅ **Compliance:** Easier for some privacy regulations  
✅ **Performance:** No internet latency  
✅ **Customization:** Can customize for specific clinic needs  

---

### 2.6 Disadvantages of On-Premises

❌ **High Upfront Cost:** Customer buys hardware ($800-$10,000+)  
❌ **Maintenance Burden:** Clinic responsible for hardware failures  
❌ **Your Travel:** You need to visit clinic for major issues  
❌ **Backup Complexity:** Clinic must manage backups properly  
❌ **Power/Network Issues:** Clinic downtime = EMR downtime  
❌ **Security Responsibility:** Clinic must secure physical access  
❌ **Scaling:** Adding capacity requires hardware purchase  

---

### 2.7 Who Owns What?

| Responsibility | Customer (Clinic) | You (Developer) |
|----------------|-------------------|-----------------|
| Hardware purchase | ✅ Pays | Recommends specs |
| Hardware maintenance | ✅ Owns/fixes | Advises |
| Software license | ✅ Pays | Provides |
| Software updates | You deploy | ✅ Creates |
| Data backup | ✅ Executes | ✅ Configures |
| Internet/power | ✅ Provides | - |
| Security (physical) | ✅ Locks doors | - |
| Security (software) | You patch | ✅ Monitors |
| Technical support | You provide | ✅ Provides |

---

### 2.8 Your Support Model for On-Premises

**What You Provide:**

**Remote Support (90% of issues):**
- Phone/email/chat support
- Remote desktop access (with permission)
- Software updates and patches
- Bug fixes
- Performance monitoring
- Backup monitoring

**On-Site Support (10% of issues):**
- Initial installation
- Major hardware failures
- Significant upgrades
- Quarterly maintenance visits (optional)

**Support Pricing Models:**

**Option A: Included in License**
- Free support for X months after installation
- Then monthly support fee

**Option B: Pay-Per-Incident**
- Charge per support call or on-site visit

**Option C: Annual Support Contract**
- Fixed annual fee for unlimited support

---

### 2.9 Backup Strategy for On-Premises

**Required Backups:**

1. **On-Site Backup (Daily)**
   - Automated PostgreSQL dumps
   - Store on separate disk in server
   - Retain 30 days

2. **Off-Site Backup (Weekly)**
   - Copy backups to cloud storage
   - Encrypted uploads
   - Retain 90 days
   - **You should set this up for them**

3. **Disaster Recovery**
   - Monthly full system backup
   - Test restore quarterly
   - Document recovery procedures

**Your Responsibility:**
- Configure automated backup scripts
- Monitor backup success
- Test restores periodically
- Alert clinic if backups fail

---

## 3. Cloud Deployment (Remote Hosting)

### 3.1 What It Means

**You host the EMR application on servers you control (cloud provider or data center).**

**The clinic accesses via web browser over the internet.**

**Ownership:**
- You own/rent the infrastructure
- You manage everything
- Clinic just uses the application

---

### 3.2 Cloud Hosting Options

#### Option A: Cloud Provider (AWS, Google Cloud, Azure, DigitalOcean)

**Pros:**
- ✅ Scalable (start small, grow easily)
- ✅ Reliable (99.9%+ uptime)
- ✅ Professional infrastructure
- ✅ Built-in backups and security
- ✅ You control everything remotely

**Cons:**
- ❌ Monthly costs (forever)
- ❌ Requires internet at clinic
- ❌ Data privacy concerns (data leaves clinic)
- ❌ Compliance complexity (HIPAA, etc.)

**Cost:**
- Small: $50-150/month per clinic
- Medium: $150-500/month per clinic
- Large: $500-2000+/month per clinic

---

#### Option B: Dedicated Server / VPS

**Use Cases:**
- Single large clinic
- Multiple small clinics on one server
- Cost-effective for multiple customers

**Providers:**
- DigitalOcean Droplets
- Linode
- Vultr
- Hetzner

**Cost:**
- Basic: $25-50/month (1 small clinic)
- Standard: $100-200/month (multiple clinics)
- Premium: $500+/month (high performance)

---

#### Option C: Multi-Tenant SaaS Model

**What It Means:**
- ONE application instance
- MULTIPLE clinics use it
- Shared infrastructure
- Each clinic's data isolated in database

**Advantages:**
- ✅ Most cost-effective
- ✅ Easy to update (update once, all clinics get it)
- ✅ Economies of scale

**Challenges:**
- ⚠️ Complex data isolation (must ensure Clinic A can't see Clinic B data)
- ⚠️ Single point of failure
- ⚠️ Performance bottlenecks

**Cost Per Clinic:**
- $20-50/month (with many clinics)

---

### 3.3 Cloud Deployment Architecture

**Single-Tenant (One Clinic):**
```
Internet
    ↓
Firewall/Load Balancer
    ↓
Web Server (Clinic EMR)
    ↓
Database (PostgreSQL)
    ↓
Backup Storage
```

**Multi-Tenant (Multiple Clinics):**
```
Internet
    ↓
Load Balancer
    ↓
Web Servers (Clinic EMR) × N
    ↓
Database (PostgreSQL) - with clinic_id isolation
    ↓
Backup Storage
```

---

### 3.4 Advantages of Cloud Hosting

✅ **No Customer Hardware:** Clinic doesn't buy servers  
✅ **You Control Everything:** Remote management  
✅ **Easy Updates:** Deploy once, all clinics updated  
✅ **Professional Infrastructure:** 99.9%+ uptime  
✅ **Automatic Backups:** Built into cloud platform  
✅ **Scalability:** Add resources instantly  
✅ **Disaster Recovery:** Cloud providers handle it  
✅ **Remote Access:** Staff can access from anywhere  
✅ **Multi-Location:** Multiple clinic branches use same system  

---

### 3.5 Disadvantages of Cloud Hosting

❌ **Monthly Costs Forever:** Recurring expense  
❌ **Internet Dependency:** No internet = no access  
❌ **Data Privacy:** Customer data on your servers (trust issue)  
❌ **Compliance Complexity:** HIPAA Business Associate Agreement required  
❌ **Performance:** Internet latency affects responsiveness  
❌ **Vendor Lock-In:** Clinic depends on your service  
❌ **Your Responsibility:** Uptime, security, backups all on you  

---

### 3.6 Who Owns What?

| Responsibility | Customer (Clinic) | You (Developer) |
|----------------|-------------------|-----------------|
| Cloud hosting | Pays monthly | ✅ Provides |
| Infrastructure | - | ✅ Manages |
| Software license | Included | ✅ Provides |
| Software updates | Automatic | ✅ Deploys |
| Data backup | - | ✅ Executes |
| Internet at clinic | ✅ Provides | - |
| Security | - | ✅ Full responsibility |
| Technical support | Included | ✅ Provides |
| Uptime SLA | Expects 99.9% | ✅ Guarantees |

---

### 3.7 Pricing Models for Cloud Hosting

**Option A: Monthly Subscription Per User**
- $20-50 per user per month
- Example: 5 users = $100-250/month
- Scales with clinic size

**Option B: Flat Monthly Fee Per Clinic**
- $200-500/month per clinic (unlimited users)
- Predictable pricing
- Better for larger clinics

**Option C: Tiered Pricing**
- Small: $99/month (1-5 users)
- Medium: $249/month (6-20 users)
- Large: $499/month (21+ users)

**Typical Includes:**
- Hosting infrastructure
- Software updates
- Daily backups
- Email support
- 99.9% uptime SLA

---

### 3.8 Your Ongoing Responsibilities

**Daily:**
- Monitor uptime
- Check error logs
- Review performance metrics

**Weekly:**
- Verify backups completed
- Review security alerts
- Check disk space / resources

**Monthly:**
- Apply security patches
- Review and optimize performance
- Customer usage reports

**Quarterly:**
- Major feature updates
- Disaster recovery testing
- Security audits

---

## 4. Hybrid Deployment

### 4.1 What It Means

**Mix of on-premises and cloud:**
- Database at clinic (data stays local)
- Application in cloud (you manage)
- OR vice versa

**Use Cases:**
- Clinic wants data control but not infrastructure management
- Regulatory requirements for data location
- Performance + convenience balance

---

### 4.2 Common Hybrid Models

**Model A: Database On-Premises, App in Cloud**
- Clinic hosts PostgreSQL on their server
- You host application in cloud
- Application connects to clinic database via VPN

**Pros:**
- ✅ Data stays at clinic (control)
- ✅ You manage application (convenience)

**Cons:**
- ❌ Complex network setup (VPN required)
- ❌ Performance issues (latency)
- ❌ Two failure points

---

**Model B: App On-Premises, Backups in Cloud**
- Everything runs at clinic
- Automated cloud backups
- You monitor remotely

**Pros:**
- ✅ Performance (all local)
- ✅ Safe backups (off-site)
- ✅ Best of both worlds

**Cons:**
- ❌ Clinic still owns hardware
- ❌ Your on-site support needed

---

## 5. Cost Comparison (3-Year Total)

### Scenario: Medium Clinic (10 users, 5000 patients)

| Cost Item | On-Premises | Cloud Hosting |
|-----------|-------------|---------------|
| **Initial Setup** | | |
| Hardware | $3,000 | $0 |
| Installation | $2,000 (your time) | $500 (setup) |
| **Monthly Costs** | | |
| Hosting | $0 | $300/month |
| Internet | $80 | $80 |
| Power | $30 | $0 |
| **Annual Costs** | | |
| Support contract | $1,200 | Included |
| Software updates | Included | Included |
| Backup storage | $120 | Included |
| **3-Year Total** | | |
| Setup | $5,000 | $500 |
| Recurring (×36 months) | $5,100 | $13,680 |
| **TOTAL 3-YEAR** | **$10,100** | **$14,180** |

**Conclusion:** On-premises is 29% cheaper over 3 years, but requires upfront investment.

---

### Break-Even Analysis

**On-premises becomes cheaper after:** ~18 months

**If clinic uses system for:**
- 1 year: Cloud is cheaper (lower upfront)
- 3 years: On-premises is cheaper
- 5+ years: On-premises much cheaper

---

## 6. Security and Compliance

### 6.1 HIPAA Compliance (US Healthcare)

**Requirements:**
- ✅ Encryption (data at rest and in transit)
- ✅ Access controls
- ✅ Audit logging
- ✅ Business Associate Agreement (BAA)
- ✅ Physical security
- ✅ Backup and disaster recovery

**On-Premises:**
- Clinic is responsible for physical security
- You configure software security
- Easier compliance (data never leaves clinic)

**Cloud Hosting:**
- You must sign BAA with clinic
- Cloud provider must be HIPAA compliant
- More complex compliance documentation

---

### 6.2 Data Privacy Regulations

**GDPR (Europe):**
- Data must stay in EU (affects cloud provider choice)
- Right to deletion
- Data portability

**Local Regulations:**
- Some countries require health data stay in-country
- Affects cloud deployment options
- On-premises simplest for compliance

---

### 6.3 Security Measures

**Both Deployments Need:**
- ✅ HTTPS/TLS (encrypted connections)
- ✅ Strong passwords (enforce policy)
- ✅ Role-based access control
- ✅ Audit logging (all actions tracked)
- ✅ Regular backups (tested restores)
- ✅ Software updates (security patches)
- ✅ Firewall (block unauthorized access)

**On-Premises Additional:**
- Physical server security (locked room)
- Network security (firewall rules)
- UPS (power protection)

**Cloud Additional:**
- DDoS protection
- Intrusion detection
- Professional monitoring

---

## 7. Maintenance and Support

### 7.1 Update Strategy

**On-Premises:**
- You deploy updates remotely (SSH)
- OR on-site for major upgrades
- Schedule during clinic off-hours
- Test on staging server first

**Cloud:**
- You deploy instantly (zero downtime)
- All clinics updated simultaneously
- Roll back if issues detected

---

### 7.2 Monitoring

**What to Monitor:**
- Application uptime
- Database performance
- Disk space usage
- Backup success
- Error rates
- User activity (usage patterns)

**Tools:**
- On-premises: Nagios, Prometheus, custom scripts
- Cloud: Cloud provider monitoring + application metrics

---

### 7.3 Support Levels

**Basic Support (Included):**
- Email support (24-hour response)
- Bug fixes
- Security patches

**Standard Support ($X/month):**
- Phone support (business hours)
- 4-hour response time
- Monthly health checks

**Premium Support ($XX/month):**
- 24/7 phone support
- 1-hour response time
- Dedicated support engineer
- Quarterly on-site visits

---

## 8. Scalability Considerations

### 8.1 Growth Scenarios

**Scenario A: Clinic Grows (more users/patients)**

**On-Premises:**
- May need hardware upgrade
- Customer pays for new server
- Your time to migrate

**Cloud:**
- Scale server resources instantly
- Higher monthly fee
- No customer hardware investment

---

**Scenario B: Multiple Clinic Locations**

**On-Premises:**
- Separate server at each location
- OR VPN connection to main server
- Complex network management

**Cloud:**
- Single system, multi-location access
- Ideal for clinic chains
- Centralized management

---

**Scenario C: Multiple Customer Clinics (Your Business Growth)**

**On-Premises:**
- Each clinic = separate installation
- Your travel to each clinic
- Individual support contracts

**Cloud Multi-Tenant:**
- One system serves all clinics
- Update once, all benefit
- Economies of scale
- **This is SaaS model**

---

## 9. Decision Framework

### 9.1 Choose On-Premises If:

✅ Clinic has budget for upfront hardware  
✅ Clinic has IT staff or tech-savvy person  
✅ Clinic has reliable power/internet infrastructure  
✅ Clinic wants complete data control  
✅ Long-term use (3+ years)  
✅ Privacy regulations require local data  
✅ Only one clinic location  

---

### 9.2 Choose Cloud Hosting If:

✅ Clinic has limited upfront budget  
✅ Clinic has no IT staff  
✅ You want to control everything remotely  
✅ Multiple clinic locations  
✅ Clinic wants anytime/anywhere access  
✅ You plan to serve multiple clinics (SaaS)  
✅ You want easy updates and scaling  

---

### 9.3 Questions to Ask Customer

1. **Budget:** Upfront hardware cost OK? Or prefer monthly payments?
2. **IT Skills:** Do you have technical staff?
3. **Control:** Must data stay on your premises?
4. **Internet:** Reliable connection? Backup internet?
5. **Growth:** Planning to expand locations?
6. **Access:** Need remote access for staff?
7. **Regulations:** Any data residency requirements?

---

## 10. Implementation Roadmap

### 10.1 Phase 1: Development (Current)

**You're Here:**
- ✅ Application developed
- ✅ Testing complete (UAT)
- ⏳ Production readiness assessed

**Next:**
- Choose deployment model
- Prepare deployment documentation

---

### 10.2 Phase 2: Pilot Deployment (First Customer)

**Timeline:** 2-4 weeks

**Steps:**
1. Find pilot customer (friendly clinic)
2. Agree on deployment model
3. Prepare infrastructure
4. Deploy and test
5. Train users
6. Collect feedback
7. Document lessons learned

**Goal:** Prove the system works in real clinic

---

### 10.3 Phase 3: Production Deployment

**For Each New Clinic:**

**Week 1: Planning**
- Site survey (on-premises) or account setup (cloud)
- Hardware specification/procurement
- Network assessment
- Training plan

**Week 2: Installation**
- Deploy system
- Configure for clinic
- Import initial data
- Security hardening

**Week 3: Training & Go-Live**
- Staff training
- Parallel run (old + new system)
- Go-live
- On-site support

**Week 4: Post-Go-Live Support**
- Daily check-ins
- Issue resolution
- Performance optimization
- Final documentation

---

### 10.4 Phase 4: Ongoing Operations

**Your Business Model:**

**Revenue Streams:**
1. **One-Time:** Installation fees
2. **Recurring:** 
   - Support contracts (on-premises)
   - OR Monthly hosting (cloud)
3. **Additional:** Feature customizations

**Support Model:**
- Remote support (primary)
- On-site visits (as needed)
- Update deployment
- Monitoring and maintenance

---

## 11. Recommended Approach for Your Situation

### Starting Out (First 1-3 Clinics)

**Recommendation: On-Premises Deployment**

**Why:**
- ✅ Simpler for you (no hosting infrastructure to manage)
- ✅ Lower ongoing costs for clinic
- ✅ Easier data privacy compliance
- ✅ Learn deployment process
- ✅ Build reputation with successful installations

**Your Responsibilities:**
- Installation and setup
- Remote support
- Periodic on-site maintenance

**Clinic Responsibilities:**
- Provide hardware
- Maintain physical security
- Ensure power/internet

---

### Growth Phase (3-10 Clinics)

**Recommendation: Transition to Cloud Multi-Tenant**

**Why:**
- ✅ Easier to manage multiple clinics
- ✅ Update once, all benefit
- ✅ Economies of scale
- ✅ Professional infrastructure
- ✅ Better for your business growth

**Transition:**
- Keep existing on-premises customers
- Offer cloud option to new customers
- Gradually migrate existing customers (optional)

---

### Scale Phase (10+ Clinics)

**Recommendation: Full SaaS Model**

**Why:**
- ✅ Standardized offering
- ✅ Predictable costs
- ✅ Easy scaling
- ✅ Competitive pricing
- ✅ Sustainable business model

---

## 12. Practical Next Steps

### Immediate (Before First Customer)

**Week 1-2: Prepare Deployment**

1. ☐ Create deployment documentation
   - Installation guide
   - System requirements
   - Network requirements
   - Security checklist

2. ☐ Prepare deployment scripts
   - Automated installation
   - Database setup
   - Backup configuration
   - Monitoring setup

3. ☐ Create support materials
   - User manual
   - Admin manual
   - Troubleshooting guide
   - FAQ

4. ☐ Define pricing
   - Installation fee
   - Support contract
   - Hardware recommendations

5. ☐ Legal documents
   - Service agreement
   - Privacy policy
   - Terms of service
   - Support SLA

---

### First Customer Deployment

**Recommended Approach:**

1. **Find friendly first customer** (small clinic, willing to be pilot)
2. **Use on-premises deployment** (simpler to start)
3. **Heavily document everything** (for future customers)
4. **Charge lower price** (pilot pricing)
5. **Be available 24/7 first week** (ensure success)
6. **Collect detailed feedback**
7. **Refine process for next customer**

---

### After First Success

1. **Standardize deployment** (repeatable process)
2. **Consider cloud option** (if demand warrants)
3. **Build support team** (as you grow)
4. **Automate monitoring** (reduce manual work)
5. **Scale gradually** (don't over-commit)

---

## 13. Final Recommendations

### For Your First Deployment:

**✅ Start with On-Premises**
- Simpler for learning
- Lower risk for you
- Customer has more control
- Easier compliance

**✅ Choose a Friendly Pilot Clinic**
- Willing to give feedback
- Understanding of pilot nature
- Accessible for on-site support

**✅ Document Everything**
- Installation process
- Issues encountered
- Solutions applied
- Time required

**✅ Build Support Infrastructure**
- Remote access tools
- Monitoring setup
- Backup verification
- Update procedures

### Transition to Cloud When:

✅ You have 3+ successful on-premises installations  
✅ You understand operational requirements  
✅ You have reliable support processes  
✅ You can commit to 99.9% uptime  
✅ You have budget for cloud infrastructure  

---

## Conclusion

**The deployment decision depends on:**
- Your customer's needs and budget
- Your capabilities and resources
- Compliance requirements
- Long-term business model

**For starting out:** On-premises is simpler and lower risk.

**For scaling:** Cloud multi-tenant is more sustainable.

**Both are valid:** Many successful EMR companies use both models.

---

**Next Steps:**
1. Choose your initial deployment model
2. Find pilot customer
3. Complete Issue #4 (Ubuntu testing) to validate deployment process
4. Create deployment documentation
5. Deploy to first customer
6. Learn and iterate

---

**Document Author:** ANSVK, Project Manager  
**Date:** 2026-11-18  
**Status:** Ready for Deployment Planning

---

**END OF GUIDE**
