- **Purpose & Audience**
  - What Ronin is (1-page executive summary)
  - Who this doc is for (Business, Dev, Data, Ops, Leadership)
- **Mental Model First**
  - What problem Ronin solves
  - What Ronin is **not**
  - Core concepts & vocabulary (canonical glossary)
- **System at 10,000 ft**
  - End-to-end flow (Source → Staging → Core → Ops → Reports)
  - Major domains (Finance, Media, Clients, Security, MDM)
- **Progressive Depth (Tiered Learning)**
  - Beginner → Mid → Senior → Architect paths
  - Clear "You should read this next" pointers
- **Domain-Driven Breakdown**
  - Budgeting
  - OPEX
  - Actuals
  - Media (MyFC)
  - Clients & Hierarchies
  - Targets
  - Scenarios & States
- **Data Architecture**
  - Schemas and why they exist
  - Table purpose (not column dumps)
  - Cardinality & ownership
  - Source-of-truth rules
- **Data Flow & ETL**
  - Standard ETL pattern (staging → merge → enhance)
  - What guarantees idempotency
  - Failure modes & recovery
  - Pipeline metadata & lineage
- **Business Logic**
  - Stored procedure categories
  - What logic lives where (DB vs ETL vs reports)
  - Critical procedures explained narratively
- **Security Model**
  - Philosophy (authority, intent, accountability)
  - Security layers (header, detail, RLS, functions)
  - How a user query is filtered end-to-end
- **Change & History**
  - Why change history exists
  - Triggers vs temporal tables
  - How to debug "why did this number change?"
- **Reporting Layer**
  - View taxonomy (helper, fact, comparison)
  - How Power BI consumes Ronin
  - Semantic model ownership
- **Operational Playbooks**
  - "If data is missing, do this"
  - "If numbers don't match, check this"
  - Common production incidents & root causes
- **Performance & Scale**
  - Big tables & why they're big
  - Indexing strategy
  - Partitioning logic
  - What _not_ to query directly
- **Integration Map**
  - All upstream systems
  - What data comes from where
  - Frequency & freshness expectations
- **Governance & Ownership**
  - Who owns which domain
  - Who approves changes
  - What requires cross-team sign-off
- **Design Principles**
  - Non-negotiables (patterns you never break)
  - Anti-patterns (things that look tempting but are wrong)
- **Decision Log**
  - Why things are the way they are
  - Historical constraints
  - Trade-offs made consciously
- **Getting Productive Fast**
  - "Day-1 checklist"
  - Queries every new joiner should run
  - Tables every role must know
- **Appendices**
  - Glossary
  - Acronyms
  - Naming conventions
  - Reference diagrams

**Litmus test:**

If someone senior leaves tomorrow, Ronin still makes sense to the next person.

**Front Door**

- **0.1 What is Ronin (1 page)**
  - 3-line definition (business + tech)
  - Top 5 things users do in Ronin (Budget, OPEX, Forecast, Actuals, Media/MyFC)
  - "Ronin in one diagram" (Source systems → Staging → Core → Ops → Views/Power BI)

01_BEGINNER_GUIDE

- **0.2 Who should read what**
  - Beginner / Mid / Senior reading paths (with links)

README

- **0.3 Glossary (canonical)**
  - Budget Header vs Position
  - OPEX Header vs Position
  - Actuals vs Forecast vs Budget
  - Scenario, Stage, State, LoV, MDM, DPC, MSH, RLS

**1) Mental Model (make people "get it" fast)**

- **1.1 Ronin's job in the enterprise**
  - Ronin is the _planning + integration + reporting backbone_ (not the source ERP)
  - What comes from SAP/D365/Workday/etc vs what is created inside Ronin

RONIN_APPLICATION_ANALYSIS

- **1.2 The "Truth Model"**
  - What tables are "source of truth" (Core master data)
  - What tables are "operational truth" (dbo budgets/opex/targets)
  - What tables are "enhanced truth" (Synapse dimensions)
- **1.3 The "3 timelines" concept**
  - Budget timeline (planned)
  - OPEX timeline (rolling expectation)
  - Actuals timeline (accounting truth)
  - Where comparisons happen (helper/comparison views)

RONIN_APPLICATION_ANALYSIS

**2) System at 10,000 ft (Architecture)**

- **2.1 Enterprise architecture page**
  - External systems list + what data each contributes

RONIN_APPLICATION_ANALYSIS

- - Integration methods (ETL, API, file imports if applicable)

03_SENIOR_GUIDE

- **2.2 Database "schema map"**
  - **Core** = master entities (Client/Account/Employee/Entity/CostCenter)

RONIN_APPLICATION_ANALYSIS

- - **Core_Staging** = landing zone for Core loads

RONIN_APPLICATION_ANALYSIS

- - **dbo** = operational planning tables (Budget/OPEX/Targets/Security)

RONIN_APPLICATION_ANALYSIS

- - **Synapse + Synapse_Staging** = enhancement layer & its staging

RONIN_APPLICATION_ANALYSIS

- - **mdm** = reference + master governance entities (DPC, Markets, Semantic Models)

RONIN_APPLICATION_ANALYSIS

- **2.3 "How to navigate Ronin"**
  - Finding anything via: Schema → Table type → Views → SPs
  - Conventions: T_tables, V_ views, sp_Load_ loaders, ChangeHistory, temporal history tables

RONIN_APPLICATION_ANALYSIS

**3) Domain Documentation (each domain is a "mini book")**

**3A) Budgeting**

- **3A.1 What "Budget" means in Ronin**
- **3A.2 Core objects**
  - T_BudgetHeader (scenario/year/client/status/ownership)
  - T_BudgetPosition (line items, accounts, dates, amounts)
- **3A.3 Lifecycle**
  - Draft → Submitted → Approved → Closed → Reopened (state machine)
  - What each state allows/disallows (edits, approvals, comparisons)

01_BEGINNER_GUIDE

- **3A.4 Key procedures**
  - Add positions, batch add, reopen flow, validations

RONIN_APPLICATION_ANALYSIS

- **3A.5 Reporting + comparison**
  - Which views drive "Budget vs Actual" and what logic they embed

RONIN_APPLICATION_ANALYSIS

- **3A.6 Common failure modes**
  - Missing actuals, wrong mapping, incorrect date granularity, security filtering

**3B) OPEX**

- **3B.1 What OPEX is (and how it differs from Budget)**
- **3B.2 Objects**
  - T_OpexHeader, T_OpexPosition

RONIN_APPLICATION_ANALYSIS

- **3B.3 Lifecycle + reopening**
- **3B.4 Key procedures**
  - Position add / batch add / reopen

RONIN_APPLICATION_ANALYSIS

- **3B.5 OPEX forecast logic**
  - How it blends: prior actuals, current run-rate, scenario assumptions
- **3B.6 Reconciliation playbook**
  - "Why OPEX differs from Actuals?" checklist

**3C) Actuals**

- **3C.1 Where actuals come from (sources + cadence)**
- **3C.2 Key tables**
  - Actual amount detail / with cost center allocation
  - Staging actuals patterns (e.g., R3 staging)

RONIN_APPLICATION_ANALYSIS

- **3C.3 Mapping rules**
  - Account mapping, cost center mapping, entity mapping
- **3C.4 Audit + traceability**
  - How to trace an actual number back to source system load and run id

**3D) Media / MyFC**

- **3D.1 What MyFC is**
- **3D.2 Key entities**
  - Media types, subtypes, partner levels, sales house mapping, vendors

RONIN_APPLICATION_ANALYSIS

- **3D.3 Forecast hierarchy rules**
- **3D.4 Common reporting pitfalls**
  - hierarchy mismatch, subtype mapping, vendor duplicates

**3E) Clients & Hierarchies**

- **3E.1 Client, ClientGroup, ClientGroupByCompany**
- **3E.2 Dentsu Parent Client (DPC)**
  - What it represents + why it matters

RONIN_APPLICATION_ANALYSIS

- - Map/unmap/merge procedures and "when to use which"

RONIN_APPLICATION_ANALYSIS

- **3E.3 Market Stakeholder (MSH)**
  - Lifecycle + mapping rules

RONIN_APPLICATION_ANALYSIS

- **3E.4 De-duplication / merge policy**
  - Rules for "same client across systems"

**3F) Accounts**

- **3F.1 4-level BPC hierarchy (L1-L4)**
- **3F.2 Chart of accounts rules**
  - SAP keys, BPC codes, GL flags

RONIN_APPLICATION_ANALYSIS

- **3F.3 Security interaction**
  - Account-level access filtering via functions

RONIN_APPLICATION_ANALYSIS

**3G) Org Structure (Entity / CostCenter / Employee)**

- **3G.1 Entity master**
- **3G.2 Cost centers**
- **3G.3 Employees + Workday enhancements**
  - What is in Core vs what is in enhancement tables

RONIN_APPLICATION_ANALYSIS

**3H) Targets (Strawberry / TV Share / NB)**

- **3H.1 What each target type means**
- **3H.2 How change history is used for targets**

RONIN_APPLICATION_ANALYSIS

**4) Data Architecture Deep Dive (how the DB is "meant to be used")**

- **4.1 Table taxonomy**
  - Master tables (Core)
  - Operational tables (dbo)
  - Enhancement dimensions (Synapse)
  - Reference lists (mdm + LoV)

RONIN_APPLICATION_ANALYSIS

- **4.2 "Do not query" guidance**
  - Which tables are too raw / too heavy
  - Prefer views for analytics (why)
- **4.3 Canonical joins**
  - Client ↔ Group ↔ Company
  - BudgetHeader ↔ BudgetPosition ↔ Account ↔ Date
  - OpexHeader ↔ OpexPosition ↔ Account
- **4.4 Naming conventions**
  - \*\_ChangeHistory, temporal history tables, V_\* views, fn_\* security filters

RONIN_APPLICATION_ANALYSIS

**5) ETL & Integration (the "truth pipeline")**

- **5.1 The standard pattern**
  - Land in staging → validate → transform/enrich → MERGE into production → track pipeline

03_SENIOR_GUIDE

- **5.2 Idempotency rules**
  - How reruns behave, how duplicates are avoided
- **5.3 Pipeline tracking**
  - Where run ids live, how to trace a load end-to-end

RONIN_APPLICATION_ANALYSIS

- **5.4 Error handling + recovery**
  - Typical causes: schema mismatch, mapping gaps, referential breaks, late arriving data
  - "Fix forward" steps and rollback boundaries
- **5.5 Source-by-source integration pages**
  - SAP R3/BPC
  - D365
  - Workday
  - Navision, Paprika, NetSuite, BMD

RONIN_APPLICATION_ANALYSIS

**6) Security Model (must be crystal clear)**

- **6.1 Security mental model**
  - Header defines context; detail defines rules; functions enforce; views apply

RONIN_APPLICATION_ANALYSIS

- **6.2 RLS (row level security)**
  - How dynamic RLS definitions work
  - How allowed values are computed per user

RONIN_APPLICATION_ANALYSIS

- **6.3 Key security functions**
  - BPCL3, DccCam, KSB security functions (inputs/outputs, examples)

RONIN_APPLICATION_ANALYSIS

- **6.4 Debug playbook**
  - "User can't see X" checklist:
    - user login mapping
    - header entry exists
    - detail rules exist
    - function output contains expected ids
    - views apply the correct filters

**7) Change History, Auditing, Temporal Data**

- **7.1 Why there are two mechanisms**
  - Trigger-based change history tables
  - Temporal tables (MDM)

RONIN_APPLICATION_ANALYSIS

- **7.2 "Explain this number" workflow**
  - For a budget line: show all changes over time and who changed it
- **7.3 Governance policies**
  - What must be auditable
  - Retention strategy / archiving assumptions

**8) Reporting & Semantic Models (Power BI layer)**

- **8.1 View taxonomy**
  - Helper views
  - Fact views
  - Comparison views
  - Dimension views

RONIN_APPLICATION_ANALYSIS

- **8.2 Semantic model inventory**
  - What models exist, ownership, refresh cadence, dependencies

RONIN_APPLICATION_ANALYSIS

- **8.3 "How to add a metric safely"**
  - Add/adjust view → validate security → refresh semantic model → validate totals

**9) Operations Playbooks (the pages people actually use daily)**

- **9.1 Missing data**
  - Staging has data?
  - Loader ran?
  - Merge conditions?
  - Pipeline status?

02_MID_LEVEL_GUIDE

- **9.2 Numbers don't match**
  - Date mapping, scenario filters, account mapping, security filters, duplicates
- **9.3 Performance incidents**
  - which tables are hot
  - which joins explode row counts
  - how to use the correct filtered views

**10) Performance, Scale, and "How not to hurt Ronin"**

- **10.1 Big tables + usage rules**
  - Which are the largest and why (enhancement tables, position tables)

RONIN_APPLICATION_ANALYSIS

- **10.2 Index / partition strategy (documented expectations)**
- **10.3 Query patterns**
  - "good vs bad" examples
  - required filters (year/scenario/company)
- **10.4 Batch operations**
  - safe batch sizes, transactions, retry strategy

**11) Engineering Standards**

- **11.1 Where logic should live**
  - ETL vs Stored Procedures vs Views vs BI
- **11.2 Change checklist**
  - data model changes
  - security impact
  - reporting impact
  - backfill strategy
- **11.3 Decision log**
  - record why important design decisions were made

**12) Onboarding: "Become dangerous in 2 days"**

- **Day 1**
  - 10 queries to understand data
  - trace one client from source → staging → core → budget → report
- **Day 2**
  - trace a security issue
  - trace a number mismatch
  - read one ETL loader end-to-end

**13) Appendices (reference library)**

- Glossary & acronyms
- Schema dictionary
- Procedure catalog (categorized)
- View catalog (by domain)
- Example datasets / test cases
- Mermaid diagrams index
