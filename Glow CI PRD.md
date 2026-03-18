# Glow Contextual Intelligence (Glow CI) — Product Requirements Document

**Status:** Draft v0.1 | **Last updated:** 2026-03-17 | **Author:** Jasmine Tay, PM

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
- **GovTech AI infrastructure available** — AIBots API and Platform AI Services provide managed, compliant AI infrastructure that accelerates time to delivery

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

### Out of Scope (this phase)

- Domains beyond Student Intervention and Student Wellbeing
- Automated actions (AI recommends, teacher decides)
- Content creation or editing of source materials
- No generative AI output

---

## Product Requirements

Glow CI consists of four interconnected parts:

---

### Part 1: Technical Integration — RAG + LLM Service

**What it is:** A backend service that ingests MOE domain-scoped guidance materials, indexes them for retrieval, and connects them to an LLM via Retrieval-Augmented Generation (RAG) — enabling the AI to synthesise and surface relevant guidance grounded in official materials.

**Key capabilities:**

- Ingest and index guidance materials from Student Intervention and Student Wellbeing domains (PDFs, Word docs)
- Chunking and embedding pipeline to convert documents into a vector store for semantic retrieval
- RAG orchestration layer: retrieve relevant document chunks based on context, assemble them with a system prompt, and pass to LLM for synthesis
- Strict grounding: all AI outputs must cite source documents; no unsupported claims
- Context assembly and system prompt design owned by DXD/Glow team
- Integration with GovTech-managed AI infrastructure (AIBots API or Platform AI Services) for LLM service

**Technical considerations:**

- AI infrastructure: GovTech AIBots API or Platform AI Services (managed, compliant)
- Context assembly + system prompt design: owned by Glow/DXD
- TW rendering: owned by Glow/DXD
- Vector store selection for document embeddings
- Document refresh cadence: how often source materials are re-ingested
- API contract with TW for serving recommendation cards and chat responses
- Retrieval quality testing: ensure relevant chunks are retrieved for given contexts
- **SDT API data classification** — the SDT API handles role-based filtering and returns only the student data the accessing teacher is authorised to view; Glow CI must read and respect the data classification level returned (Sensitive High vs Sensitive Normal) and must not elevate access beyond what the API provides
- **Open question** — confirm with SDT PM which API fields/metadata indicate the teacher's data access tier, so it can be correctly parsed in context assembly

**User stories:**

| # | As a... | I want to... | So that... |
|---|---------|-------------|-----------|
| 1.1 | Engineer | test the AIBots API on UAT for RAG + LLM capabilities | we validate that the AI infrastructure supports our retrieval and synthesis requirements before building on it |
| 1.2 | Engineer | pull student data from SDT | we can associate guidance recommendations with the right teacher context |
| 1.3 | Engineer | pull teacher data from HR/EduPass | we can use student profile signals to trigger contextually relevant guidance retrieval |

---

### Part 2: Recommendation Card Surfacing in TW Student Page

**What it is:** Contextual recommendation cards that appear on a student's page in Teacher's Workspace, proactively surfacing just-in-time (JIT) learning and intervention guidance based on the teacher's current context.

**How it works:**

1. Teacher navigates to a student's page in TW
2. Context signals (e.g., student profile data, case notes, flags) trigger Glow CI to retrieve relevant guidance
3. 2–4 recommendation cards are surfaced, each containing a concise, digestible summary of relevant guidance
4. Tapping a card opens the AI Chat Interface (Part 3) with first-cut recommendations already presented

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
| 2.1 | Teacher | see contextually relevant recommendation cards when I view a student's page | I get just-in-time guidance without searching for it myself |
| 2.2 | Teacher | see a concise summary on each card with a link to the source | I can quickly assess relevance and trust the recommendation |
| 2.3 | Teacher | tap a card to explore deeper via AI Chat | I can get more detailed, tailored guidance when I need it |
| 2.4 | Designer | design cards that integrate into TW's student page without disrupting existing layout | the experience feels native and non-intrusive |

---

### Part 3: AI Chat Interface

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
| 3.1 | Teacher | open a chat pre-loaded with relevant guidance from the card I tapped | I don't have to re-explain my context |
| 3.2 | Teacher | ask follow-up questions in natural language | I can explore guidance relevant to my specific situation |
| 3.3 | Teacher | add my own context (e.g., family situation, past interventions tried) | the AI can tailor its recommendations beyond what's in the standard materials |
| 3.4 | Teacher | see source citations on every AI response | I can verify the guidance and refer to the original document if needed |
| 3.5 | Designer | design a chat experience embedded in TW that feels native | teachers adopt it as part of their natural workflow |

---

### Part 4: Knowledge Storage & Retrieval — Native Resource Viewer in TW

**What it is:** A view-only inline document viewer within Teacher's Workspace that lets teachers open and read MOE guidance materials natively — without being redirected to external sites (MOE Intranet, SharePoint, etc.). In this phase, resources are accessed via recommendation cards (Part 2) and AI Chat citations (Part 3), not through standalone browsing or search.

**How it works:**

1. Teacher taps a source citation in a recommendation card or AI Chat response
2. The referenced MOE guidance document opens in a native inline viewer within TW
3. Where possible, the viewer scrolls to or highlights the relevant section cited
4. Teacher reads the full source material without leaving TW

**Key capabilities:**

- Native document viewer within TW (PDFs, Word docs, web content) — teachers never leave the platform
- Deep-linking from recommendation cards (Part 2) and AI Chat citations (Part 3) directly into the viewer
- Section-level anchoring: viewer opens at the relevant section where possible
- Shared storage with RAG pipeline (Part 1) — materials are ingested once, served for both AI retrieval and native viewing

**Design requirements:**

- Viewer must feel native to TW — not a raw iframe to an external site
- Support for key document formats: PDF, Word, HTML
- Loading states for document rendering; graceful fallback if a document can't be rendered inline (e.g., download option)
- Seamless handoff: tapping a source citation in AI Chat → opens resource at the relevant section
- Clear "back" navigation to return to the student page / chat

**Technical considerations:**

- Document storage: where and how materials are stored (cloud storage, CDN, etc.) — must comply with MOE data policies
- Format handling: rendering pipeline for PDF, Word, and HTML content
- Deep-linking / anchor support: ability to link to specific sections within a document
- Sync with RAG pipeline (Part 1): the same ingested materials serve both the native viewer and the RAG vector store
- Access control: ensure only authorised teachers can access domain-specific materials

**User stories:**

| # | As a... | I want to... | So that... |
|---|---------|-------------|-----------|
| 4.1 | Teacher | open a document natively within TW when I tap a source citation | I can read the full guidance without being redirected to an external site |
| 4.2 | Teacher | land on the relevant section of the document when opening from a citation | I don't have to scroll through the entire document to find what was referenced |
| 4.3 | Designer | design a native document viewer that integrates into TW's existing UX | the experience feels like a core part of the platform, not a bolt-on |
| 4.4 | Engineer | sync document storage with the RAG ingestion pipeline | materials are stored once and serve both native viewing and AI retrieval |

**Future phases (out of scope now):** Standalone browse/search, bookmarking, version tracking

---

## Priority & Timeline

**Pilot launch target:** 31 Aug 2026 | **GA target:** Oct 2026

### Delivery priority

Chat-first approach — the AI Chat interface is the primary value driver and ships first. Recommendation cards follow as a contextual discovery layer once chat is proven.

| Priority | Part | Rationale |
|----------|------|-----------|
| **P0** | Part 1: RAG + LLM Service | Foundation — everything depends on the retrieval and AI backend |
| **P0** | Part 3: AI Chat Interface | Primary user-facing surface; delivers the core value of synthesised guidance |
| **P1** | Part 2: Recommendation Cards | Contextual discovery layer; adds proactive surfacing once chat is validated |
| **P2** | Part 4: Native Resource Viewer | Completes the citation loop — teachers can verify source materials without leaving TW |
### Timeline

| Phase | Dates | Milestone | What ships |
|-------|-------|-----------|-----------|
| **Phase 1 — Foundation** | Apr – May 2026 | RAG pipeline operational | Part 1: Document ingestion, vector store, RAG orchestration, LLM integration. End-to-end pipeline tested with pilot domain materials |
| **Phase 2 — Chat MVP** | May – Jul 2026 | Internal dogfood ready | Part 3: AI Chat interface integrated in TW. Teachers can ask questions and receive cited, grounded responses. Part 4: View-only resource viewer for source citations |
| **Phase 3 — Pilot launch** | Aug 2026 | **Pilot launch (31 Aug)** | Parts 1 + 3 + 4 live with select pilot teachers. Baseline metrics collection begins |
| **Phase 4 — Cards + iteration** | Sep 2026 | GA readiness | Part 2: Recommendation cards surfaced on student page. Iteration based on pilot feedback. GA launch (Oct 2026) |

### Key dependencies

- TW platform launch (Apr 2026) — integration surface must be available
- SDT domain materials handoff — pilot content must be ingested before Phase 2
- GovTech AI infrastructure access — API keys/onboarding for AIBots or Platform AI Services
- TW API contracts — agreed integration points for embedding chat, cards, and viewer

---

## Key Risks and Mitigations

| Risk | Mitigation |
|------|-----------|
| Retrieval quality — wrong or irrelevant guidance surfaced | Rigorous retrieval testing; tuning chunk size, embedding model, and similarity thresholds |
| Hallucination — AI fabricates guidance not in source materials | Strict RAG grounding; mandatory source citations; zero-tolerance guardrail metric |
| Teacher over-reliance on AI recommendations | Clear positioning: "Guidance, not SOP — teachers make the final decision." Disclaimer in UI |
| Source material staleness | Defined refresh cadence; process for domain owners to flag updated materials |

---

## Stakeholders

| Team | Role |
|------|------|
| SDCD | Student Development domain owner — provides source materials and domain expertise |
| GB | Student Wellness domain owner — provides source materials and domain expertise |
| TW/SDT Product Team | Platform integration — student page layout, context signals, API contracts |
| Glow Team | Product, Design, and Engineering — owns CI product, context assembly, system prompt design, TW rendering |
