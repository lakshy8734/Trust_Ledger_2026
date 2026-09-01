<div align="center">

# 🏆 Trust Ledger
### Reusable KYC Platform — Lloyds Reboot Hackathon 2026 Winner

[![Winner](https://img.shields.io/badge/Lloyds%20Reboot%20Hackathon%202026-%231%20Winner-gold?style=for-the-badge)](https://www.lloydsbankinggroup.com)
[![Hyperledger Fabric](https://img.shields.io/badge/Hyperledger%20Fabric-2.x-blue?style=for-the-badge)](https://hyperledger.org/use/fabric)
[![NestJS](https://img.shields.io/badge/NestJS-Backend-red?style=for-the-badge)](https://nestjs.com)
[![React](https://img.shields.io/badge/React-Frontend-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://reactjs.org)
[![Go](https://img.shields.io/badge/Chaincode-Go-00ADD8?style=for-the-badge&logo=go&logoColor=white)](https://golang.org)
[![AWS](https://img.shields.io/badge/Deployed-AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)](https://aws.amazon.com)
[![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)](https://postgresql.org)
[![Docker](https://img.shields.io/badge/Container-Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docker.com)

<br/>

> 🥇 **#1 of 200+ teams · 5000+ participants across Lloyds Banking Group**
>
> Awarded by **Sirisha Voruganti** *(Managing Director / Director, Lloyds Technology Centre)*
>
> ✅ Selected for **future productionisation** by Lloyds Banking Group

</div>

---

## 🧩 Problem

Customers applying for products across Lloyds, Halifax, and other group banks are forced to **repeat the entire KYC process every time** — submitting the same documents, causing:

- ❌ Duplicate verification effort
- ❌ Higher operational costs for banks
- ❌ Slower customer onboarding
- ❌ Poor customer experience

---

## ✅ Solution

Trust Ledger enables customers to **complete KYC once** at any Lloyds group bank and **reuse their verified identity** across the entire group — without ever sharing raw documents.

```
Customer completes KYC at Lloyds
         │
         ▼
Documents stay encrypted off-chain (PostgreSQL)
SHA-256 hash stored on Hyperledger Fabric
         │
         ▼
Customer grants consent on-chain
         │
         ▼
Halifax verifies credential — no re-submission needed
         │
         ▼
Loan / Credit Card decision made on verified identity
```

---

## 🏗️ Architecture

```
┌─────────────────────────────────┐
│         React Frontend          │
│  Admin · Customer · KYC         │
│  Registry · Ledger Explorer     │
│  Loan Applications · Decisions  │
└────────────────┬────────────────┘
                 │  REST API (/api/v1/kyc)
┌────────────────▼────────────────┐
│         NestJS Backend          │
│  Auth · KYC · Credential Share  │
│  Uploads · Fabric Gateway SDK   │
└────────────────┬────────────────┘
                 │  Fabric Gateway SDK
┌────────────────▼────────────────┐
│     Hyperledger Fabric Network  │
│                                 │
│  Lloyds Org    Halifax Org      │
│  Peer + CouchDB  Peer + CouchDB │
│         RAFT Orderer            │
└────────────────┬────────────────┘
                 │
┌────────────────▼────────────────┐
│  PostgreSQL (Off-chain)         │
│  Encrypted Documents · Audit    │
│  Consent History · Metadata     │
└─────────────────────────────────┘
```

---

## 🔐 Security Design

| Layer | Detail |
|---|---|
| **On-chain** | SHA-256 document hashes only — **zero PII on ledger** |
| **Identity** | X.509 certificates + MSP per organisation |
| **Transport** | TLS across all components |
| **Consent** | On-chain `GrantConsent` gate before any cross-bank verification |
| **Auth** | JWT-based authentication module (NestJS) |
| **Network** | Permissioned — only authorised orgs can participate |

---

## ⛓️ Chaincode Functions (Go)

| Function | Description |
|---|---|
| `CreateCustomer` | Register customer on ledger with document hash |
| `IssueKYC` | Mark customer as KYC verified by issuing bank |
| `GrantConsent` | Customer authorises cross-bank credential sharing |
| `VerifyKYC` | Second bank verifies existing credential (consent-gated) |
| `RevokeKYC` | Revoke credential — invalidates cross-bank verification |
| `GetHistory` | Full immutable transaction audit trail from ledger |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Blockchain** | Hyperledger Fabric 2.x, Go Chaincode, CouchDB |
| **Backend** | NestJS, TypeScript, Fabric Gateway SDK, PostgreSQL |
| **Frontend** | React, Tailwind CSS, Shadcn/UI |
| **Infrastructure** | Docker, Docker Compose, AWS |
| **Security** | X.509, MSP, TLS, JWT, SHA-256 |

---

## 🖥️ Frontend Pages

| Page | Description |
|---|---|
| **Admin Control Centre** | Full admin view of network, credentials, orgs |
| **Customer Dashboard** | Customer view of their KYC status and consent |
| **KYC Registry** | Registry of all issued credentials |
| **Ledger Explorer** | Real-time on-chain transaction viewer |
| **New Customer Upload** | Document upload and KYC initiation |
| **Loan Applications** | Apply for loans using verified KYC identity |
| **Loan Decision** | Bank decision view based on KYC credentials |
| **Credit Cards** | Credit card application using reused KYC |

---

## 🚀 Key Features

- ♻️ **Reusable KYC** — verify once, reuse across the banking group
- 🔒 **Zero PII on blockchain** — only cryptographic hashes stored on-chain
- ✅ **On-chain consent management** — customers control who sees their data
- 📋 **Immutable audit trail** — every transaction permanently recorded
- 🏦 **Multi-role UI** — Admin, Customer, Bank views
- 🔍 **Live Ledger Explorer** — real-time on-chain data visualisation
- 💳 **End-to-end banking workflow** — KYC → Loan/Credit decisions
- 🚫 **Credential revocation** — full lifecycle management

---

## 📁 Project Structure

```
Trust-Ledger/
├── backend/          # NestJS API + Fabric Gateway SDK
│   └── src/
│       ├── auth/           # JWT authentication
│       ├── kyc/            # KYC lifecycle management
│       ├── credential-share/  # Cross-bank sharing
│       ├── customers/      # Customer management
│       ├── fabric/         # Fabric connection
│       └── uploads/        # Document handling
├── chaincode/        # Go chaincode (Hyperledger Fabric)
│   └── trustledger/
│       └── contract/ # IssueKYC, VerifyKYC, GrantConsent...
├── frontend/         # React + Tailwind UI
│   └── src/
│       ├── pages/    # All dashboard pages
│       └── components/  # Shared components
├── fabric-network/   # Network config, scripts, crypto
└── docs/             # Architecture documentation
```

---

## 🏆 Recognition

> **🥇 1st Place — Lloyds Reboot Hackathon 2026**
> Competing against **200+ teams and 5000+ participants** across Lloyds Banking Group.
> Awarded by **Sirisha Voruganti** *(Managing Director / Director, Lloyds Technology Centre)*
> Project selected for **future productionisation** by Lloyds Banking Group.

---

<div align="center">

Built with ❤️ at **Lloyds Technology Centre, Hyderabad**

</div>

---

# Overall Architecture

```text
                    +----------------------+
                    |     React UI         |
                    +----------+-----------+
                               |
                        REST APIs
                               |
                    +----------v-----------+
                    |     NestJS API       |
                    |  Fabric Gateway SDK  |
                    +----------+-----------+
                               |
                 ==============================
                 Hyperledger Fabric Network
                 ==============================

         +----------------+     +----------------+
         |   Lloyds Org   |     | Halifax Org    |
         |                |     |                |
         | Peer0          |     | Peer0          |
         | CouchDB        |     | CouchDB        |
         +--------+-------+     +-------+--------+
                  \               /
                   \             /
                    \           /
                +---------------------+
                |    RAFT Orderer     |
                +---------------------+

                               |

                 PostgreSQL (Off-chain DB)

                 Encrypted Customer Documents
                 Consent History
                 Audit Logs
                 Application Metadata
```

---

# 1. Organization Structure

We'll keep **2 organizations**.

```text
Organizations

Org1
-----
Name : Lloyds

MSP ID :
LloydsMSP

Peer :
peer0.lloyds

Domain :
lloyds.com


---------------------------------

Org2
-----
Name :
Halifax

MSP ID :
HalifaxMSP

Peer :
peer0.halifax

Domain :
halifax.com


---------------------------------

Orderer

orderer.trustledger.com
```

### Why?

Simple.

Every transaction clearly crosses an organizational boundary.

---

# 2. Peer & Orderer Topology

```text
                     RAFT Orderer

                    orderer0

                        |

        -----------------------------------

        Lloyds Peer             Halifax Peer

        peer0                   peer0

        CouchDB                 CouchDB
```

For the hackathon:

* 1 Orderer
* 2 Peers
* 2 CouchDBs

No need for 3 RAFT orderers in a demo.

---

# 3. Network Layout

```text
Docker Network

trustledger-network

Containers

orderer

peer0.lloyds

couchdb.lloyds

peer0.halifax

couchdb.halifax

cli

nestjs

postgres

react
```

Everything runs inside Docker except React if we want local development.

---

# 4. Channel Configuration

One channel only.

```text
Channel

kycchannel
```

Both organizations join.

```text
Lloyds

    |

kycchannel

    |

Halifax
```

No multiple channels.

No private collections.

---

# 5. Ledger Schema

This is probably the most important decision.

## Asset Type

```text
KYCCredential
```

Ledger stores only the fingerprint.

```text
CredentialID

CustomerID

Hash

Issuer

IssueDate

ExpiryDate

Status

ConsentStatus

Version

TransactionID
```

### Example

```json
{
  "credentialId": "KYC-1001",
  "customerId": "CUST-001",
  "documentHash": "SHA256.....",
  "issuer": "Lloyds",
  "issueDate": "2026-07-20",
  "expiryDate": "2027-07-20",
  "status": "ACTIVE",
  "consent": true,
  "version": 1
}
```

### Notice

* No ID Proof
* No Address Proof
* No Passport
* No Name
* No Address

Exactly matching the privacy-first architecture.

---

# 6. Chaincode API

We will keep only business functions.

```text
InitLedger()

IssueKYC()

VerifyKYC()

VerifyClaim()

RevokeKYC()

UpdateConsent()

GetCredential()

GetHistory()

CredentialExists()
```

## IssueKYC()

### Input

```text
CustomerID

Hash

Issuer

Expiry
```

### Output

```text
CredentialID
```

---

## VerifyKYC()

### Input

```text
CredentialID
```

### Output

```text
Valid

Expired

Revoked

Issuer

Expiry
```

---

## VerifyClaim()

### Input

```text
CredentialID

ClaimType
```

Example:

```text
kyc_active
```

Returns:

```text
true
```

or

```text
false
```

No extra data is returned.

---

## RevokeKYC()

### Input

```text
CredentialID

Reason
```

### Output

```text
Revoked
```

---

## GetHistory()

Uses the Fabric History API.

Returns:

```text
Issue

Verify

Revoke

Updates
```

This is an excellent demo feature because judges love seeing immutable history.

---

# 7. REST API Contract

```text
POST /api/kyc/issue

POST /api/kyc/verify

POST /api/kyc/verify-claim

POST /api/kyc/revoke

GET /api/kyc/:credentialId

GET /api/kyc/history/:credentialId
```

---

# 8. PostgreSQL Schema

Blockchain should never become your database.

We'll store business data off-chain.

## customers

```text
customer_id

first_name

last_name

email

phone

dob

created_at
```

---

## documents

```text
document_id

customer_id

document_type

encrypted_path

hash

uploaded_at
```

---

## credentials

```text
credential_id

customer_id

issuer

status

expiry
```

---

## consent

```text
consent_id

customer_id

requested_by

approved

timestamp
```

---

## audit_logs

```text
event

actor

timestamp

remarks
```

---

# 9. Folder Structure

```text
trust-ledger/

├── fabric-network/
│
├── chaincode/
│     └── kyc/
│
├── backend/
│     └── nestjs/
│
├── frontend/
│     └── react/
│
├── postgres/
│
├── docs/
│
├── scripts/
│
├── docker-compose.yml
│
└── README.md
```

Inside chaincode:

```text
chaincode/

kyc/

    main.go

    contract.go

    models.go

    utils.go
```

---

# 10. End-to-End Transaction Flow

```text
Customer

↓

Lloyds Branch

↓

Officer verifies documents

↓

SHA256 Generated

↓

IssueKYC()

↓

Fabric Ledger

↓

Credential Created

↓

-------------------------

Customer visits Halifax

↓

Mortgage Application

↓

Halifax API

↓

VerifyKYC()

↓

Ledger

↓

ACTIVE

↓

Proceed
```

No documents are moved.

Only the document fingerprint is verified.

---

# 11. Demo Scenario

We'll make the demo feel like a real banking journey.

## Step 1 – Lloyds

Officer uploads:

* Passport
* Driving Licence
* Address Proof

Backend computes:

```text
SHA256
```

Stores encrypted documents in PostgreSQL/file storage.

Calls:

```text
IssueKYC()
```

Ledger now contains only:

```text
CredentialID

Hash

Issuer

Status
```

---

## Step 2 – Halifax

Customer applies for a mortgage.

Halifax requests consent.

Customer approves.

Halifax calls:

```text
VerifyClaim("kyc_active")
```

Ledger returns:

```text
true
```

Halifax skips document upload.

---

## Step 3 – Lloyds

Suppose fraud is detected.

Officer clicks:

```text
Revoke
```

Chaincode executes:

```text
RevokeKYC()
```

---

## Step 4 – Halifax

Customer reapplies.

```text
VerifyClaim()
```

Returns:

```text
false
```

Application stops automatically.

---

# Future Enhancement

Introduce a **Network Identity** instead of tying the ledger directly to a customer ID.

Instead of:

```text
CustomerID
```

Use:

```text
NetworkIdentityID
```

Example:

```text
NID-1A9F8D23
```

Then map it off-chain:

```text
NetworkIdentityID
      ↓
CustomerID
      ↓
Customer Profile
      ↓
Encrypted Documents
```

The blockchain never stores a bank-specific customer identifier. If another Lloyds Banking Group brand joins later, it only needs to understand the shared `NetworkIdentityID`, making the ledger more privacy-preserving and extensible while keeping the off-chain mapping within each institution.

I genuinely think this architecture is strong enough to stand up to technical questions from judges while still being achievable within a 48-hour implementation window.

The next step would be to design the **ledger data models (Go structs), transaction payloads, endorsement policy, and Docker/Fabric topology** in detail before writing the first line of code.
