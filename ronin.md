Purpose & Audience

What Ronin is (1-page executive summary)

Who this doc is for (Business, Dev, Data, Ops, Leadership)

Mental Model First

What problem Ronin solves

What Ronin is not

Core concepts & vocabulary (canonical glossary)

System at 10,000 ft

End-to-end flow (Source → Staging → Core → Ops → Reports)

Major domains (Finance, Media, Clients, Security, MDM)

Progressive Depth (Tiered Learning)

Beginner → Mid → Senior → Architect paths

Clear “You should read this next” pointers

Domain-Driven Breakdown

Budgeting

OPEX

Actuals

Media (MyFC)

Clients & Hierarchies

Targets

Scenarios & States

Data Architecture

Schemas and why they exist

Table purpose (not column dumps)

Cardinality & ownership

Source-of-truth rules

Data Flow & ETL

Standard ETL pattern (staging → merge → enhance)

What guarantees idempotency

Failure modes & recovery

Pipeline metadata & lineage

Business Logic

Stored procedure categories

What logic lives where (DB vs ETL vs reports)

Critical procedures explained narratively

Security Model

Philosophy (authority, intent, accountability)

Security layers (header, detail, RLS, functions)

How a user query is filtered end-to-end

Change & History

Why change history exists

Triggers vs temporal tables

How to debug “why did this number change?”

Reporting Layer

View taxonomy (helper, fact, comparison)

How Power BI consumes Ronin

Semantic model ownership

Operational Playbooks

“If data is missing, do this”

“If numbers don’t match, check this”

Common production incidents & root causes

Performance & Scale

Big tables & why they’re big

Indexing strategy

Partitioning logic

What not to query directly

Integration Map

All upstream systems

What data comes from where

Frequency & freshness expectations

Governance & Ownership

Who owns which domain

Who approves changes

What requires cross-team sign-off

Design Principles

Non-negotiables (patterns you never break)

Anti-patterns (things that look tempting but are wrong)

Decision Log

Why things are the way they are

Historical constraints

Trade-offs made consciously

Getting Productive Fast

“Day-1 checklist”

Queries every new joiner should run

Tables every role must know

Appendices

Glossary

Acronyms

Naming conventions

Reference diagrams

Litmus test:

If someone senior leaves tomorrow, Ronin still makes sense to the next person.
