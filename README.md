# 🔐 ShadowVault — Intelligent Adaptive Data Anonymization Platform


---

## 🚀 Quick Setup (Windows)

### Step 1 — Install Python dependencies
```bash
pip install -r requirements.txt
```

### Step 2 — Download spaCy model
```bash
python -m spacy download en_core_web_sm
```

### Step 3 — Run ShadowVault
```bash
python app.py
```

Open: http://127.0.0.1:5000

---

## 📁 Project Structure

```
shadowvault/
├── shadowvault.py    ← Core engine (detection, encryption, tokenisation)
├── app.py            ← Flask REST API
├── templates/
│   └── index.html    ← Single-page UI
├── static/
│   ├── css/style.css
│   └── js/app.js
├── uploads/          ← Session input files
├── outputs/          ← Tokenised output files
└── vault/
    └── <session_id>/
        ├── vault.db      ← Encrypted token→value mapping
        ├── master.key    ← AES-256 master key
        └── keys/
            ├── doctor.key
            ├── auditor.key
            ├── legal_team.key
            ├── analyst.key
            └── admin.key
```
 

---

## ✨ Features

### 1. 🔍 PII Detection 
- **Regex**: 20+ patterns — Aadhaar, PAN, passport, phone, email, addresses
- **spaCy NER**: Named entity recognition for names, locations, dates
- **Ollama LLM**: Contextual/implied PII detection (requires Ollama running)(optional)

### 2. 🔄 Reversible Anonymization
- PII replaced with encrypted tokens: `<<SV-TOKEN-A1B2C3D4>>`
- Real values stored in AES-256 encrypted SQLite vault
- Role-based decryption: admin > doctor > auditor > legal > analyst > viewer

### 3. 🔗 Cross-Document Entity Linking
- Detects same person across multiple documents
- Handles name variations: "Harini Shankar" = "H. Shankar" = "H.S."
- Assigns consistent pseudonyms (PERSON-0001) across all documents

### 4. 📊 Privacy Risk Score
- Weighted scoring based on PII type sensitivity
- k-anonymity risk estimation
- Levels: Low / Medium / High / Critical

### 5. 📋 Compliance Profiles
- **GDPR** (EU) — email, name, location, IP, cookies
- **HIPAA** (Healthcare) — MRN, diagnosis, insurance, patient data
- **DPDP Act 2023** (India) — Aadhaar, PAN, voter ID, bank account
- **PCI-DSS** (Financial) — card numbers, CVV, routing numbers

---

## 🔐 Role-Based Access

| Role    | Can Decrypt                                    |
|---------|------------------------------------------------|
| admin   | Everything                                     |
| doctor  | Name, DOB, MRN, Phone, Email, Insurance        |
| auditor | Aadhaar, PAN, SSN, Bank Account, Insurance     |
| legal   | Name, DOB, Address, Passport, Voter ID, SSN    |
| analyst | ZIP, Dates, Insurance ID only                  |
| viewer  | Nothing (can see tokens only)                  |

---

## ⚠️ If Ollama is not running
ShadowVault works without Ollama — it falls back to regex + spaCy.
The LLM layer adds contextual/implied PII detection on top.

---

## 🧪 Test with sample documents
Try uploading:
- A scanned ID card (JPG/PNG) — detects Aadhaar, name, DOB, photo
- A medical PDF — detects MRN, patient name, insurance ID
- Multiple documents with the same person — see entity linking in action
