# The Immutable Global Data Guard

##  Goal

Design a multi-layered AWS backup and disaster recovery solution that protects against:

* Infrastructure failure (AZ-level)
* Accidental deletion
* Malicious or compromised admin actions (ransomware scenario)

---

##  Architecture Overview

### High Availability Foundation

* VPC across 2 Availability Zones

* 2 Public Subnets (EC2)

* 2 Private Subnets (RDS)

* EC2 (Web Server) in Public Subnet

* RDS (MySQL/Postgres) in Private Subnets with Multi-AZ enabled

---

##  Backup Strategy (AWS Backup Plan)

### Backup Plan Configuration

* Backup frequency: Daily
* Resource selection: Tag-based (`Project: Dataguard`)
* Services protected:

  * EC2 (EBS snapshots)
  * RDS databases

### Lifecycle Policy

* Transition to cold storage after 30 days
* Long-term retention: 630 days

---

##  Immutable Backup Protection (Vault Lock)

* Backup Vault configured with **Compliance Mode**
* Enforces Write-Once-Read-Many (WORM) model
* Prevents deletion or modification of backups
* Protection applies even to root users

---

##  RTO / RPO Design

* **RPO:** ≤ 24 hours (daily backups)
* **RTO:**

  * EC2: minutes (snapshot restore)
  * RDS: minutes to hours (depending on size)

---

##  Failure Simulation

### Scenario 1: Accidental Deletion

* Deleted EC2 instance → restored from snapshot

### Scenario 2: Database Failure

* Simulated RDS issue → restored from backup

### Scenario 3: Security Breach Simulation

* Attempted backup deletion → blocked due to Vault Lock

---

##  Key Design Decisions

* Used Backup Plan instead of manual backups → automation & scalability
* Tag-based selection → zero-touch onboarding for new resources
* Multi-AZ RDS → high availability before backup
* Vault Lock → ransomware protection

---

##  Services Used

* AWS Backup
* EC2 / EBS
* RDS (Multi-AZ)
* VPC
* IAM

---

##  Future Enhancements

* Cross-region backup replication
* SNS alerts for backup failures
* Move EC2 behind ALB (production-grade architecture)
