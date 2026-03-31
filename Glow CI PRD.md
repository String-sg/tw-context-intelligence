# Glow Contextual Intelligence (Glow CI) — Product Requirements Document

**Status:** Draft v1.8 | **Last updated:** 2026-03-31 | **Author:** Jasmine Tay, PM

---

## Table of Contents

- [Background](#background)
  - [Problem Statement](#problem-statement)
  - [Evidence of Problem](#evidence-of-problem)
  - [Hypothesis of Root Causes](#hypothesis-of-root-causes)
- [Why Now](#why-now)
- [Success](#success)
  - [North Star Metric](#north-star-metric)
  - [Product Metrics](#product-metrics)
  - [Guardrail Metrics](#guardrail-metrics)
- [In Scope](#in-scope)
  - [SDT Pilot within TW](#sdt-pilot-within-tw)
  - [Addressable Market](#addressable-market)
  - [Data Classification Constraints](#data-classification-constraints)
  - [Out of Scope](#out-of-scope-this-phase)
- [Product Requirements](#product-requirements)
  - [Part 1: Technical Integration — Contextual Data + RAG + LLM](#part-1-technical-integration--contextual-data--rag--llm)
  - [Part 2: AI Chat Interface](#part-2-ai-chat-interface)
  - [Part 3: Recommendation Cards](#part-3-recommendation-card-surfacing-in-tw-student-page)
  - [Part 4: Analytics & Tracking](#part-4-analytics--tracking)
  - [Part 5: Native Resource Viewer](#part-5-knowledge-storage--retrieval--native-resource-viewer-in-tw)
- [Priority & Timeline](#priority--timeline)
  - [Delivery Priority](#delivery-priority)
  - [Timeline](#timeline)
  - [Key Dependencies](#key-dependencies)
- [Key Risks and Mitigations](#key-risks-and-mitigations)
- [Team Roles](#team-roles)

---

<details>
<summary>Changelog</summary>

| Version | Date | Author | Summary |
|---------|------|--------|---------|
| v1.8 | 2026-03-31 | Jasmine Tay | Restructured Part 1 Key Capabilities into Contextual Data / Knowledge Base / AI Model sub-sections |
| v1.7 | 2026-03-31 | Jasmine Tay | Reordered Product Requirements sections to match part numbering; renamed Part 1 |
| v1.6 | 2026-03-31 | Jasmine Tay | Added table of contents |
| v1.5 | 2026-03-31 | Jasmine Tay | Renumbered parts to reflect delivery priority order |
| v1.4 | 2026-03-31 | Jasmine Tay | Added other commitments notes to Core Product Team |
| v1.3 | 2026-03-31 | Jasmine Tay | Removed Stakeholders section (consolidated into Team Roles) |
| v1.2 | 2026-03-31 | Jasmine Tay | Added team capacity to Core Product Team table |
| v1.1 | 2026-03-31 | Jasmine Tay | Added Team Roles section |
| v1.0 | 2026-03-25 | Jasmine Tay | Updated tech stack to Google Cloud (Vertex AI + Google Drive); added GDrive TRA as compliance prerequisite; flagged Drive viewer as open question for Part 4 |
| v0.7 | 2026-03-18 | Jasmine Tay | Added changelog + Analytics & Tracking section |
| v0.6 | 2026-03-18 | Jasmine Tay | Added SDT data classification constraints and guardrail metric |
| v0.5 | 2026-03-18 | Jasmine Tay | Manual edits — updated Part 1 user stories, reordered priority table |
| v0.4 | 2026-03-18 | Jasmine Tay | Renamed "Backend engineer" → "Engineer" across all user stories |
| v0.3 | 2026-03-18 | Jasmine Tay | Updated Part 1 user stories: AIBots UAT, SDT + HR/EduPass data pull |
| v0.2 | 2026-03-18 | Jasmine Tay | Added OPAL evidence, addressable market, priority & timeline, native resource viewer (Part 4) |
| v0.1 | 2026-03-17 | Jasmine Tay | Initial draft |

</details>

---

## Background

### Problem Statement

Teachers face high cognitive load daily. MOE has extensive domain-scoped learning and guidance materials to support them — but these materials fail teachers at the moment of need:

1. **Invisible at point of need** — Materials require active recall; teachers must already know a resource exists and where to find it
2. **Scattered across platforms** — Content lives across MOE Intranet, SharePoint, and other sites with poor search and browse experiences, leading to time sunk in information retrieval
3. **Text-heavy and hard to parse** — Guidance documents are lengthy and dense, making it difficult to extract key actionable points quickly (e.g., teachers spend significant time sifting through a 20-page student bullying intervention guide)

### Evidence of Problem

- 58.7% of users cannot find what they need on OPAL (n=90) [link](http://go.gov.sg/2026-0203-km)
- Search trends on OPAL across all keywords have dropped sharply since launch [link](https://gvt-external.slack.com/archives/C08CAM58T9U/p1740982877978059?thread_ts=1740712798.642099&cid=C08CAM58T9U)
- Teachers filter for relevance when engaging in PL [link](https://drive.google.com/drive/u/0/folders/1QxQsVP1sqjYvxLIPRSCSZLTJ1xDkLk27)
- Teachers want a path of least resistance to discovery [link](https://drive.google.com/drive/u/0/folders/1QxQsVP1sqjYvxLIPRSCSZLTJ1xDkLk27)

### Hypothesis of Root Causes

1. **No contextual delivery mechanism** — There is no system that proactively surfaces relevant guidance based on what a teacher is currently doing (e.g., handling a student wellbeing case)
2. **No intelligent synthesis layer** — Materials are served as-is; there is no AI layer that can extract key points, summarise guidance, and present it in a digestible format
3. **Fragmented information architecture** — Content is distributed across multiple platforms with no unified retrieval layer

---

## Why Now

- **TW ready in April 2026** — Teacher's Workspace is launching as the primary working tool for teachers, providing the ideal integration surface to contextually surface guidance
- **Available content from SDT** — Student development and wellbeing domains have structured materials ready to be ingested as the knowledge base
- **AI appetite on the ground is growing** — Teachers and school leaders are increasingly receptive to AI-assisted workflows; this is the right window to establish a trustworthy first experience
- **Google Cloud selected as AI infrastructure** — Vertex AI and Google Drive provide a scalable, sustainable foundation for RAG and knowledge management that can be extended across MOE products beyond CI

---

## Success

### North Star Metric

**Time saved in synthesis cost** — Reduction in time teachers spend locating materials + extracting information from materials. Measured via workstream mapping survey comparing pre- and post-intervention workflows.

### Product Metrics

| Metric | Definition | Target |
|--------|-----------|--------|
| CI card engagement | % of surfaced recommendation cards that teachers interact with (click/expand) | > 40% |
| CI recommendations CTR | Click-through rate on recommendations within cards | > 20% |
| Usefulness rating | # of teachers who report the recommendation as useful (thumbs up / feedback) | Track and grow |
| AI chat sessions per teacher | Frequency of teachers engaging with the chat interface after card interaction | TBD after pilot baseline |

### Guardrail Metrics

| Metric | Definition | Threshold |
|--------|-----------|-----------|
| Hallucination incidents | AI-generated content that is fabricated or not grounded in source materials | 0 |
| Source citation coverage | % of AI responses that include citations back to source documents | 100% |
| Response latency | Time for recommendation card or chat response to render | < 5s (p95) |
| Data classification breach | Sensitive High student data surfaced to a teacher without Sensitive High access | 0 |

---

## In Scope

### SDT Pilot within TW

- **Pilot domains:** Tier 2/3 Student Intervention and Student Wellbeing
- **Pilot audience:** Select group of teachers using Teacher's Workspace, moving to GA in Oct 2026
- **Domain owners:** SDCD (Student Development), GB (Student Wellness)
- **Addressable market:** All 33,000 General Education Officers (EOs) in MOE (post-pilot)

### Addressable Market

| Phase | Audience | Reach |
|-------|----------|-------|
| Phase 1 (Pilot) | All SDT team | Up to 3,300 |
| Phase 2 (Expansion) | + 15 pilot schools | Up to 5,000 |
| Phase 3 (GA — Sep/Oct 2026) | All teachers incl. independent schools | Up to 33,000 |

### Data Classification Constraints

SDT student data has two classification tiers. Teacher roles determine which tier they can access:

| Classification | Data included | Teacher access |
|---|---|---|
| Sensitive High | Counselling details, offence details | Role-restricted (e.g., counsellors, HODs) |
| Sensitive Normal | Number of offences | Broader teacher access |

> Glow CI must only use student data the accessing teacher is authorised to view. Context assembly and card surfacing must be scoped to the teacher's data access level as determined by the SDT API.

> **Google Drive data classification gap:** The current MOE Google Drive environment is classified Sensitive Low. Glow CI requires a dedicated GDrive folder cleared to **Official Closed (Sensitive Normal)** to store guidance materials associated with student context. A **Threat Risk Assessment (TRA)** must be completed before this folder can be used in production. This is a prerequisite for Phase 1.

### Out of Scope (this phase)

- Domains beyond Student Intervention and Student Wellbeing
- Automated actions (AI recommends, teacher decides)
- Content creation or editing of source materials
- No generative AI output

---

## Product Requirements

Glow CI consists of five interconnected parts:

---

### Part 1: Technical Integration — Contextual Data + RAG + LLM

**What it is:** A backend service with three integrated layers: (1) contextual data ingestion from teacher and student signals, (2) a knowledge base of MOE guidance materials indexed for semantic retrieval, and (3) a RAG + LLM layer that synthesises relevant guidance grounded in official materials.

**Key capabilities:**

**Contextual Data** — dynamic, per-session signals that shape what gets retrieved

- **Teacher context** (from EduPass/HR) — teacher role, school; used to scope relevant guidance
- **Student context** (from SDT API) — student data such as late-coming %, bullying offences count, low-mood count; used as the primary trigger for retrieval

*SDT API handles role-based filtering — only data the accessing teacher is authorised to view is returned. Data classification level (Sensitive High vs Sensitive Normal) must be respected in context assembly.*

**Knowledge Base** — guidance content that the AI retrieves from; managed by domain owners

- Guidance materials from Student Intervention and Student Wellbeing domains, sourced from a dedicated **Google Drive folder**
- Chunking and embedding pipeline to convert documents into a searchable vector store, using **Vertex AI** (embeddings + Vector Search)
- Document refresh cadence: re-ingestion process when domain owners update source materials

**AI Model** — retrieval and synthesis layer that turns contextual signals + knowledge base content into grounded guidance

- **Context assembly** — combines teacher + student context signals into a structured retrieval query; system prompt design owned by Glow/DXD
- **RAG orchestration** via Vertex AI — retrieves relevant document chunks based on assembled context
- **LLM synthesis** via Gemini — synthesises retrieved chunks into digestible guidance
- **Strict grounding** — all outputs must cite source documents; no unsupported claims

**Technical considerations:**

- AI infrastructure: **Google Cloud** — Vertex AI for embeddings, vector search, RAG orchestration, and Gemini LLM; Google Drive as the document source and knowledge management layer; chosen for scalability and reuse across MOE products
- Context assembly + system prompt design: owned by Glow/DXD
- TW rendering: owned by Glow/DXD
- Document refresh cadence: how often source materials are re-ingested from Google Drive
- **Open question** — confirm Vertex AI region / data residency satisfies MOE IT requirements
- API contract with TW for serving recommendation cards and chat responses
- Retrieval quality testing: ensure relevant chunks are retrieved for given contexts
- **SDT API data classification** — the SDT API handles role-based filtering and returns only the student data the accessing teacher is authorised to view; Glow CI must read and respect the data classification level returned (Sensitive High vs Sensitive Normal) and must not elevate access beyond what the API provides
- **Open question** — confirm with SDT PM which API fields/metadata indicate the teacher's data access tier, so it can be correctly parsed in context assembly

**User stories:**

| # | As a... | I want to... | So that... |
|---|---------|-------------|-----------|
| 1.1 | Engineer | set up and validate Vertex AI for RAG + LLM capabilities (embeddings, vector search, Gemini) | we validate the Google Cloud AI stack meets our retrieval and synthesis requirements before building on it |
| 1.2 | Engineer | pull student data from SDT | we can associate guidance recommendations with the right teacher context |
| 1.3 | Engineer | pull teacher data from HR/EduPass | we can use student profile signals to trigger contextually relevant guidance retrieval |

---

### Part 2: AI Chat Interface

**What it is:** A conversational interface within TW that allows teachers to ask natural-language questions and receive AI-synthesised responses grounded in MOE guidance materials. Opened from recommendation cards with first-cut recommendations pre-loaded.

**How it works:**

1. Teacher taps a recommendation card → AI Chat opens (slide-over panel or embedded)
2. First-cut recommendations are already presented in a digestible format (not raw document text)
3. Teacher can ask follow-up questions or add their unique context that only they know (e.g., "the student's parents are divorced and the child is acting out more at home")
4. AI synthesises further guidance, always citing source materials
5. Suggested follow-up questions guide exploration

**Key capabilities:**

- Pre-loaded context from the recommendation card that triggered the chat
- Natural-language Q&A over MOE guidance materials
- Teachers can add situational context to get more tailored guidance
- Inline source citations on every response
- Conversation starters / suggested prompts to lower barrier to first use
- Session-based conversation history

**Design requirements:**

- Chat should feel like a natural extension of the student page, not a separate tool
- Responses must be structured and scannable: bullet points, bold key actions, inline citations — never wall-of-text
- Clear positioning: "This is guidance, not SOP. You are responsible for making the final decision."
- Suggested follow-up questions after each response

**User stories:**

| # | As a... | I want to... | So that... |
|---|---------|-------------|-----------|
| 2.1 | Teacher | open a chat pre-loaded with relevant guidance from the card I tapped | I don't have to re-explain my context |
| 2.2 | Teacher | ask follow-up questions in natural language | I can explore guidance relevant to my specific situation |
| 2.3 | Teacher | add my own context (e.g., family situation, past interventions tried) | the AI can tailor its recommendations beyond what's in the standard materials |
| 2.4 | Teacher | see source citations on every AI response | I can verify the guidance and refer to the original document if needed |
| 2.5 | Designer | design a chat experience embedded in TW that feels native | teachers adopt it as part of their natural workflow |

---

### Part 3: Recommendation Card Surfacing in TW Student Page

**What it is:** Contextual recommendation cards that appear on a student's page in Teacher's Workspace, proactively surfacing just-in-time (JIT) learning and intervention guidance based on the teacher's current context.

**How it works:**

1. Teacher navigates to a student's page in TW
2. Context signals (e.g., student profile data, case notes, flags) trigger Glow CI to retrieve relevant guidance
3. 2–4 recommendation cards are surfaced, each containing a concise, digestible summary of relevant guidance
4. Tapping a card opens the AI Chat Interface (Part 2) with first-cut recommendations already presented

**Card anatomy:**

- **Headline:** Concise insight or recommendation (e.g., "Tier 2 Intervention: Peer mediation strategies for recurring conflict")
- **Summary:** 2–3 sentence digest of relevant guidance, extracted from source materials
- **Source citation:** Link/reference to the original MOE document
- **CTA:** Opens AI Chat for deeper exploration

**Design requirements:**

- Cards must be lightweight and glanceable — teachers are time-poor
- Visual hierarchy: headline > summary > source > CTA
- Integrate naturally into TW's existing student page layout (coordinate with TW team)
- Handle loading, empty state (no relevant guidance to surface), and error states gracefully
- Cards should feel proactive but not intrusive — they are suggestions, not mandates

**User stories:**

| # | As a... | I want to... | So that... |
|---|---------|-------------|-----------|
| 3.1 | Teacher | see contextually relevant recommendation cards when I view a student's page | I get just-in-time guidance without searching for it myself |
| 3.2 | Teacher | see a concise summary on each card with a link to the source | I can quickly assess relevance and trust the recommendation |
| 3.3 | Teacher | tap a card to explore deeper via AI Chat | I can get more detailed, tailored guidance when I need it |
| 3.4 | Designer | design cards that integrate into TW's student page without disrupting existing layout | the experience feels native and non-intrusive |

---

### Part 4: Analytics & Tracking

**What it is:** The instrumentation layer that enables measurement of product metrics and guardrail metrics. Glow CI uses a dedicated GA4 property (separate from TW), supplemented with custom events and server-side logging.

**Standard GA4 (no custom instrumentation needed):**
- Sessions, users, DAU/WAU
- Traffic sources, device/browser breakdown
- Page-level engagement time

**Custom events required:**

| Event | Trigger | Maps to metric |
|-------|---------|----------------|
| `ci_card_impression` | CI cards rendered on student page | CI card engagement (denominator) |
| `ci_card_click` | Teacher clicks/expands a card | CI card engagement |
| `ci_card_cta_click` | Teacher taps "Open chat" CTA on a card | CI recommendations CTR |
| `ci_usefulness_rating` | Teacher submits thumbs up/down on a card or chat response | Usefulness rating |
| `ci_chat_session_start` | AI Chat opens (from card or standalone) | AI chat sessions per teacher |
| `ci_chat_message_sent` | Teacher sends a follow-up message in chat | Chat depth signal |
| `ci_citation_click` | Teacher taps a source citation link | Citation engagement |
| `ci_resource_viewer_open` | Document opens in native resource viewer | Resource viewer adoption |

**Server-side logging (outside GA — guardrail metrics):**
These cannot be tracked in GA and require server/API-level logging:
- **Response latency** — logged at API level (request → render time)
- **Source citation coverage** — tracked at LLM response generation level
- **Hallucination incidents** — flagged via teacher feedback + manual review process
- **Data classification breach** — logged at SDT API integration layer

---

---

### Part 5: Knowledge Storage & Retrieval — Native Resource Viewer in TW

**What it is:** A view-only inline document viewer within Teacher's Workspace that lets teachers open and read MOE guidance materials natively — without being redirected to external sites (MOE Intranet, SharePoint, etc.). In this phase, resources are accessed via recommendation cards (Part 3) and AI Chat citations (Part 2), not through standalone browsing or search.

**How it works:**

1. Teacher taps a source citation in a recommendation card or AI Chat response
2. The referenced MOE guidance document opens in a native inline viewer within TW
3. Where possible, the viewer scrolls to or highlights the relevant section cited
4. Teacher reads the full source material without leaving TW

**Key capabilities:**

- Native document viewer within TW (PDFs, Word docs, web content) — teachers never leave the platform
- Deep-linking from recommendation cards (Part 3) and AI Chat citations (Part 2) directly into the viewer
- Section-level anchoring: viewer opens at the relevant section where possible
- Shared storage with RAG pipeline (Part 1) — materials are ingested once, served for both AI retrieval and native viewing

**Design requirements:**

- Viewer must feel native to TW — not a raw iframe to an external site
- Support for key document formats: PDF, Word, HTML
- Loading states for document rendering; graceful fallback if a document can't be rendered inline (e.g., download option)
- Seamless handoff: tapping a source citation in AI Chat → opens resource at the relevant section
- Clear "back" navigation to return to the student page / chat

**Technical considerations:**

- Document storage: materials are stored in a dedicated Google Drive folder (cleared to Official Closed via TRA — see Data Classification Constraints)
- Format handling: rendering pipeline for PDF, Word, and HTML content
- Deep-linking / anchor support: ability to link to specific sections within a document
- Sync with RAG pipeline (Part 1): the same ingested materials serve both the native viewer and the RAG vector store
- Access control: ensure only authorised teachers can access domain-specific materials
- **Open question — viewer approach:** Source documents now live in Google Drive. Evaluate whether Google Drive's native document preview (via Drive Viewer API or iframe) can serve as the inline viewer within TW, reducing custom build effort, vs. building a fully custom TW-native viewer. Decision needed before Phase 2.

**User stories:**

| # | As a... | I want to... | So that... |
|---|---------|-------------|-----------|
| 5.1 | Teacher | open a document natively within TW when I tap a source citation | I can read the full guidance without being redirected to an external site |
| 5.2 | Teacher | land on the relevant section of the document when opening from a citation | I don't have to scroll through the entire document to find what was referenced |
| 5.3 | Designer | design a native document viewer that integrates into TW's existing UX | the experience feels like a core part of the platform, not a bolt-on |
| 5.4 | Engineer | sync document storage with the RAG ingestion pipeline | materials are stored once and serve both native viewing and AI retrieval |

**Future phases (out of scope now):** Standalone browse/search, bookmarking, version tracking

## Priority & Timeline

**Pilot launch target:** 31 Aug 2026 | **GA target:** Oct 2026

### Delivery priority

Chat-first approach — the AI Chat interface is the primary value driver and ships first. Recommendation cards follow as a contextual discovery layer once chat is proven.

| Priority | Part | Rationale |
|----------|------|-----------|
| **P0** | Part 1: Technical Integration — Contextual Data + RAG + LLM | Foundation — everything depends on the retrieval and AI backend |
| **P0** | Part 2: AI Chat Interface | Primary user-facing surface; delivers the core value of synthesised guidance |
| **P1** | Part 3: Recommendation Cards | Contextual discovery layer; adds proactive surfacing once chat is validated |
| **P1** | Part 4: Analytics & Tracking | Required for pilot baseline measurement — must be live before 31 Aug |
| **P2** | Part 5: Native Resource Viewer | Completes the citation loop — teachers can verify source materials without leaving TW |
### Timeline

| Phase | Dates | Milestone | What ships |
|-------|-------|-----------|-----------|
| **Phase 1 — Foundation** | Apr – May 2026 | RAG pipeline operational | Part 1: Document ingestion, vector store, RAG orchestration, LLM integration. End-to-end pipeline tested with pilot domain materials |
| **Phase 2 — Chat MVP** | May – Jul 2026 | Internal dogfood ready | Part 2: AI Chat interface integrated in TW. Teachers can ask questions and receive cited, grounded responses. Part 5: View-only resource viewer for source citations |
| **Phase 3 — Pilot launch** | Aug 2026 | **Pilot launch (31 Aug)** | Parts 1 + 2 + 4 + 5 live with select pilot teachers. GA4 custom events instrumented. Baseline metrics collection begins |
| **Phase 4 — Cards + iteration** | Sep 2026 | GA readiness | Part 3: Recommendation cards surfaced on student page. Iteration based on pilot feedback. GA launch (Oct 2026) |

### Key dependencies

- TW platform launch (Apr 2026) — integration surface must be available
- SDT domain materials handoff — pilot content must be ingested before Phase 2
- Google Cloud access — Vertex AI project setup, service account provisioning, and Gemini model access
- **Google Drive TRA** — dedicated folder must be cleared to Official Closed (Sensitive Normal) before document ingestion can begin; initiate early as a Phase 1 prerequisite
- TW API contracts — agreed integration points for embedding chat, cards, and viewer

---

## Key Risks and Mitigations

| Risk | Mitigation |
|------|-----------|
| Retrieval quality — wrong or irrelevant guidance surfaced | Rigorous retrieval testing; tuning chunk size, embedding model, and similarity thresholds |
| Hallucination — AI fabricates guidance not in source materials | Strict RAG grounding; mandatory source citations; zero-tolerance guardrail metric |
| Teacher over-reliance on AI recommendations | Clear positioning: "Guidance, not SOP — teachers make the final decision." Disclaimer in UI |
| Source material staleness | Defined refresh cadence; process for domain owners to flag updated materials |
| Google Drive data classification | Current GDrive is Sensitive Low; TRA required to clear a dedicated folder to Official Closed (Sensitive Normal). Ingestion cannot begin until TRA is approved. | Initiate TRA in parallel with Phase 1 setup; treat as a hard blocker |

---

## Team Roles

### Core Product Team

| Name | Role | Capacity | Notes |
|------|------|----------|-------|
| Jasmine | Product Manager | 0.3 | Also handling MK, Glow |
| Ivan | Tech Lead | 0.2 | Also handling Glow, COVAA, Edge Search, other ad-hoc vibecoding projects |
| Nicholas | Engineer | 0.5 | Also handling Glow, COVAA |
| Ralph | Engineer | 0.5 | Also handling Glow, COVAA |
| Wondo | UI/UX Designer | 0.1 | |
| Rebecca | UI/UX Design Intern | TBD | Incoming 11 May – 30 Sep 2026 |
| TBD | AI Engineer | TBD | Role to be filled |

### Collaborative Products

**SDT** — provides student contextual data

| Name | Role |
|------|------|
| Xingyu | TW Students Product Manager |
| Eileen | TW Students Tech Lead |
| Layhui | TW Students Designer |

**SEConnect** — subject matter expert for student wellbeing domain; supplies socio-emotional data to SDT

| Name | Role |
|------|------|
| Eric | SEConnect / TW Wellbeing — Product Manager |
| Claire | SEConnect / TW Wellbeing — Designer |

### Business Stakeholders

| Organisation | Role |
|-------------|------|
| Guidance Branch (GB) | Professional partner on student intervention knowledge |
| West Zone Sups | Owner of existing SDT AI bot with organic adoption in West Zone; subject matter expert on pilot approach |

