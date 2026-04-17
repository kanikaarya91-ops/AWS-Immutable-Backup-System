# AWS Immutable Backup Guard (EC2 & RDS)

## 🎯 Project Goal
To implement a tamper-proof, automated backup solution across an AWS environment using Tag-based discovery and Vault Lock for ransomware protection.

## 🚀 What I Built
* **Automated Discovery:** Used AWS Backup to automatically find and protect any resource tagged `Project: Dataguard`.
* **Multi-Service Support:** Configured protection for both **EC2 instances** and **RDS databases**.
* **Immutability:** Locked the Backup Vault in **Compliance Mode**, ensuring backups cannot be deleted by anyone (including the root user) for 630 days.

## 🛠️ Challenges I Overcame
* **IAM Role Resolution:** Troubleshooted and corrected a case-sensitivity issue where the service role name `SSM` had to match the policy exactly.
* **Service Configuration:** Enabled RDS Backup "Opt-in" at the account level to allow the plan to access database snapshots.
* **Tag Formatting:** Fixed a JSON syntax error where tag values were incorrectly formatted as lists instead of strings.

## ✅ Results
* Successfully triggered an automated backup at **18:43 IST**.
* Verified that recovery points are "Locked" and cannot be manually deleted.
