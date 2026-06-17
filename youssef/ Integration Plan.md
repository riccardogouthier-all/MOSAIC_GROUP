# Integration Report: Themea × Pro Studio


## Executive Summary

**Timeline:** 6 months
**Upfront Cost:** €60–100K
**Annual Cost:** €53K/year
**IT Staffing:** 1 person

Adopt Google Workspace + Odoo SaaS. Use AWS as identity broker and infrastructure. Pro Studio staff unchanged. Themea staff get better tools.

---

## Target Architecture

```
┌──────────────────────────────────────────┐
│  AWS IAM Identity Center (Single Login)  │
│  One password = access to everything     │
└────────┬───────────────┬────────────────┘
         │               │
         ▼               ▼
    Google Workspace   Odoo SaaS
    • Email           • CRM
    • Drive           • Accounting
    • Meet            • Timesheet
    • Docs            • Inventory
```

---

## The 10 Services: Selected Platform & Why

### 1. Email & Calendar
**✓ CHOSEN:** Google Workspace

| Why Google? | SugarCRM Now | Google | Profis |
| --- | --- | --- | --- |
| Easy to share | Limited | | No |
| Cost/user | €8 + IT labor | €10/mo | €12/mo |
| Pro Studio already using? | No | **YES** | No |

**Migration:** Export Zimbra → Google DataTransfer API → 2-week coexistence → MX cutover
**Timeline:** 4 weeks

---

### 2. Documents & Spreadsheets
**✓ CHOSEN:** Google Workspace (Docs, Sheets, Slides)

| Why Google? | MS365/LibreOffice | Google |
| --- | --- | --- |
| Real-time collab | Poor | |
| Works in browser | No | |
| Version history | Limited | Unlimited |
| Cost | €15/mo | **Included** |

**Migration:** Convert docs to Google format → organize by team → 6-month archive period
**Timeline:** 3 weeks

---

### 3. Video Conferencing & Chat
**✓ CHOSEN:** Google Meet + Google Chat

| Why Google? | MS Teams | Google |
| --- | --- | --- |
| Ease of use | Complex | Simple |
| Video quality | Good | Excellent |
| Chat integrated | Separate | Built-in |
| Pro Studio using? | No | **YES** |

**Migration:** Disable Teams → Archive chats to Google Drive → Train on 2-click calling
**Timeline:** 2 weeks

---

### 4. File Storage
**✓ CHOSEN:** Google Drive

| Why Google? | NextCloud | Google |
| --- | --- | --- |
| 99.99% uptime | No (depends on server) | |
| Mobile access | Limited | Perfect |
| IT maintenance | High (backup, patching) | Zero |
| Cost | €5/mo + labor | **Included** |

**Migration:** AWS DataSync (NextCloud → S3 staging) → Google Drive API ingest → organize by team
**Timeline:** 3 weeks

---

### 5. CRM (Customers, Accounts, Opportunities)
**✓ CHOSEN:** Odoo SaaS

| Why Odoo? | SugarCRM | Odoo |
| --- | --- | --- |
| Modern UI | 1990s design | |
| Linked to accounting | Separate | Automatic |
| Mobile app | Poor | Full app |
| IT maintenance | High | Zero |
| Pro Studio using? | No | **YES** |

**Migration:** AWS Glue ETL (SugarCRM → Odoo CRM schema) → 500 accounts + contacts → validate
**Timeline:** 3 weeks design + 2 weeks execution

---

### 6. Accounting & Invoicing
**✓ CHOSEN:** Odoo SaaS (Accounting Module)

| Why Odoo? | Profis | Odoo |
| --- | --- | --- |
| Modern interface | Legacy (1990s) | |
| Mobile access | No | Full app |
| Italian compliance | Yes (outdated) | Up-to-date |
| IT maintenance | High (Windows server) | Zero |
| Linked to CRM? | No | Yes (automatic invoicing) |

**Migration:** AWS DMS (Profis SQL Server → Odoo PostgreSQL) → chart of accounts + invoices → 2-week Profis fallback
**Timeline:** 3 weeks design + 1 week execution

---

### 7. Time Tracking & Project Hours
**✓ CHOSEN:** Odoo SaaS (Timesheet Module)

| Why Odoo? | Kimai | Odoo |
| --- | --- | --- |
| Linked to projects | No | |
| Auto-billing | Manual | |
| Mobile app | Limited | |
| Cost | €200/mo (self-hosted) | **Included** |

**Migration:** Export Kimai → transform → bulk ingest to Odoo Timesheet → reconcile hours
**Timeline:** 2 weeks

---

### 8. Source Control & Git Repositories
**✓ CHOSEN:** Bitbucket SaaS

| Why Bitbucket? | GiTea | Bitbucket |
| --- | --- | --- |
| Built-in CI/CD | Limited | |
| Code review tools | Basic | Advanced |
| Reliability (SLA) | Medium | 99.99% |
| Pro Studio using? | No | **YES** |

**Migration:** Git mirror (GiTea → Bitbucket) → update CI/CD webhooks → test builds
**Timeline:** 2 weeks

---

### 9. Internal Documentation & Knowledge Base
**✓ CHOSEN:** MediaWiki on AWS EC2 + RDS

We keep MediaWiki (don't switch to Confluence) because:
- Both companies already use it (zero retraining)
- Low cost on AWS (€20/month)
- Can migrate to Confluence later if needed
- Not customer-facing (internal only)

**Migration:** Merge Themea + Pro Studio wikis into one database → AWS RDS → EC2 host
**Timeline:** 3 weeks

---

### 10. Identity & Access Management (Master Login)
**✓ CHOSEN:** AWS IAM Identity Center

This is the "glue" that connects everything.

**How it works:**
1. Employee logs in once to AWS IAM Identity Center (username + password + fingerprint)
2. Clicks "Google Workspace" → automatically logged into Gmail
3. Clicks "Odoo" → automatically logged into CRM
4. Clicks "Bitbucket" → automatically logged into Git

**Why AWS IAM Identity Center?**
- Bridges on-premises (Samba4) to cloud (Google + Odoo)
- Central MFA enforcement (multi-factor authentication)
- Automatic user provisioning (new hire → all systems)
- Audit logging (AWS CloudTrail)
- No vendor lock-in (industry standard SAML 2.0)

**Integration:**
```
AWS IAM Identity Center
├─ SAML 2.0 → Google Workspace
├─ OpenID Connect → Odoo SaaS
└─ OAuth → Bitbucket
```

**Timeline:** 2 weeks

---

## Implementation Timeline

```
WEEKS 1–4: Setup
├─ AWS IAM Identity Center configuration
├─ Google Workspace tenant creation
├─ Odoo SaaS provisioning
└─ Data inventory (all 10 systems)

WEEKS 5–8: Data Migration
├─ Email (Zimbra → Google)
├─ Files (NextCloud → Google Drive)
├─ CRM (SugarCRM → Odoo)
└─ Accounting (Profis → Odoo)

WEEKS 9–10: Identity Cutover
├─ Test SAML federation
└─ Go-live with AWS IAM Identity Center

WEEKS 11–13: Cleanup
├─ Migrate remaining systems
├─ Decommission legacy apps
└─ Final validation

GO-LIVE: December 2026
```

---

## Cost Breakdown

### Year 1

**One-time Setup:**
- AWS infrastructure: €10K
- Data migration: €25K
- Google Workspace setup: €8K
- Odoo configuration: €12K
- Testing: €7K
- **Setup Total: €62K**

**Year 1 Recurring:**
- Google Workspace (100 users): €12K
- Odoo SaaS (enterprise plan): €24K
- Bitbucket (20 developers): €2.4K
- AWS infrastructure: €8K
- MediaWiki: €2.4K
- Support: €5K
- **Annual Total: €53.8K**

**Year 1 Total: €115.8K**

### Years 2+

**Annual Cost: €53.8K/year** (no more setup)

**3-Year Total: €169.6K**

---

## Success Metrics

✅ 100% of users can log in with single credentials

✅ 99% of data migrates correctly

✅ All email flows through Google with zero loss

✅ All 500 GB files accessible in Google Drive

✅ CRM + accounting integrated (invoicing automated)

✅ Legacy systems decommissioned (Zimbra, Profis, NextCloud, GiTea offline)

✅ IT overhead drops to 1 person

---

## Why This Plan?

| Reason | Details |
| --- | --- |
| **Speed** | 6 months to unified operations |
| **Cost** | €60–100K upfront, €53.8K/year ongoing |
| **IT Simplicity** | 1 person manages integration; vendors handle the rest |
| **Pro Studio Impact** | Zero change for 50 staff (already on Google + Odoo) |
| **Themea Upgrade** | All tools are better than current systems |
| **No Vendor Lock-in** | Can switch Google/Odoo anytime (data is portable) |
| **Risk Management** | Data discrepancies are recoverable and manageable |



