# ResearchBridge AI

**Secure AI-Powered Research Collaboration, Intellectual Property Protection and Fair Reward Distribution Platform**

---

## Overview

ResearchBridge AI is a full-stack web platform that connects Research Sponsors, Domain Experts and Students to collaborate on academic and industry research projects. It provides:

- ✅ Verified four-role collaboration (Admin, Sponsor, Expert, Student)
- 📜 Digital project charter with recorded acceptance (SHA-256 hash)
- 🤖 AI research assistant (IBM watsonx.ai / OpenAI / Gemini / Mock)
- 🔗 Tamper-evident SHA-256 hash-chained contribution ledger
- 🛡️ Intellectual property protection and document access control
- 💰 Funded and non-funded reward distribution with charter-defined percentages
- 🔔 Real-time notifications
- 📊 Role-specific dashboards

---

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React.js 18, React Router, Axios, Tailwind-compatible CSS |
| Backend | Java 17, Spring Boot 3.2, Spring Security, Spring Data JPA |
| Database | MySQL 8.0 |
| Authentication | JWT (JJWT 0.11.5), BCrypt |
| AI Layer | IBM watsonx.ai / OpenAI / Google Gemini / Mock (configurable) |
| Hashing | SHA-256 (JDK built-in) |
| Build | Maven (backend), npm (frontend) |

---

## Project Structure

```
ResearchBridge-AI/
├── frontend/                   # React.js frontend
│   ├── src/
│   │   ├── components/         # Navbar, Footer, ProtectedRoute
│   │   ├── pages/              # Landing, Login, Register, Marketplace, AI, Ledger, Rewards
│   │   ├── dashboards/         # Admin, Sponsor, Expert, Student
│   │   ├── services/api.js     # Axios API layer
│   │   ├── context/AuthContext # JWT auth state
│   │   └── App.js              # Routing
│   └── package.json
│
├── backend/                    # Spring Boot backend
│   ├── src/main/java/com/researchbridge/
│   │   ├── controller/         # REST controllers
│   │   ├── service/            # Business logic
│   │   ├── repository/         # JPA repositories
│   │   ├── entity/             # JPA entities
│   │   ├── security/           # JWT filter, UserDetails
│   │   ├── ai/                 # AI provider abstraction
│   │   ├── ledger/             # Hash-chained ledger service
│   │   ├── dto/                # Data transfer objects
│   │   └── config/             # Security config, JPA auditing
│   └── pom.xml
│
├── database/
│   ├── schema.sql              # Complete MySQL schema (27 tables)
│   └── seed.sql                # Demo users, projects, ledger entries
│
├── docs/
│   ├── architecture.md         # System architecture
│   └── api-documentation.md    # API reference
│
├── .env.example                # Backend env vars template
└── README.md
```

---

## Prerequisites

- Java 17+
- Node.js 18+ and npm
- MySQL 8.0+
- Maven 3.8+
- (Optional) IBM watsonx.ai / OpenAI / Gemini API key

---

## Quick Start

### 1. Database Setup

```bash
mysql -u root -p < database/schema.sql
mysql -u root -p < database/seed.sql
```

### 2. Backend Setup

```bash
cd backend

# Copy and configure environment
cp ../.env.example ../.env
# Edit .env with your MySQL credentials

# Run with environment variables
export DB_USERNAME=root
export DB_PASSWORD=your_password
# (Windows PowerShell): $env:DB_PASSWORD="your_password"

mvn spring-boot:run
# Backend starts on http://localhost:8080
```

### 3. Frontend Setup

```bash
cd frontend
cp .env.example .env
npm install
npm start
# Frontend starts on http://localhost:3000
```

---

## Demo Accounts

After running `seed.sql`, use these accounts:

| Role | Email | Password |
|------|-------|----------|
| Admin | admin@researchbridge.ai | Admin@123 |
| Sponsor | sponsor1@techcorp.com | Sponsor@123 |
| Expert | expert1@university.edu | Expert@123 |
| Student | student1@college.edu | Student@123 |
| Student | student2@college.edu | Student@123 |

> These passwords use BCrypt hash `$2a$12$LQv3c1yqBWVHxkd0LHAkCOYz6TtxMQJqhN8/LewdBPj/RiSQF0iO6`

---

## AI Provider Configuration

The platform uses an **AI provider abstraction layer**. Set the `AI_PROVIDER` environment variable:

| Value | Description |
|-------|-------------|
| `MOCK` | Demo mode — clearly labeled fake responses. **Default.** |
| `OPENAI` | OpenAI GPT — requires `OPENAI_API_KEY` |
| `GEMINI` | Google Gemini — requires `GEMINI_API_KEY` |
| `WATSONX` | IBM watsonx.ai — requires `WATSONX_API_KEY` and `WATSONX_PROJECT_ID` |

If credentials are missing, the system automatically falls back to MOCK mode.

**AI Disclosure:** Every AI-generated output is:
- Clearly labeled as AI-generated
- Logged with provider, model, user ID, project ID and timestamp
- Requires human review before use in official project documents
- Never automatically counted as a human contribution

---

## Contribution Ledger

The platform uses **SHA-256 hash chaining** for the contribution ledger:

```
record_hash = SHA-256(recordId | projectId | actorId | actorType | actionType | 
                       description | resourceId | resourceType | timestamp | previousHash)
```

**Important Notice:** Hash chaining makes unauthorized changes **detectable** by breaking the chain. This is not equivalent to blockchain immutability. For production-grade immutability, combine with:
- Restricted database permissions (append-only grants)
- Independent encrypted backups
- External hash anchoring (e.g., timestamping authorities, public blockchain)

---

## Reward System

**Funded Projects:**
- Formula: `Individual Reward = Total Budget × Agreed Percentage / 100`
- Total allocations must equal exactly 100%
- Percentages are defined in the project charter before work begins
- Actual contributions support dispute review but do not silently change agreed percentages

**Non-funded Projects:**
- Digital certificates, co-authorship records, skill badges, achievement points
- Certificates issued upon project completion approval

**Payment Note:** The platform does not simulate actual bank transfers. Payment processing requires sponsor-configured payment provider sandbox credentials.

---

## IP Protection

Documents uploaded to projects are:
- SHA-256 fingerprinted for tamper detection
- Assigned confidentiality levels (PUBLIC, TEAM_ONLY, EXPERT_ONLY, SPONSOR_ONLY, RESTRICTED)
- Access-controlled by role and project membership
- Access-logged for audit
- Protected from external AI processing unless sponsor explicitly allows

**Important:** A hash proves file integrity but does not prove originality. Text similarity detection is an indicator, not a definitive plagiarism verdict.

---

## Project Charter

Before project activation, every team member must digitally accept the charter:
- Charter is auto-generated from project information
- SHA-256 hash is stored with the charter content
- Each acceptance records: user ID, timestamp, IP address, charter version
- Project does not activate until all required participants accept
- If charter changes, fresh acceptance is required

**This is a recorded digital acceptance, not a legally certified electronic signature.**

---

## Testing Workflows

1. Register as Admin → Login → Verify pending users
2. Register as Sponsor → Create project → Publish project
3. Register as Student → Browse marketplace → Apply to project
4. Admin verifies users → Sponsor reviews applications
5. All members accept charter → Project activates
6. Use AI assistant (Demo mode by default)
7. Verify ledger integrity at `/ledger/:projectId`
8. View reward allocations and certificates

---

## API Documentation

See [`docs/api-documentation.md`](docs/api-documentation.md) for complete API reference.

---

## Architecture

See [`docs/architecture.md`](docs/architecture.md) for system architecture overview.
