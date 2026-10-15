# Meme Coin Analysis Dashboard — Proposal

This folder contains the English proposal for the Data Engineering course (IDSC501 / IMEN891K / AXGS-501). The technical design is maintained in Korean under [architecture](../architecture/README.md).

## Reading order

1. [Project proposal](project-proposal.md): submission draft, including motivation, data, database decisions, evaluation, and a six-week work plan.
2. [Evidence and references](evidence-and-references.md): course requirements, inspected inputs, source feasibility, and limitations behind the proposal.
3. [Presentation](presentation/README.md): an eight-slide English PowerPoint for a seven-minute presentation, with Korean speaker notes, a PDF, and image credits.
4. [Architecture overview](../architecture/system-design.md): implementation decisions and the source-to-application diagram.

The agreed storage design is **PostgreSQL + MongoDB + a raw file archive**. PostgreSQL serves relational market data and analytical tables; MongoDB serves social documents and observations. PostgreSQL can also handle JSON, so the choice of MongoDB is an explicit modeling and educational decision, with additional operational costs.

## Status and submission details

- Document baseline: **2026-10-14**.
- Deliverable at this stage: a proposal and implementation design, not a deployed dashboard or working collector.
- Team members, exact submission deadline, and required submission format: **TBD**.
- The proposal targets roughly 1,200–1,600 words as an editorial choice. The inspected lectures do not specify this length.
- Six weeks is the project planning assumption, not a verified course deadline.
- The source notebook, price ZIP, and lecture HTML files are local inputs and are not redistributed in these folders. Their identifiers and findings are recorded in the evidence document.

Technical details belong in the architecture documents. When an implementation decision changes, update the architecture first and then revise the corresponding proposal summary. A documented design or successful spot check must not be reported as an implemented or continuously available service.
