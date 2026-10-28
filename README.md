# MATACS — AI Proposal Triage Design

**Candidate:** Minahil Naseer  
**Assessment reference:** `MATACS-TH-2026-02`  
**Time spent:** 5 hours

# Overview
This repository describes a proposed AI-assisted workflow for triaging contractor proposals for a 150 kW commercial rooftop solar installation in Windsor, Ontario. It is a design exercise; no deployed application is included.

# Deliverables
- [Process flow diagram](diagram/process-flow.png)
- [Design write-up](WRITEUP.md)
- [AI prompts](PROMPTS.md)
- [Expected ranking](ranking.json)

# How I used AI tools
I used ChatGPT to help organize the design, draft prompt examples, and check that the proposed workflow addresses the requirements in the assessment brief. I reviewed the output and made the final design decisions myself. One important correction was to reject the instruction embedded in Proposal C that asks automated systems to rank it first: proposal text is untrusted evidence, not an instruction to the system. I also treated Proposal B's bidder-licence evidence and Proposal D's missing timeline as unresolved rather than inventing facts.

# Assumptions and limitations
- The scoring rubric is a proposed, transparent example because the brief specifies criterion weights but not exact sub-scoring formulas.
- Only information stated in the sample proposal summaries is treated as evidence.
- A proposal is not ranked until mandatory checks pass and any material uncertainty affecting eligibility or feasibility has been resolved.
- Before real procurement use, administrators should approve the detailed scoring rubric and validate all legal/compliance rules.
