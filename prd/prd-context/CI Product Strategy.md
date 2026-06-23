# CI Product Strategy

## 💡 What is CI?

CI is the secure Intelligence Layer of our platform — the Educator Productivity Intelligence Gateway.

Instead of making teachers go looking for information, CI weaves intelligence directly into the core workflows they already use. It operates behind the scenes within Teacher's Workspace to route requests, evaluate data triggers, run AI Assist, retrieve contextual guidance via RAG (Retrieval-Augmented Generation), and compose multi-system responses.

---

## 🏫 The AI & Teaching Landscape

**High AI Appetite:** According to the OECD TALIS Survey, 3 in 4 Singapore teachers use AI to teach or support learning — more than double the global average of 36%.

**Existing Ecosystem:** All MOE teachers have Google Workspace accounts (giving them native access to Gemini) and actively use government-whitelisted utilities like Pair Chat.

**Top Use Cases:** They primarily rely on AI for topic learning, parent-facing brainstorming, drafting administrative text, and lesson planning.

---

## 🔑 What problem is this solving?

Teachers are dealing with severe administrative burnout, but existing AI tools cannot easily alleviate it due to strict structural barriers:

**The Administrative Burden:** Singapore teachers work an average of 47.3 hours a week. A massive portion of this time is swallowed by heavy administrative tasks: tracking student interventions, reading dense policy manuals, compiling case notes, sifting through data spreadsheets, and drafting parent communications. AI has the potential to automate much of this, but current setups block it.

**System Lock-In & Security:** Because of strict data security policies, teachers must use internal MOE systems for their core admin and student workflows. They are cut off from using public or external AI tools for day-to-day heavy lifting.

**The "Context Transfer" Tax:** When teachers try to use permitted external tools (like Pair Chat), they must exert massive manual effort to strip out, scrub, and transfer context back and forth between internal platforms and external chat boxes.

**Strict Policy Boundaries:** The MOE data policy for external chatbots strictly limits usage to Official (Open) / Non-Sensitive data only. Teachers cannot legally feed student files, internal operational guidelines, or case notes into external interfaces.

**The "Verification Tax":** Because generic public models hallucinate, teachers cannot fully trust AI-generated outputs without cross-referencing them against actual, official policy — adding overhead that partially erodes the time savings.

---

## ⚡ Why is this worth solving?

By building CI natively inside the secure Teacher's Workspace ecosystem, we eliminate this friction entirely. We pull the intelligence directly to where the secure data already lives. This removes the "context transfer tax," safely enforces data boundaries, and dramatically lowers the verification tax by using grounded, official data — allowing teachers to finally reclaim actual time.

---

## 🏆 Success

Our ultimate North Star metric is measurable time saved for educators. We evaluate success by how effectively our platform capabilities shrink their weekly administrative backlog:

**Knowledge Retrieval:** Saves time in discovery and information synthesis. It removes the high friction of finding the right document and scanning chunky, dense materials by delivering grounded, accurate answers at the point of need.

**Drafting Assistance:** Saves time in context re-composition and cold-start friction. It eliminates "blank-page syndrome" and removes the need to manually sanitize text by securely pre-filling internal institutional context so teachers only have to review, edit, and sign off.

**Insights Summary:** Saves time in data sifting and pattern recognition. It cuts out hours spent manually analysing disjointed spreadsheets and files by securely flagging student or cohort trends.

---

## 🎯 Our Prioritization Framework

We are in our infancy and cannot prioritize every JTBD under the sun. To protect our bandwidth, every proposed feature is aggressively filtered through four lenses:

**1. Magnitude of Time Saved:** Does the solution fundamentally move the needle on a teacher's weekly administrative load?

**2. Exclusivity of TW/CI:** Can this time-saving job only be fulfilled natively inside Teacher's Workspace? If a teacher can easily fulfil the task using external tools like Pair Chat or Gemini without facing security boundaries or heavy context-transfer friction, we do not build it.

**3. AI Performance Baseline:** We only deploy AI where it explicitly outperforms traditional UX. A well-designed data dashboard will always beat a clumsy, expensive AI summary if it helps a teacher understand student insights faster and better. Not every workflow needs AI.

**4. TW Platform Readiness:** Is the relevant TW app or workflow surface built and available for CI to plug into? CI is an intelligence layer, not a standalone product — it depends on TW apps having the right data hooks and integration surfaces. If the underlying TW app doesn't exist yet, CI cannot be embedded there regardless of use case merit.

---

## 👥 Audience

Singapore educators and teachers operating within the Teacher's Workspace (TW) ecosystem. They are highly active, AI-literate users — 3 in 4 already use AI in their teaching — who expect tools that are secure, accurate, and embedded in their existing workflows rather than bolted on separately.

---

## 🛠️ How are we going to build it?

**Shared Infrastructure:** CI is built as a shared platform capability layer — shared RAG and model services power context-aware AI utilities natively across TW apps. Build once, serve many.

**Embedded Guardrails:** Compliance is non-negotiable. Data classification, Sensitive-High masking, output grounding, and automated auditing are native to the core infrastructure — not bolted on as an afterthought.

---

## 🚀 Where we're starting: Pilot Scope

Our first JTBD in production is **Knowledge Retrieval** — surfacing the right guidance at the moment a teacher needs it, grounded in official MOE materials, delivered in the flow of work without requiring a separate search.

**Pilot use case: SwAN student intervention support** within the SDT student profile page in TW. When a student's signals (long-term absenteeism, SEN, or offence) are detected, CI surfaces contextually relevant guidance to the teacher at the point of need — without requiring them to search for it. Teacher queries are answered by an AI chat interface grounded against the SwAN knowledge base via RAG, with source citations on every response.

This pilot scores highly on all four prioritization lenses:
- **Time savings** — teachers currently search across dense, fragmented policy materials for intervention guidance
- **TW/CI exclusivity** — student signal data cannot leave internal systems; an embedded solution is the only viable path
- **AI outperforms static UX** — nuanced, student-specific guidance retrieval cannot be adequately served by a document library or dashboard
- **TW platform readiness** — the SDT student profile page already exists as the integration surface
