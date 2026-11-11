> From the OWASP GenAI Security Collection (OWASP GenAI Security Project, https://genai.owasp.org/). Licensed CC BY-SA 4.0 (https://creativecommons.org/licenses/by-sa/4.0/). Split from the combined Markdown edition into one file per document; text otherwise unchanged.

### GenAI Data Security Best Practices Part 1

**Original PDF:** [GenAi-Data-Securty-Best-Practices-Part-1.pdf](https://chatgpt.com/library?file_id=libfile_993ab55d56fc819182e1ef4cd97d19f2&account_id=personal) · **45 pages**

### Data security page 1

GENAI SECURITY PROJECT

GenAI Data Security

Best Practices

Version 2.0 – PART 1

October 2026

### Data security page 2

##### License and Usage

This document is licensed under Creative Commons, CC BY-SA 4.0.

You are free to:

- Share — copy and redistribute the material in any medium or format for any purpose, even commercially.

- Adapt — remix, transform, and build upon the material for any purpose, even commercially.

The licensor cannot revoke these freedoms as long as you follow the license terms.

Under the following terms:

- Attribution — You must give appropriate credit, provide a link to the license, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.

- ShareAlike — If you remix, transform, or build upon the material, you must distribute your contributions under the same license as the original.

- No additional restrictions — You may not apply legal terms or technological measures that legally restrict others from doing anything the license permits.

Link to full license text: <https://creativecommons.org/licenses/by-sa/4.0/legalcode>

The information provided in this document does not, and is not intended to constitute legal advice. All information is for general informational purposes only. This document contains links to other third-party websites. Such links are only for convenience and OWASP does not recommend or endorse the contents of the third-party sites.

### Data security page 3

#### Table of Content

- [About This Series — How the Parts Fit Together](#data-security-page-5) · 5
- [What You Are Holding](#data-security-page-5) · 5
- [The Foundation Every Part Assumes](#data-security-page-5) · 5
- [The Series at a Glance](#data-security-page-6) · 6
- [How the Parts Depend on Each Other](#data-security-page-8) · 8
- [How to Use This Part](#data-security-page-9) · 9
- [Why Version 2?](#data-security-page-10) · 10
- [Chapter Format](#data-security-page-13) · 13
- [Chapter 1 — Introduction & Scope](#data-security-page-14) · 14
- [Chapter 2 — GenAI Data Security Fundamentals](#data-security-page-20) · 20
- [Chapter 3 — AI Data Security Posture Management (AI-DSPM)](#data-security-page-26) · 26
- [Acknowledgements](#data-security-page-42) · 42
- [Authors](#data-security-page-42) · 42
- [Contributors](#data-security-page-42) · 42
- [Reviewers](#data-security-page-43) · 43

### Data security page 4

- [OWASP GenAI Security Project Sponsors](#data-security-page-44) · 44
- [Project Supporters](#data-security-page-45) · 45

### Data security page 5

You are reading Part 1 — Foundation, Scope & AI-DSPM, covering Chapters 1–3.

#### About This Series — How the Parts Fit Together

This chapter is repeated at the front of every part of the series. If you have already read it in another part, skip to the part's first chapter.

##### What You Are Holding

GenAI Data Security Best Practices v2 is the implementation playbook that accompanies OWASP GenAI Data Security Risks and Mitigations 2026. The companion document enumerates the 21 data-security risks specific to GenAI systems (DSGAI01–DSGAI21) and answers what can go wrong; this guide answers what to actually do about it — architectural patterns, tiered controls, tooling references, and compliance mappings for every team building, deploying, or operating a GenAI system.

Because the full guide runs to more than 400 pages, it is published as a twelve-part series. Each part is self-contained enough to be read on its own, follows the same chapter format, and carries this orientation chapter plus the shared author, contributor, and reviewer credits. The parts are not twelve independent papers: they are one architecture, split for readability, and the controls in any one part assume the substrate established in the parts before it.

##### The Foundation Every Part Assumes

Part 1 (Foundation, Scope & AI-DSPM) is the root of the series and the recommended starting point regardless of which part brought you here. It establishes three things every subsequent part builds on:

1. The scope and conventions (Chapter 1) — the relationship to the companion risk document, the DSGAI risk-reference convention, the Tier 1 / Tier 2 / Tier 3 maturity semantics (Foundational → Hardening → Advanced), and the Build / Buy scope tags used in every chapter.

2. The fundamentals (Chapter 2) — the GenAI data surface, the context window as a trust-domain fusion problem, and the six core principles (minimization, isolation, lifecycle rigor, integrity, continuous monitoring, zero trust in the model) that every pattern in Parts 2–12 instantiates.

### Data security page 6

3. The operating discipline (Chapter 3) — AI-DSPM, the continuous inventory, classification, and posture-management program that tells every downstream control what exists and where it lives. The patterns in Parts 2–12 assume this inventory and act on it.

If you read nothing else before your part of interest, read those three chapters. Everything else in the series is written against them.

##### The Series at a Glance

| Part | Title | Chapters | What it covers | Builds on |
| --- | --- | --- | --- | --- |
| 1 | Foundation, Scope & AI-DSPM | Front matter + Ch 1–3 | Scope, fundamentals, and the AI- DSPM operating discipline | — (root of the series) |
| 2 | Data Governance & Lifecycle | Ch 4 | Classification, metadata envelopes, retention, erasure, lineage / AI-BOM — the governance substrate | Part 1 (inventory, principles) |
| 3 | Training Pipelines, RAG, Memory & Vector Stores | Ch 5–7 | Securing what goes into models and retrieval systems: training and fine- tuning, RAG and memory, vector stores and embeddings | Parts 1–2 (classification and lineage feed every pattern here) |
| 4 | Agentic AI, Tools, Plugins & MCP | Ch 8–9 | Securing what AI systems do: agent identity, delegation, memory isolation, HITL checkpoints, tool and MCP connector security | Parts 1–3 (agents consume the planes secured in Part 3) |
| 5 | Prompt, Context & Output Security | Ch 10–11 | The runtime perimeter as a pair: input composition and injection defense in; sensitive-content filtering, grounding, and DLP out | Parts 1–4 (context windows carry retrieved, tool, and agent content) |
| 6 | AI Identity & Shadow AI Governance | Ch 12–13 | The identity plane: NHI lifecycle, JIT credentials, delegation — plus discovery of the AI the organization doesn't know it has | Parts 1, 4 (supplies the identity substrate agents assume) |

### Data security page 7

| Part | Title | Chapters | What it covers | Builds on |
| --- | --- | --- | --- | --- |
| 7 | Observability Posture & Multimodal Leakage | Ch 14–15 | Making telemetry a governed capability rather than a leakage surface; multimodal capture and cross-channel leakage | Parts 1–6 (every prior control emits telemetry this part governs) |
| 8 | Natural-Language Data Gateways & AI Supply Chain | Ch 16–17 | LLM-to-SQL/Cypher/Gremlin gateway security; artifact provenance, registries, SBOM/DBOM, and the AI software supply chain | Parts 1–3 (extends Part 3's artifact signing into an organizational program) |
| 9 | Deployment Topologies & Privacy-Enhancing Technologies | Ch 18–19 | Where GenAI runs (managed API, self- hosted, hybrid, edge, sovereign) and the privacy primitives (DP, federated, confidential computing) | Parts 1–2 (topology and PET selection follow classification and residency) |
| 10 | Inference Defense, Incident Response & Resilience | Ch 20–21 | Defending the model as a data asset (MIA, inversion, extraction) and responding when controls fail (detection, scoping, containment, recovery) | Parts 1–9 (IR correlates signals from nearly every prior part) |
| 11 | Regulatory Compliance, Governance & Human Oversight | Ch 22–23 | The compliance program (EU AI Act, GDPR, NIST AI RMF, ISO 42001) plus the human-in-the-loop and labeler data-security discipline | Parts 1–10 (consolidates the per-chapter compliance mappings) |
| 12 | Quantum Readiness & Closing Materials | Ch 24 + back matter | Post-quantum migration for the cryptography under every prior part, plus the DSGAI Risk Coverage Matrix, glossary, references, and credits | All parts (the cryptographic foundation beneath the whole series) |

### Data security page 8

##### How the Parts Depend on Each Other

The series follows data through a GenAI system, and the dependencies follow the data — each part below builds on the ones above it:

| Part 1 | Foundation & AI-DSPM — the root |
| --- | --- |
| Part 2 | Governance & Lifecycle — the substrate: classification, lineage, erasure |
| Part 3 | Training, RAG, Vector Stores — what goes INTO models and indexes |
| Part 4 | Agents, Tools, MCP — what AI systems DO with data |
| Part 5 | Prompt & Output — the runtime perimeter, in and out |
| Part 6 | Identity & Shadow AI — the identity plane and the ungoverned estate |
| Part 7 | Observability & Multimodal — the telemetry every part above emits |
| Part 8 | Gateways & Supply Chain — structured-data access and artifact trust |
| Part 9 | Topologies & PETs — where it runs and the privacy primitives |
| Part 10 | Inference Defense & IR — attacks on the model; response when controls fail |
| Part 11 | Compliance & Human Oversight — the regulatory and human program layer |
| Part 12 | Quantum & Closing — the cryptographic foundation and series reference material |

Three cross-cutting threads run the length of the series and are worth knowing before you start:

- The metadata envelope (Part 2) is read by nearly every downstream pattern — retrieval boundaries in Part 3, agent authorization in Part 4, output classification in Part 5, telemetry governance in Part 7, erasure scoping in Part 10.

- The lineage substrate (Parts 2–3) is what makes erasure provable, incident scoping fast, and compliance documentation generable — it appears as a dependency in Parts 8, 10, and 11.

- The tiered maturity model (Tier 1 Foundational → Tier 2 Hardening → Tier 3 Advanced) is consistent across every chapter in every part; an organization can locate itself at one tier across the whole series and mature deliberately.

### Data security page 9

##### How to Use This Part

If you are new to the series: read Part 1 first (or at minimum its Chapters 1–3), then return here.

If you are reading role-first: CISOs and risk leaders will find their center of gravity in Parts 1, 6, 11, and 12; security architects and engineers in Parts 2–5 and 7–10; developers and ML engineers in Parts 3–5 and 8–9. Every chapter states its audience assumptions in its scope section.

If you are reading risk-first: start from the DSGAI risk in the companion document, then use the DSGAI Risk Coverage Matrix (Part 12) to find the chapters — and therefore the parts — that operationalize its mitigations.

Within this part, every chapter follows the same shape: Chapter Scope → DSGAI Risk Reference → Architectural Patterns → Tiered Best Practices → Implementation Guidance → Tooling References → Compliance Mappings. Start at Tier 1 for the chapters that match your dominant risks; mature toward Tier 2 and Tier 3 as your systems scale.

The full series, the companion Risks and Mitigations 2026 document, and the current published versions of both are maintained at genai.owasp.org. Consult the project site for the latest revision before citing this series in audits, attestations, or contractual obligations.

### Data security page 10

#### Why Version 2?

The original LLM and Gen AI Data Security Best Practices Guide (February 2025) established a foundational framework for protecting data in Large Language Model deployments. In the sixteen months since its publication, the threat landscape has moved faster than any single document could anticipate:

- Large-scale adoption of agentic and multi-agent systems has created sprawling Non-Human Identity (NHI) surfaces and cross-boundary data flows that did not exist at enterprise scale when v1 was written.

- Retrieval-Augmented Generation (RAG) has become the dominant production architecture, introducing vector stores, embedding pipelines, and retrieval-time access control as first-class security problems.

- Model Context Protocol (MCP) and tool-calling integrations have extended the data perimeter to third-party plugins, external APIs, and browser-embedded assistants.

- Multimodal inputs — images, audio, video, documents — have expanded the leakage surface beyond text-based filtering controls.

- Real-world incidents — including documented prompt-injection exfiltration against Microsoft Copilot (EchoLeak / CVE-2025-32711) \[1\], Google Gemini (the “Gemini Trifecta”) \[2\], Sourcegraph Amp \[3\], and the Cline VS Code assistant \[4\] in 2025, and the Grok disinformation incidents \[5\] — have validated threat models that were previously theoretical. \[6\]

\[1\] Microsoft 365 Copilot “EchoLeak” — first zero-click indirect prompt-injection data-exfiltration in a production LLM system; CVE-2025-32711. Aim Labs / Aim Security, June 2025.

\[2\] Google Gemini “Gemini Trifecta” — three indirect-prompt-injection issues across Cloud Assist, Search Personalization, and the Browsing Tool. Tenable Research, September 2025.

\[3\] Sourcegraph Amp — invisible-instruction injection and markdown-image data exfiltration. Embrace The Red (J. Rehberger), June 2025.

\[4\] Cline VS Code assistant — markdown-image data exfiltration via prompt injection. Embrace The Red, 2025. (Corrects the earlier “VS Code Continue” attribution.)

\[5\] xAI Grok, 2025 — two distinct mechanisms: (a) retrieval/fact-checker amplification of low-trust sources; (b) system-prompt tampering (“white genocide,” “MechaHitler”). Only (a) is a RAG-retrieval failure.

\[6\] Related, widely documented AI data-exposure incidents (illustrative, not exhaustive): DeepSeek exposed ClickHouse database (Wiz, Jan 2025); Microsoft AI 38 TB SAS-token exposure (Wiz, Sep 2023); Samsung ChatGPT source-code leakage (2023).

### Data security page 11

In parallel, the OWASP GenAI Security Project has restructured its data security output into two companion documents:

- GenAI Data Security Risks and Mitigations 2026 — which enumerates 21 specific data security risks (DSGAI01–DSGAI21) with tiered mitigations.

- This document — Best Practices v2 — which provides the implementation playbook that translates those risks into actionable architectural patterns, operational controls, and governance programs.

Neither document stands alone; together they form a complete practitioner reference.

Objective

This guide exists to answer one question for every team building, deploying, or operating a GenAI system:

Given what we now know about how data is created, stored, transformed, retrieved, generated, and exposed across GenAI and LLM pipelines — what should we actually do?

Best Practices v2 is designed to:

- Provide implementation-first guidance — not just control lists — with concrete architectural patterns, tooling references, and tiered approaches that serve organizations at different stages of security maturity.

- Extend v1’s foundations into the realities of agentic AI, RAG-dominant architectures, tool ecosystems, multimodal data flows, and the AI-DSPM discipline that did not exist in v1.

- Map directly to the 21 risks enumerated in the GenAI Data Security Risks and Mitigations 2026 companion document, providing the “how to fix it” to that document’s “what can go wrong.”

- Serve three distinct audiences — technical cybersecurity leaders who need architectural controls; developers and ML engineers who need secure-by-default patterns; and CISOs who need governance frameworks and compliance readiness.

- Remain aligned with the OWASP Top 10 for LLM Applications, the Agentic AI Top 10, NIST AI RMF, MITRE ATLAS, and relevant regulations including GDPR, HIPAA, CCPA, and the EU AI Act \[7\].

\[7\] Standards and regulations cited throughout. EU AI Act (Regulation (EU) 2024/1689) Arts. 9, 10, 11, 12, 13, 14, 15, 50, 73 — high-risk obligations from 2 Dec 2026; GPAI obligations from 2 Aug 2025 (legacy models by 2 Aug 2027). GDPR (Regulation (EU) 2016/679) Arts. 17,

### Data security page 12

What Has Changed Since v1

Version 2 meaningfully expands three areas that v1 treated as emerging or briefly covered:

Agentic AI and Multi-Agent Systems Version 1 addressed LLM agents in a single section. Version 2 treats agentic architecture as a primary lens across all chapters — NHI credential governance, per-agent least privilege, cross-agent data isolation, tool sandboxing, and human-in-the-loop checkpoints for irreversible actions are threaded throughout.

The AI Data Surface Has Expanded Prompts, context windows, vector stores, embedding pipelines, telemetry streams, browser assistants, multimodal inputs, agent skills and MCP tool payloads are all first-class data assets requiring explicit security controls. Version 2 provides dedicated best practices for each.

AI-DSPM as an Operational Discipline AI Data Security Posture Management — continuous discovery, classification, access governance, DLP, and integrity monitoring across GenAI pipelines — has matured from a concept to a deployed capability. Version 2 dedicates a full chapter to building and operating an AI-DSPM program.

New Risk Coverage Version 2 adds dedicated treatment for risks that v1 did not address or only touched on briefly: natural-language data gateways (LLM-to-SQL/Graph injection), human-in-the-loop and labeler data overexposure, endpoint and browser assistant overreach, cross-context conversation bleed, and disinformation attacks via data poisoning.

22, 25, 28, 30, 32, 33. NIS2 (Directive (EU) 2022/2555) Art. 21(2)(d),(h),(j). ISO/IEC 42001:2023; ISO/IEC 27001:2022; ISO/IEC 30107-3. NIST AI RMF (AI 100-1) and Generative AI Profile (AI 600-1); SP 800-207; SP 800-92; SP 800-226. HIPAA Safe Harbor 45 CFR §164.514(b)(2). CCPA/CPRA Cal. Civ. Code §1798.105/.130. PCI DSS v4.0. Primary texts: EUR-Lex, NIST CSRC, ISO, U.S. HHS.

### Data security page 13

##### Chapter Format

Each chapter follows a consistent structure to maximize usability across roles and maturity levels from chapter 4 onwards:

| Section | Purpose |
| --- | --- |
| DSGAI Risk Reference | Pointer to the relevant DSGAIxx entries in the companion document (GenAI Data Security Risks and Mitigations 2026) for threat context, attack scenarios, and tiered mitigations |
| Architectural Patterns | Secure-by-default design patterns, reference architectures, and data flow diagrams specific to this chapter’s domain |
| Tiered Best Practices & Implementation Guidance | Foundational → Hardening → Advanced controls, so organizations at any maturity level have a clear starting point Step-by-step operational procedures, secure configuration patterns, and code examples focused on how to implement the controls |
| Tooling Reference & DSGAI tool | Specific tools, frameworks, and libraries (vendor-neutral where possible) DSGAI tool - Machine readable tool producing a report of the risk |
| Compliance Mappings | Alignment to NIST AI RMF, MITRE ATLAS, GDPR, HIPAA, EU AI Act, ISO 42001, and related standards |

### Data security page 14

Chapter 1 — Introduction & Scope

##### 1.1 Purpose of This Chapter

This chapter orients readers to the structure, scope, and intended use of GenAI Data Security Best Practices v2. It establishes how the guide relates to its companion document, who it is written for, which authoritative frameworks and regulations it aligns with, and what falls outside its boundaries. Subsequent chapters assume familiarity with the conventions defined here.

##### 1.2 Relationship to the Companion Document

This guide is the implementation companion to OWASP GenAI Data Security Risks and Mitigations 2026 v1.0. The two documents are designed to be read together:

| Document | Function |
| --- | --- |
| Risk and Mitigations 2026 | Enumerates 21 GenAI-specific data security risks (DSGAI01-DSGAI21), each with attack mechanics, capability assumptions, tiered mitigations, and known incidents. Answers “what can go wrong”. |
| Best Practices V2 (this document) | Provides architectural patterns, operational controls, tooling references, and governance procedures to remediate those risks at organizational scale. Answers “what to do about it”. |

Every technical chapter in this guide carries an explicit DSGAI Risk Reference linking back to the corresponding risk entries in the companion document. A complete cross-reference is provided in the DSGAI Risk Coverage Matrix (end of this document).

For convenience, the twenty-one risks of the companion GenAI Data Security Risks and Mitigations 2026 are reproduced below so this guide can be navigated without the companion open. The titles here are the canonical reference used throughout this document.

### Data security page 15

| ID | DSGAI Risk Title | Primary Chapter(s) |
| --- | --- | --- |
| DSGAI01 | Sensitive Data Leakage | 6, 10, 11 |
| DSGAI02 | Agent Identity & Credential Exposure | 7, 12 |
| DSGAI03 | Shadow AI & Unsanctioned Data Flows | 13 |
| DSGAI04 | Data, Model & Artifact Poisoning | 5, 17 |
| DSGAI05 | Data Integrity & Validation Failures | 6, 9, 11 |
| DSGAI06 | Tool, Plugin & Agent Data Exchange Risks | 7, 8 |
| DSGAI07 | Data Governance, Lifecycle & Classification for AI Systems | 4, 22 |
| DSGAI08 | Non-Compliance & Regulatory Violations | 22 |
| DSGAI09 | Multimodal Capture & Cross-Channel Data Leakage | 15, 11 |
| DSGAI10 | Synthetic Data, Anonymization & Transformation Pitfalls | 5, 19 |
| DSGAI11 | Cross-Context & Multi-User Conversation Bleed | 7, 10 |
| DSGAI12 | Unsafe Natural-Language Data Gateways (LLM-to-SQL/Graph) | 16 |
| DSGAI13 | Vector Store Platform Data Security | 6, 9 |
| DSGAI14 | Excessive Telemetry & Monitoring Leakage | 14 |
| DSGAI15 | Over-Broad Context Windows & Prompt Over-Sharing | 7, 10 |
| DSGAI16 | Endpoint & Browser Assistant Overreach | 8, 13 |
| DSGAI17 | Data Availability & Resilience Failures in AI Pipelines | 21 |
| DSGAI18 | Inference & Data Reconstruction | 20, 9, 19 |
| DSGAI19 | Human-in-the-Loop & Labeler Overexposure | 23 |
| DSGAI20 | Model Exfiltration & IP Replication | 20, 5 |
| DSGAI21 | Disinformation & Integrity Attacks via Data Poisoning | 6, 11 |

This guide also supersedes the LLM and Gen AI Data Security Best Practices Guide v1.0 (February 2025). v1 remains authoritative for historical context but should not be used as a current reference; its agentic-AI, RAG, MCP, and AI-DSPM coverage has been substantially extended or restructured here.

### Data security page 16

##### 1.3 How to Use This Guide

The document is organized into 24 thematic chapters plus reference appendices. It is not intended to be read linearly. Three recommended entry points:

- Implementation-driven entry. Identify the architectural component being built or hardened (RAG pipeline, agent system, vector store, MCP integration, observability stack) and read the corresponding chapter directly. Each chapter is self-contained for that domain.

- Risk-driven entry. Start from a specific DSGAI risk in the companion document, then follow its DSGAI Risk Reference into the relevant chapter(s) here. The DSGAI Risk Coverage Matrix provides the inverse mapping.

- Maturity-driven entry. Within each chapter, controls are organized into Tier 1 (Foundational), Tier 2 (Hardening), and Tier 3 (Advanced). Organizations new to GenAI security should establish Tier 1 baselines across all relevant chapters before progressing. The tiering convention is consistent with the companion document.

Where a chapter introduces architectural patterns, those patterns follow a uniform structure (Intent → Problem → Forces → Tiered Implementation → Tradeoffs) adapted from established software architecture pattern conventions. Tooling references are provided vendor-neutrally where feasible; named products are cited only as representative examples and not as endorsements.

##### 1.4 Intended Audience

This guide addresses three practitioner roles whose responsibilities intersect across the GenAI data lifecycle:

Technical Cybersecurity Leaders — security architects, security engineers, AppSec teams, and SOC analysts. Primary use: selecting and deploying architectural controls (governed ingress, retrieval-time ACLs, NHI lifecycle management, observability redaction), integrating GenAI telemetry into SIEM/XDR pipelines, and aligning controls to NIST AI RMF, MITRE ATLAS, and ISO/IEC 42001.

Developers, ML Engineers, and Data Scientists — practitioners building, fine-tuning, and operating GenAI applications, RAG pipelines, and agentic systems. Primary use: secure-by-default implementation patterns, CI/CD integration of adversarial testing and DLP, signed model artifacts and Data Bills of Materials (Data Bill of Materials (DBOM)), and guardrails for prompts, tools, and outputs.

CISOs and Risk Leaders — executives accountable for AI governance, regulatory readiness, and enterprise risk. Primary use: AI-DSPM program design, policy-as-code governance, Data Subject Rights operationalization, EU AI Act Article 10 readiness, and shadow-AI control programs. Chapters 3, 13, 22, and 23 are the recommended starting points for this audience.

### Data security page 17

The guide assumes working familiarity with conventional application and data security concepts (authentication, authorization, encryption, secret management, logging hygiene). It does not re-derive those fundamentals; it extends them to GenAI-specific surfaces.

##### 1.5 Framework and Regulatory Alignment

Controls in this guide are designed to map cleanly onto the following authoritative references. Per-chapter compliance mappings appear in each chapter's Compliance Mappings section.

| Category | Reference |
| --- | --- |
| OWASP | OWASP Top 10 for LLM Applications (2026), OWASP Top 10 for Agentic Applications, OWASP AI Exchange, OWASP Top 10 Web Application Security Risks |
| U.S. Government Standards | NIST AI Risk Management Framework (AI 100-1) and Generative AI Profile (NIST AI 600-1), NIST SP 800-53 Rev. 5, NIST SP 800-218A (SSDF for AI), NIST FIPS 2038/204/205 (post-quantum cryptography) |
| Adversarial Knowledge Bases | MITRE ATLAS, MITRE ATT&CK |
| International Standards | ISO/IEC 42001:2023 (AI management systems), ISO/IEC 23894:2023 (AI risk management), ISO/IEC 27001:2022, ISO/IEC 27701:2019 |
| Software Supply Chain | SLSA (Supply-chain Levels for Software Artifacts), CycloneDX (incl. AI/ML extensions), C2PA (content provenance) |
| Privacy and Sectoral Regulation | GDPR (Regulation (EU) 2016/679), EU AI Act (Regulation (EU) 2024/1689) — including Article 10 obligations for high-risk AI applicable from 2 December 2027, CCPA/CPRA, HIPAA, PIPEDA, PIPL, LGPD |

\[8\] Post-quantum cryptography. FIPS 203 (ML-KEM), FIPS 204 (ML-DSA), FIPS 205 (SLH-DSA), finalized 13 Aug 2024; draft FIPS 206 (FN-DSA, from Falcon). HQC selected as the fifth algorithm (NIST, 11 Mar 2025), documented in NIST IR 8545; transition guidance in NIST IR 8547 (draft); SP 800-208 (stateful hash-based signatures). NSA CNSA 2.0 transition timeline. Source: NIST CSRC / NSA.

### Data security page 18

Regulatory references are illustrative, not exhaustive, and cite the most directly applicable provisions only. This guide does not constitute legal advice; organizations should validate jurisdictional applicability with qualified counsel.

##### 1.6 Scope

In scope. Data security across the full GenAI data lifecycle: source corpora, derived artifacts (embeddings, indices, fine-tuning datasets), model artifacts (weights, adapters, registries), runtime data (prompts, context windows, KV caches, tool payloads, agent memory), operational exhaust (logs, traces, debug captures), and human-in-the-loop labeling pipelines. Architectures covered include monolithic LLM applications, RAG systems, multi-agent orchestrations, MCP and tool-calling integrations, multimodal pipelines, and edge or browser-embedded assistants.

Out of scope. Model alignment and safety policy design, content moderation policy beyond data-leakage relevance, AI ethics frameworks, AI red-teaming methodology (covered by the OWASP GenAI Red Teaming Guide), generic application security topics not specific to GenAI (covered by the OWASP Top 10 and OWASP ASVS), and infrastructure security topics not specific to AI workloads (covered by CIS Benchmarks, cloud provider baselines, and the OWASP Cloud-Native Application Security Top 10). Cross-references are provided where these adjacent domains intersect with GenAI data security.

##### 1.7 Conventions and Versioning

Quantitative targets and KPIs in this document (coverage percentages, time-to-X, ratios) are illustrative planning benchmarks drawn from practitioner experience and expert consensus, not measured population statistics — calibrate them to your environment. Named vendors and tools are cited as representative examples of a capability category, not endorsements. External factual claims — incidents, CVEs, regulatory dates, and acquisitions — are collected with primary sources in the References & Sources section.

Risk identifiers. References of the form DSGAI0X point to the companion Risks and Mitigations 2026 document.

Tier labels. Foundational / Hardening / Advanced describe maturity stages, not severity. A Tier 1 control is not "weaker" than Tier 3; it is the minimum baseline below which a production GenAI system should not operate.

Implementation scope tags. Where applicable, controls are tagged Build (organization-implemented), Buy (vendor capability), or Buy and Build (typically requires both), consistent with the companion document.

### Data security page 19

Living document. GenAI threat surfaces evolve faster than any annual release cycle. This document is maintained as a living reference under the OWASP GenAI Security Project. Material revisions will be tracked in the Revision table; minor corrections will be issued through the project repository. Practitioners are encouraged to consult <https://genai.owasp.org/> for the current published version before referencing this guide in audits, attestations, or contractual obligations.

### Data security page 20

Chapter 2 — GenAI Data Security Fundamentals

##### 2.1 Why GenAI Data Security Is a Distinct Discipline

Traditional data security operates on three implicit assumptions that hold reliably in conventional application architectures: data has a schema, access is bound to identity, and operations are bound to queries. A row in a database is either readable by a principal or it is not; an audit log entry records a specific subject-object-action triple; perimeter controls enforce a clean boundary between trusted and untrusted zones.

GenAI systems violate all three assumptions simultaneously. Sensitive data is transformed into embeddings, summaries, and adapter weights that have no schema and no row-level identity. It is fused into context windows where instructions and data share a single representation. It is generated — at inference time — from a probabilistic state that may reproduce training material verbatim, near-verbatim, or as inferred reconstructions. A model that has been exposed to sensitive data carries some non-zero probability of leaking it through memorization, model inversion, membership inference, or coerced reasoning traces, regardless of whether the original record is still retrievable from any database.

This is not an evolution of conventional data security; it is an extension of its scope into surfaces and failure modes that classical controls were not designed to address. Effective GenAI data security therefore retains every traditional control (encryption, IAM, DLP, segmentation, logging) and adds a layer of GenAI-specific controls on top.

##### 2.2 The GenAI Data Surface: A Working Inventory

Effective controls require an explicit mental model of what data exists across a GenAI pipeline. Six categories cover the operational surface in production systems and align with the taxonomy used in Risks and Mitigations 2026:

### Data security page 21

| Category | Examples | Primary Risk Concentration |
| --- | --- | --- |
| Source Data | Raw corpora (structured and unstructured), user uploads, ticket histories, knowledge bases, analytics exports, document repositories | DSGAI04, DSGAI07, DSGAI21 |
| Derived Data | Embeddings, vector indices, retrieved passages, summaries, synthetic datasets, feature stores, KV caches | DSGAI10, DSGAI13, DSGAI18 |
| Model Artifacts | Foundation model checkpoints, fine-tuned weights, LoRA and PEFT adapters, training logs, evaluation sets, model registries | DSGAI04, DSGAI20 |
| Runtime data | Prompts, system instructions, context windows, tool- call payloads (LLM-to-SQL, LLM-to-Graph, LLM-to- API), agent-to-agent messages, session memory | DSGAI01, DSGAI11, DSGAI15 |
| Operational exhaust | Application logs, distributed traces, debug captures, prompt-replay archives, observability platform records | DSGAI14 |
| Agent state and delegation artifacts | Short- and long-term agent memory, inter-agent messages, delegation chains, cached credentials and tokens, scratchpads | DSGAI02, DSGAI06, DSGAI11 |

Two properties of this surface are operationally important. First, derivative artifacts inherit the sensitivity of their sources but rarely inherit their access controls. An embedding produced from a confidential HR document is itself confidential, but the vector index storing it is often governed by infrastructure-level permissions rather than the HR system's row-level ACLs. Second, the surface expands at deployment time, not design time. Adding an MCP server, enabling browser assistants, or introducing an agent-to-agent handoff each creates new categories of runtime data that did not exist when the system was originally threat-modeled. The surface not only expands at deployment, it mutates afterward - served tool contracts change under stable endpoints with no deployment event (Ch9 P7/P8). A threat model pinned to its approval-time snapshot goes stale silently.

### Data security page 22

##### 2.3 The Context Window as a Trust-Domain Fusion Problem

A single architectural property distinguishes GenAI data security from every prior computing model:

The context window aggregates data from multiple trust domains — system prompt, user input, retrieved passages, tool outputs, conversation history — into a single flat namespace with no internal access control.

In conventional systems, mandatory access control models such as Bell-LaPadula (confidentiality) and Biba (integrity) rely on labeled subjects and objects whose interactions are mediated by a reference monitor. The context window has no such monitor. A retrieved RAG chunk drawn from a confidential corpus sits alongside untrusted user input and adversary-controlled web content with equal trust weight. There is no widely deployed mechanism to mark a context segment as "available for reasoning but not for direct output," "usable for decision-making but not forwardable to other agents," or "trusted as instruction versus trusted as data."

This fusion of control plane (instructions) and data plane (content) is the architectural root of multiple risks enumerated in the companion document, including DSGAI01 (sensitive data leakage), DSGAI11 (cross-context conversation bleed), DSGAI15 (over-broad context windows), and DSGAI21 (disinformation injection via poisoned retrieval). It is also the structural reason why prompt injection, indirect prompt injection, and tool-call manipulation remain unsolved at the model layer despite years of mitigation research.

Practitioners should internalize a corollary: the controls in this guide work around the trust-domain fusion problem; they do not solve it. Until model architectures support per-segment trust labels and a reference monitor over the context window, every GenAI data security program must assume that any content reaching the model can influence any output, and architect compensating controls accordingly — at ingestion, at retrieval, at tool invocation, and at output.

##### 2.4 Core Principles

The remainder of this guide elaborates six principles that reappear, in domain-specific form, across every subsequent chapter. They are not novel in isolation; their application to GenAI surfaces is.

- Data Minimization and Controlled Context. Send only the data required for the immediate task as allowed by access permissions. Constrain context window composition through retrieval limits, payload schemas, and per-call scoping. Reject the convenience pattern of "stuff everything into context and let the model sort it out." Minimization reduces both the leakage surface (DSGAI01, DSGAI15) and the prompt-injection blast radius (DSGAI11).

- Isolation and Least Privilege. Enforce per-tenant, per-user, per-session, and per-agent boundaries at every layer where data is stored or processed: vector indices, conversation memory, tool credentials, and agent identities. Apply Saltzer and Schroeder's least-privilege principle to non-human identities

### Data security page 23

(agents, tools, MCP servers) with the same rigor applied to human accounts. Require human-in-the-loop checkpoints for irreversible or high-impact actions (DSGAI02, DSGAI06).

- Lifecycle Rigor. Treat retention, archival, and erasure as obligations that follow data into every derivative form. A Data Subject Rights deletion request that removes a record from a source database but leaves embeddings in a vector index, traces in an observability platform, and weights in a fine-tuned adapter is a non-compliant deletion. Lifecycle controls must propagate to all derived assets (DSGAI07, DSGAI08).

- Integrity and Provenance. Detect and reject tampering at every stage where data is ingested or transformed: training corpora, RAG sources, retrieved chunks, tool outputs, and model artifacts. Maintain cryptographically verifiable provenance through artifact signing (Sigstore, Cosign), supply-chain attestations (SLSA), and dataset bills of materials (CycloneDX AI/ML extensions). Integrity failures are the entry point for poisoning (DSGAI04), retrieval corruption (DSGAI05), and disinformation campaigns (DSGAI21).

- Continuous Monitoring and Posture Management. Static controls degrade as systems evolve. Continuous discovery, classification, access governance, and DLP across the GenAI surface — operationalized as AI Data Security Posture Management (AI-DSPM) — is required to maintain a defensible posture as agents, tools, and integrations are added at production speed. Chapter 3 develops AI-DSPM as a discipline; every other chapter contributes capabilities to it.

- Zero Trust in the Model. Assume the model is fallible, manipulable, and partially compromised at all times. It can leak through memorization, regurgitate from retrieved context, hallucinate citations, follow injected instructions, and be coerced into revealing reasoning traces. Architect controls under the assumption that no output can be trusted purely because the model produced it and no instruction can be enforced purely because the system prompt declared it. Compensating controls — output filtering, schema validation, tool sandboxing, human approval — are not optional layers; they are the load-bearing layer.

### Data security page 24

##### 2.5 Traditional vs. GenAI-Specific Controls

The discipline is additive, not replacement. The table below — extending the Traditional vs. LLM-Specific Data Security treatment from v1 — illustrates how each conventional control extends into GenAI surfaces.

| Conventional Control | GenAI Extension |
| --- | --- |
| Encryption at rest and in transit | Add encryption for embeddings, KV caches, agent memory, and tool-call payloads. Extend key rotation to vector stores and model registries. |
| Identity and Access Management | Extend to non-human identities (agents, MCP servers, tools) with JIT credential minting and zero-standing-privilege. Per-document ACLs enforced at retrieval time, not only at storage. |
| Data Loss Prevention | Apply DLP at prompt ingestion, retrieval, and output generation. Extend pattern detection across modalities (OCR text, transcribed audio, document metadata). |
| Logging and audit | Default to least-logging. Tokenize or redact sensitive content before it reaches observability platforms. Apply short TTLs to debug captures. |
| Network segmentation | Sandbox tool execution environments. Enforce egress controls on agents and code interpreters. Treat each MCP integration as a distinct trust zone. |
| Vulnerability management | Add adversarial testing (prompt injection, jailbreak, membership inference) to CI/CD. Track AI-specific advisory feeds and model registry CVEs. |
| Incident response | Extend playbooks to GenAI-specific scenarios: output leakage, RAG poisoning, agent credential abuse, vector store compromise, disinformation campaigns. |
| Compliance and audit | Extend Records of Processing Activities (GDPR Art. 30) to AI training and inference. Maintain unlearning-readiness for erasure obligations. |
| Compartmentalization | Enforce granular permission boundaries between agents. Isolate privileged vs non-privileged agents. |

### Data security page 25

##### 2.6 How These Fundamentals Map to the Rest of the Guide

Every subsequent chapter operationalizes one or more of the six principles against a specific GenAI surface. The mapping below provides a quick orientation:

| Principle | Primary Chapters |
| --- | --- |
| Data Minimization and Controlled Context | Ch 4, Ch 9, Ch 10, Ch 11 |
| Isolation and Least Privilege | Ch 6, Ch 7, Ch 8, Ch 12 |
| Lifecycle Rigor | Ch 4, Ch 5, Ch 19, Ch 22 |
| Integrity and Provenance | Ch 5, Ch 6, Ch 7, Ch 17 |
| Continuous Monitoring and Posture Management | Ch 3, Ch 13, Ch 14, Ch 21 |
| Zero Trust in the Model | Ch 8, Ch 10, Ch 11, Ch 24 |

Practitioners building a new GenAI system should not attempt to apply every chapter simultaneously. The recommended progression is: establish the data inventory of Section 2.2 for the system in scope; identify the dominant DSGAI risks for that system using the DSGAI Risk Coverage Matrix; deploy Tier 1 controls in the corresponding chapters; then mature toward Tier 2 and Tier 3 as the system scales and the threat surface expands.

### Data security page 26

Chapter 3 — AI Data Security Posture Management (AI-DSPM)

##### 3.1 Purpose and Boundary

The companion Risks and Mitigations 2026 document defines AI-DSPM and enumerates its 13 capability categories. This chapter does not redefine those capabilities or restate their per-capability practices. It addresses a different question: how does an organization actually build, run, and measure an AI-DSPM program — and how does the program connect to the security operating system the organization already has?

Specifically, this chapter provides:

- The integration model that turns 13 parallel capabilities into a coherent control plane

- An operating model — ownership, governance, and decision rights across security, ML platform, data governance, privacy, and AI governance functions

- A build-versus-buy decision framework, with representative vendor examples per category

- A rollout sequence (90 / 180 / 365 days) for organizations starting from different baselines

- Operational KPIs that demonstrate the program is working

- Integration patterns with SIEM/XDR, incident response, and GRC

- Common anti-patterns observed across enterprise AI-DSPM deployments

For per-capability what to do and tiered controls, refer to the cited DSGAI entries and to the chapters of this guide indexed in §3.9.

###### 3.1.1 Static Posture vs Runtime Behavior

AI-DSPM combines two operationally distinct disciplines that should be reasoned about separately, with different ownership, tooling, and response playbooks. Treating them as one capability is one of the most common ways enterprise AI-DSPM programs fail.

Static posture / configuration risk asks: what is this agent or AI system configured to do? A misconfigured agent with a destructive tool attached, an over-permissioned NHI, an MCP server exposed to the public internet, a Vector database exposed to the internet, an Object storage connected to agent and also exposed to internet, a training data set open for public access, artifact repository, Model serving endpoint exposed

### Data security page 27

for public access, Cache(Redis), Message Queue exposed to Internet, a model registry with an undocumented externally-sourced artifact — these are findings that exist before any session is opened, and they can be remediated before they ever run. Ownership sits with ML platform and security architecture. Detection is reconciliation-based. KPIs include: percent of agents with documented declared purpose; percent of agents with bounded tool surface (no wildcards, no auto-discovery); time-to-detect over-permissioned NHIs after introduction; percent of approved agents whose tool connections have drifted from their approval snapshot (see Chapter 9 Pattern 7b — Approved-Agent Connection Drift Detection).

Runtime behavioral risk asks: what is this agent or AI system doing right now? A live behavioral deviation during an active session — scope drift between authorized intent and executed tool calls, anomalous payload categories, cross-context bleed signals, query-pattern anomalies against a vector store — is an incident, not a configuration finding. Ownership sits with the SOC and AI runtime engineering. Detection is signal-based. KPIs include: scope-drift detections per 1,000 sessions; mean-time-to-contain a misbehaving agent; false-positive rate on runtime behavioral signals; SOC enrichment coverage for AI-DSPM telemetry.

Programs that conflate these two surfaces tend to invest heavily in one and assume they have both — a posture program that produces findings nobody can respond to in real time, or a runtime monitoring stack that has no canonical record of what each agent was supposed to be doing. The two need different tooling, different escalation paths, often different teams, and explicitly separate KPI sets. The capabilities in §3.2 below and the rollout sequence in §3.5 cover both, but the dependency structure and the operating model should be read with the static/runtime split in mind.

### Data security page 28

##### 3.2 AI-DSPM as an Integrated Control Plane

AI-DSPM fails most often not because individual capabilities are absent, but because they are deployed as parallel silos that do not share state. Asset inventory tools that don't emit into the lineage graph; classification engines whose labels never reach the DLP enforcement layer; poisoning detection signals that don't trigger incident response. The 13 capabilities are dependent, not independent — and the dependencies determine the rollout sequence.

The following dependency structure should anchor program design. Capabilities to the left feed capabilities to the right:

| Foundation Layer | Detection & Enforcement Layer | Governance & Lifecycle Layer |
| --- | --- | --- |
| (1) Asset inventory | (5) Prompt/RAG/output DLP | (10) Lifecycle and erasure |
| (2) Classification | (6) Vector store posture | (11) Training governance |
| (3) Lineage and DBOM | (7) Integrity and poisoning detection | (12) Resilience |
| (4) Access governance (incl. NHI) | (8) Observability posture | (13) Shadow AI controls |
|  | (9) Connector governance |  |

Dependencies that must be wired explicitly:

- Inventory → Classification → DLP. DLP cannot enforce on assets it has no labels for, and labels cannot exist on assets the inventory has not discovered. Classification coverage is the rate-limiter for DLP effectiveness.

- Lineage → Erasure → Compliance. Data Subject Rights compliance under GDPR and CCPA requires erasure to propagate to derived artifacts (embeddings, fine-tuning checkpoints, cached retrievals). Without lineage, propagation is manual and incomplete. See DSGAI07 and DSGAI08.

- Integrity detection → Incident response. Poisoning detection signals that do not activate an Incident Response playbook leave critical alerts effectively acknowledged but untriaged. . See Ch. 21.

- Observability posture → All detection. Every other detection capability emits telemetry that must itself be governed against leakage. Excessive logging increases the risk of sensitive data exposure by aggregating sensitive information across logs.. See DSGAI14.

### Data security page 29

- Connector governance → Shadow AI controls. Shadow AI is the inverse of connector governance: assets that should be in the connector inventory but are not. The same discovery infrastructure supports both. See DSGAI03 and DSGAI06.

A defensible AI-DSPM program treats these as an explicit data flow, not a checklist. Capability owners are accountable for what they emit downstream, not only for what they detect locally.

### Data security page 30

##### 3.3 Operating Model

Most AI-DSPM programs fail at the seam between security and ML platform teams, not within either. The operating model below assigns clear ownership and avoids the two most common dysfunctions: security teams that lack ML context, and ML platform teams that lack security mandate.

Recommended functional ownership (Responsible / Accountable / Consulted / Informed (RACI) summary):

| Capability Area | Accountable | Responsible | Consulted | Informed |
| --- | --- | --- | --- | --- |
| Asset inventory, classification, lineage | CISO | Data Governance + ML Platform | Privacy, Legal | AI Gov Committee |
| Access governance (incl. NHI) | CISO | IAM team + ML Platform | App Sec + Enterprise Architecture | Audit |
| Prompt/RAG/output DLP | CISO | App Sec + ML Platform | Privacy | Product owners |
| Vector store posture | ML Platform Lead | ML Platform + Cloud Sec | App Sec | CISO |
| Integrity & poisoning detection | CISO | SOC + ML Platform | Threat Intel | AI Gov Committee |
| Observability posture | CISO | SRE/Platform + SOC | Privacy | Engineering |
| Connector governance | CISO | App Sec + Procurement | Legal, Privacy | AI Gov Committee |
| Lifecycle & erasure | DPO / Privacy Lead | Data Governance + ML Platform | Legal, CISO | Audit |
| Training privacy | ML Platform Lead | SRE + ML Platform | Legal, CISO | AI Gov Committee |
| Resilience | CISO | SRE + ML Platform | App Sec | Business Continuity |
| Shadow AI | CISO | SOC + Procurement + HR | Legal, Privacy | All Employees |
| AI Data plane Exposure Management | CISO | Cloud Security + AI Platform | App Sec + Database team | AI Governance Committee |

### Data security page 31

Governance forum. An AI Governance Committee chaired by a senior executive (commonly the CISO, CDO, or General Counsel depending on organizational structure), with standing membership from security, privacy, legal, ML platform, and the highest-velocity AI-consuming business units. Cadence: monthly for active programs, quarterly for steady state. The committee owns the AI-DSPM program charter, KPI review, and material exception decisions. Its remit aligns with ISO/IEC 42001:2023 clause 5 (leadership) and clause 6 (planning).

Operating tempo. Daily: SOC monitoring of AI-DSPM telemetry. Weekly: ML platform and security joint review of new AI assets onboarded. Monthly: KPI review at AI Governance Committee. Quarterly: lineage and classification audit (per DSGAI07 mitigation guidance). Annually: full program review against ISO/IEC 42001 and NIST AI RMF; tabletop exercise simulating a GenAI incident (RAG poisoning, agent credential abuse, or model exfiltration).

##### 3.4 Build, Buy, or Compose: A Decision Framework

The AI-DSPM market consolidated rapidly through 2025–2026, with major platform vendors absorbing AI-specific startups. Palo Alto Networks completed its acquisition of Protect AI in July 2025 \[9\], integrating model scanning, posture management, AI red teaming, and runtime protection into Prisma AIRS. Veeam announced a strategic combination with Securiti in late 2025, integrating DSPM with AI-runtime controls (product branding still evolving as of this revision). Cisco acquired Robust Intelligence. This consolidation is ongoing: vendor names cited below are illustrative of categories, not endorsements, and category boundaries continue to shift.

The OWASP GenAI Security Project remains vendor-neutral. Vendor names appear in this chapter solely as representative examples of categories; inclusion is not endorsement, exclusion is not criticism, and category descriptions take precedence over named examples. Practitioners should validate current capability against their own requirements before procurement.

Decision criteria. Before evaluating tools, organizations should answer following questions:

- What is the regulatory floor? EU AI Act high-risk obligations (applicable from 2 December 2027), HIPAA, PCI DSS, and sectoral regulations dictate non-negotiable controls. Tooling that cannot evidence them is disqualified regardless of feature parity.

\[9\] Palo Alto Networks completed its acquisition of Protect AI in July 2025 (announced April 2025); integrated into Prisma AIRS. PANW press release.

### Data security page 32

- What is the deployment topology? Air-gapped or sovereign-cloud requirements, on-premises foundation models, and multi-cloud federation each constrain the candidate set significantly.

- What is the existing security stack? An organization with a mature CNAPP, SIEM, and DSPM gets more leverage from extension than from greenfield AI-specific platforms. An organization without these gets less.

- What is the operational capacity? Build options require engineering, maintenance, and security operations capacity, whereas Buy options require budget, integration support, and vendor-management capacity.

- What is coverage for AI Identity & NHI? Can it discover identities that are connected to AI agents, MCP Server, Vector Database etc.

- What are Identity & Access Integration capabilities? Does it integrate with IAM tools like Microsoft Entra ID, Okta, WAS IAM, GCP IAM etc.

- Does it have AI native Security Capability? Does it detect prompt injection, data poisoning, data leakage, RAG data leakage etc.

- Capabilities around scalability & Performance? What is capabilities are millions of embeddings, agents count landscape, multi-region deployments

- Capabilities around extensibility/exposure for APIs? Can it expose API for home grown AI tools or third party tools

Category-by-category guidance:

| Capability | Build | Buy | Compose (Open Source) | Decision Heuristic |
| --- | --- | --- | --- | --- |
| AI asset inventory & classification | Custom scanner over cloud APIs and ML platform metadata | Extend existing DSPM (e.g., Cyera, Sentra, BigID, Securiti, Varonis, Symmetry, Microsoft Purview, IBM Guardium, Forcepoint, Cyberhaven); or AI-specific posture (Wiz AI-SPM, Prisma AIRS, Lasso Security, HiddenLayer, Cranium) | OpenMetadata, DataHub | If DSPM exists and covers structured stores well: extend. If greenfield and AI-heavy: AI-SPM. |
| Lineage & DBOM | ML metadata in custom registry | MLflow, Weights & Biases, Vertex/SageMaker registries with lineage, Microsoft | CycloneDX ML- BOM (ECMA-424), in-toto | Lineage is rarely buy-only; expect compose + |

### Data security page 33

| Capability | Build | Buy | Compose (Open Source) | Decision Heuristic |
| --- | --- | --- | --- | --- |
|  |  | purview Lineage, Azure ML Registry | attestations, OpenLineage | integrate in all cases. |
| Access governance (incl. NHI) | Custom policy layer | IAM platforms with NHI extensions; CyberArk for privileged AI identity (CyberArk; acquisition by Palo Alto Networks announced 30 July 202510, closed 11 February 2026); Apono, Aembit, Britive, Astrix for JIT/NHI, Saviynt (Zuma) | OPA + Keycloak; SPIFFE/SPIRE for workload identity | Default to buy for NHI lifecycle. Build only the policy expression layer. |
| Prompt/RAG/ output DLP & runtime guardrails | Custom regex + LLM-based scanners | Lakera, Prompt Security, CalypsoAI, HiddenLayer, Prisma AIRS; cloud-native (AWS Bedrock Guardrails, Macie, Azure AI Content Safety, Google Cloud DLP, Sensitive Data Protection and Model Armor) | LlamaFirewall (Meta), NVIDIA NeMo Guardrails, Guardrails AI, Microsoft Presidio (PII detection) | If single-cloud: cloud-native. If multi-cloud or sensitive workloads: dedicated commercial or compose. |
| Vector store posture | Custom monitoring | Coverage in AI-SPM platforms; native controls in Pinecone, Weaviate Enterprise, Qdrant, Milvus | OpenSearch with role-level security; pgvector with PostgreSQL RLS | Largely vendor- native today; AI- SPM coverage is maturing. |

\[10\] Palo Alto Networks–CyberArk — announced 30 July 2025, closed 11 February 2026 (~$25B). PANW / CyberArk filings.

### Data security page 34

| Capability | Build | Buy | Compose (Open Source) | Decision Heuristic |
| --- | --- | --- | --- | --- |
| Integrity & poisoning detection | Custom drift and outlier monitoring | AI-SPM platforms (HiddenLayer, Prisma AIRS); model registry scanning | NVIDIA Model Validation, Hugging Face safety scanners, Adversarial Robustness Toolbox (IBM/LF AI) | Compose for foundation; buy for production-grade anomaly detection. |
| Observability posture | Custom log redaction | LLM observability with built- in redaction (Arize AI, Datadog LLM Observability, Splunk, Dynatrace) | OpenTelemetry GenAI semantic conventions, Langfuse, Helicone | Telemetry is high- volume and high- sensitivity; buy or compose with discipline. |
| Connector & MCP governance | Internal app review board with AI rider | Cloud Application Security Brokers (CASB) with AI extensions; agent-security platforms (Zenity, Knostic, Pillar Security, Prisma AIRS) | MCP server allowlist with policy gateway | This is the fastest- evolving category; expect to re- evaluate annually. |
| Lifecycle & erasure | Custom propagation | Privacy platforms (OneTrust, TrustArc, Securiti) with AI extensions | Custom lineage- driven erasure tooling | Buy for DSR workflows; build for derived-artifact propagation. |
| Training privacy (DP, federated) | DP-SGD in custom training stack | Cloud-native (Google Vertex AI privacy, AWS Clean Rooms ML) | Opacus (PyTorch DP), TensorFlow Privacy, OpenFL, Flower | Compose dominates; commercial offerings still maturing. |
| Resilience | Custom backup and | Cloud-provider native; backup platforms with ML | Standard backup tools applied to | Mostly an extension of existing resilience |

### Data security page 35

| Capability | Build | Buy | Compose (Open Source) | Decision Heuristic |
| --- | --- | --- | --- | --- |
|  | restore for ML artifacts | artifact awareness (Veeam Agent Commander) | vector stores and registries | programs; ML- artifact-aware tooling is nascent. |
| Shadow AI | Custom egress and proxy analysis | CASB/SSE platforms (Netskope, Zscaler, Palo Alto Prisma Access, Microsoft Defender for Cloud Apps) with AI categorization; browser-based controls (Cyera Browser Shield, Island Browser, LayerX, Talon (now part of Palo Alto Networks), Menlo Security, Seraphic Security) | Egress NetFlow + DNS analysis | Buy for breadth; build for organization- specific tools. |

Ten-question vendor evaluation. Beyond category fit, evaluate against:

- Does it integrate with our existing SIEM/XDR via standard formats (Common Event Format (CEF), Open Cybersecurity Schema Framework (OCSF))?

- Does it expose APIs sufficient to extract findings into our GRC platform?

- What is the sub-processor chain for the vendor's own infrastructure?

- Does the vendor offer a sovereign or self-hosted deployment for regulated workloads?

- Is the classification engine extensible to organization-specific data types?

- Does the platform emit lineage in a standard format (CycloneDX Machine-Learning Bill of Materials (ML-BOM), OpenLineage)?

- What is the runtime overhead of inline guardrails on inference latency?

- Are findings deterministic enough for audit, or LLM-generated?

- What is the vendor's posture on customer-data training (zero-data-retention by default)?

- What is the platform's coverage for the organization's primary cloud(s) and model providers?

- Does the tool calculate AI Risks score?

- How does the tool integrate with data security related tools like Microsoft Purview, Amazon Macie, AWS glue data catalog, GCP Dataplex?

- Does the tool scale to thousands of agents and millions of AI interactions?

- How does the tool detect prompt injection, data leakage and model misuse?

### Data security page 36

- What automated remediations actions can the tool perform?

- Can the platform support emerging standards like MCP and agentic AI without major architectural changes?

##### 3.5 Maturity Model and Rollout Sequencing

Organizations should not attempt to deploy 13 capabilities in parallel. The sequence below is anchored to the dependency structure in §3.2 and assumes a typical enterprise starting state — existing DSPM and IAM, cloud-resident ML workloads, and active GenAI projects in production.

Days 0–90 — Foundation. Goal: Establish ground truth.

- Stand up AI Governance Committee with charter and decision rights.

- Deploy AI asset discovery and inventory across cloud accounts, model registries, vector stores, and observability platforms.

- Issue and operationalize Shadow AI policy (DSGAI03 Tier 1 controls).

- Deploy egress DLP and Cloud Access Security Broker (CASB) for unsanctioned AI domains.

- Centralize prompt/output DLP at the foundation-model gateway layer (DSGAI01 Tier 1).

- Inventory and tag all NHIs (agents, MCP servers, tools) and rotate any standing credentials (DSGAI02 Tier 1).

Days 90–180 — Hardening. Goal: Make detection deterministic and propagate labels.

- Deploy classification with label propagation to derived artifacts (DSGAI07 Tier 1–2).

- Stand up lineage and ML-BOM emission across training and indexing pipelines.

- Implement per-document ACLs at retrieval time for RAG (DSGAI13, see Ch. 6).

- Deploy observability posture: tokenization in traces, short TTLs for debug captures (DSGAI14, see Ch. 14).

- Onboard connector inventory and risk-rate every MCP and tool integration (DSGAI06, see Ch. 9).

- Operationalize Data Subject Rights workflows including derived-artifact propagation (DSGAI07–08, see Ch. 22).

Days 180–365 — Maturity and resilience. Goal: Become measurable and survivable.

- Implement integrity and poisoning detection with IR integration (DSGAI04, DSGAI21, see Ch. 21).

- Add training privacy controls (DP-SGD, membership inference auditing) to fine-tuning pipelines (DSGAI10, DSGAI18, see Ch. 5 and Ch. 19).

- Deploy resilience controls — vector-store backup with semantic restore validation, RTO/RPO objectives for AI components (DSGAI17, see Ch. 21).

### Data security page 37

- Establish AI-DSPM KPI dashboard with targets and trend reporting to the AI Governance Committee.

- Run a tabletop exercise simulating a GenAI incident; remediate playbook gaps surfaced.

Organizations retrofitting AI-DSPM onto extensive existing GenAI deployments should expect this sequence to take 12–18 months rather than 12 months, and should prioritize asset discovery and Shadow AI discovery before any other capability — the inventory gap is almost always larger than initial estimates.

##### 3.6 Operational KPIs

What gets measured determines whether the program survives the first budget cycle. The metrics below are observed across mature AI-DSPM programs and align with the capability dependencies in §3.2.

| Capability area | Indicative KPI | Target trajectory |
| --- | --- | --- |
| Asset inventory | % of known AI assets discovered automatically vs. manually registered | &gt;90% automated within 12 months |
| Classification | % of GenAI assets with sensitivity labels propagated to derivatives | &gt;95% within 12 months |
| Lineage | % of training runs and RAG indices with complete data-to-model lineage | &gt;80% within 12 months |
| Access governance | NHI credential rotation latency (mean and p95) | p95 ≤ 24 hours |
| DLP | False positive rate; sensitive data egress events / month | FPR &lt;5%; egress events trending to zero |
| Vector store posture | % of vector stores with tenant scoping and at- rest encryption | 100% within 6 months |
| Integrity detection | Mean time to detect (MTTD) for poisoning indicators | &lt;24 hours for high-confidence signals |
| Observability | % of trace captures with PII/secret redaction enforced | 100% within 6 months |

### Data security page 38

| Capability area | Indicative KPI | Target trajectory |
| --- | --- | --- |
| Connector governance | % of connectors with current risk rating and contractual data-handling terms | 100% within 12 months |
| Lifecycle & erasure | Mean time to fulfill DSR erasure across derived artifacts | ≤ 30 days (GDPR ceiling) |
| Training privacy | % of fine-tuning runs with DP audit or membership-inference test record | &gt;50% within 12 months for sensitive workloads |
| Resilience | % of vector stores and model registries with tested restore drills | 100% annually |
| Shadow AI | New unsanctioned AI tools detected / month; mean time to remediate | Detection rate stable or declining; remediation &lt; 14 days |
| Model Provenance | % of Models/prompts with signed integrity registration | 100% within 12 months |
| NHI Least privilege | % of NHI at least privilege | &gt;90% within 12 months |
| Egress Control | % of agents on deny-by-default egress (can only reach the gateway) | 100% within 6 months |

KPIs should be reviewed monthly by the AI Governance Committee and reported annually to the Board Risk Committee or equivalent. Trend matters more than absolute value: a falling false-positive rate at constant detection volume is a healthier signal than a low rate achieved by tuning down sensitivity.

### Data security page 39

##### 3.7 Integration with Existing Security Operations

AI-DSPM is most defensible when it does not run as a parallel security operation. Five integration patterns are essential:

SIEM/XDR integration. AI-DSPM telemetry — DLP findings, NHI credential anomalies, integrity-check failures, vector store query anomalies — must reach the SIEM in a normalized format (CEF, OCSF, or vendor-native ECS-equivalent). This enables correlation with conventional security signals: a vector store enumeration spike correlated with a credential anomaly on the corresponding NHI is a far higher-confidence incident than either alone. Expect to define new use cases per DSGAI risk class: prompt-injection chains, RAG poisoning, agent credential abuse, model extraction patterns.

Incident response integration. Existing IR playbooks must be extended, not duplicated. Each DSGAI risk class needs a defined entry point in the IR runbook: detection signals, scoping queries, containment actions (revoke NHI credentials, quarantine vector indices, roll back to known-good model snapshots), and forensic artifacts to preserve. Chapter 21 develops this in detail; the AI-DSPM program supplies the upstream telemetry that those playbooks consume.

GRC integration. AI-DSPM findings should flow into the same GRC platform that handles ISO 27001, SOC 2, ISO 42001, and regulatory attestations. Treat AI-DSPM as a control family within the existing audit framework rather than a standalone compliance program. This maps to NIST AI RMF Govern and Manage functions and to ISO/IEC 42001 clauses 7–10.

Data Catalog & DSPM Integration. AI-DSPM should integrate with enterprise data catalog, DSPM platforms, purview, vector databases etc to automatically discover sensitive information, classify AI training data, detect excessive access and map data lineage across AI pipelines.

Model governance & DSPM Integration. AI-DSPM should integrate with model registries to associate sensitive datasets with approved models, model provenance, model version and deployment history. This enables detection of unauthorized model usage and supports auditability

### Data security page 40

##### 3.8 Common Anti-Patterns

Five failure modes recur across enterprise AI-DSPM deployments. Recognizing them early prevents predictable rework:

1. Tool-first programs without governance. Procuring an AI-SPM platform before establishing the AI Governance Committee and ownership model. The platform produces findings nobody owns; alert fatigue follows; the program quietly disengages within nine months. Sequence: governance → operating model → tooling. Not the reverse.

2. Governance-only programs without telemetry. Standing up policy and committee structure with no data feed. Decisions are made on anecdote rather than posture data. Effective AI-DSPM requires both — neither in isolation produces durable risk reduction.

3. Capability sprawl. Deploying all 13 capabilities in parallel. Resourcing thins, dependencies are violated (DLP without classification, erasure without lineage), and no capability reaches operational maturity. The dependency structure of §3.2 is not optional.

4. Ownership ambiguity between security and ML platform. Each team assumes the other owns AI-DSPM. The result is observable in audit findings: classification labels exist but don't reach DLP; vector stores have encryption but no posture monitoring; integrity scanning produces alerts that have no IR runbook. The RACI in §3.3 is the minimum scaffolding to prevent this; assignments must be explicit and reviewed quarterly.

5. Static program design. Standing up AI-DSPM as a project with a defined end date. The AI surface evolves continuously — new model providers, new MCP servers, new agentic frameworks, new regulatory deadlines. Programs that lack a continuous capability-evolution cadence are obsolete within 18 months of go-live.

### Data security page 41

##### 3.9 Capability-to-Chapter Map

For implementation depth on each capability, refer to the chapters listed below. This chapter has covered the program layer; the chapters below cover the control layer.

| AI-DSPM capability (companion §) | Primary chapter(s) in this guide |
| --- | --- |
| (1) Asset discovery & inventory | Ch. 4, Ch. 13 |
| (2) Classification & policy binding | Ch. 4 |
| (3) Lineage & DBOM | Ch. 4, Ch. 5 |
| (4) Access governance (incl. NHI) | Ch. 12 |
| (5) Prompt/RAG/output DLP | Ch. 10, Ch. 11 |
| (6) Vector store posture | Ch. 7 |
| (7) Integrity & poisoning detection | Ch. 5, Ch. 6, Ch. 21 |
| (8) Observability posture | Ch. 14 |
| (9) Connector governance | Ch. 9 |
| (10) Lifecycle & erasure | Ch. 4, Ch. 22 |
| (11) Training privacy | Ch. 5, Ch. 19, Ch. 24 |
| (12) Resilience | Ch. 20, Ch. 21 |
| (13) Shadow AI | Ch. 13 |

### Data security page 42

Acknowledgements

###### Authors

Emmanuel Guilherme Junior, OWASP GenAI Data Security Initiative Lead

Scott Clinton, Board Chair, OWASP GenAI Co-founder

Kyriakos "Rock" Lambros, Director of AI Standards and Governance, Zenity

###### Contributors

Anitha Dakamarri, Lead Security Engineer, Donnelley Financial Solutions

Narendra Kumar Nutalapati, Independent Researcher, AI Runtime Integrity Engineer

Kyle Topasna, Security Researcher

Logan Barré, Cybersecurity AI analyst, Société Générale

Rico Komenda

Hudson Pereira

Harish Ramachandran, SAP, Senior Director Security and Compliance Programs

Victor Lu

Oz Wasserman, Co-founder and CPO, Opsin

David Weidman, SenTeGuard

Sanjeev Agarwal, BIDODI InfoSec

Gautam Bharti, Founder, mcpindex.ai

Neeraj K Verma, Security Transformation Architect, Accenture Middle East

Sankalp Gilda, Staff Machine Learning Engineer, DeepThought Solutions

Mayur Agnihotri, Senior Information Security Specialist, StraightArc Technologies Pvt. Ltd.

### Data security page 43

###### Reviewers

Rubens Zimbres PhD, Senior AI Security Engineer, Zendata

Praveen Dandin, Architect/ Senior Principal Software Engineer, Palo Alto Networks

Russell Tait, Senior Security Advisor

Raj Annaladasu, Staff Customer Success Engineer, Palo Alto Networks

Neeraj K Verma, Security Transformation Architect, Accenture Middle East

Angela Sorosina, Information Security Manager

Rishabh Gupta, Software Engineer

Adwait Arun Sathe, Senior Data Engineer(Privacy & Security)

Nour Mousa, CISSP, PhD Candidate, Toronto Metropolitan University (Quantum Security)

S M Zia Ur Rashid, The University of Tulsa

Sahil Mehta, Product Manager, Palo Alto Networks

Eric Coomer — Builder, Personal A.I. Console (PAC) · Independent

Martin C. Rojas, OWASP Chapter Buenos Aires, Cybersecurity Technician

Hina Kousar, Security Senior Engineer, FICO

Mayur Agnihotri, Senior Information Security Specialist, StraightArc Technologies Pvt. Ltd.

Sankalp Gilda, Staff Machine Learning Engineer, DeepThought Solutions Serge Batchilo, Cybersecurity Content Architect

Ivan Dobrovolskyi, Staff Software Engineer (AI Engineering), Walmart Global Tech

Gautam Bharti, Founder, mcpindex.ai

Arpna Aggarwal, Lead IT Auditor

Illia Oleksiuk, Senior Software Engineer

Vadym Shashkov, Co-Founder & CTO, SUPERAGENT AI

Alessandro Pignati, Lead AI Security Researcher, NeuralTrust

Keyur Rajyaguru, AI SecOps, Walmart Global Tech

Oleksii Proniakin, Engineering, Harvey

Joe Gerber, AI Security Architect and Senior Consultant, Magellan Research

### Data security page 44

#### OWASP GenAI Security Project Sponsors

We appreciate our Project Sponsors, funding contributions to help support the objectives of the project and help to cover operational and outreach costs augmenting the resources provided by the OWASP.org foundation. The OWASP GenAI Security Project continues to maintain a vendor neutral and unbiased approach. Sponsors do not receive special governance considerations as part of their support.

Sponsors do receive recognition for their contributions in our materials and web properties. All materials the project generates are community developed, driven and released under open source and creative commons licenses. For more information on becoming a sponsor, visit the Sponsorship Section on our Website to learn more about helping to sustain the project through sponsorship.

##### Project Sponsors:

Sponsors list, as of publication date. Find the full sponsor list here.

**Source links on this page**

- [r list here.](<https://www.google.com/url?q=https://genai.owasp.org/supporters/&source=gmail-imap&ust=1754531103000000&usg=AOvVaw3gBhNEHqX7Wxu0V8UFXJOf>)

### Data security page 45

#### Project Supporters

Project supporters lend their resources and expertise to support the goals of the project.

Cobalt

Accenture

Kainos

PromptArmor

AddValueMachine Inc

Cohere

KLAVAN

Pynt Quiq

Aeye Security Lab Inc.

Comcast

Klavan Security Group

AI informatics GmbH

Complex Technologies

KPMG Germany FS

Red Hat

AI Village

Credal.ai

Kudelski Security

RHITE

aigos

Databook

Lakera

SAFE Security

Aon

DistributedApps.ai

Lasso Security

Salesforce

Aqua Security

DreadNode

Layerup

SAP

Astra Security

DSI

Legato

Securiti

AVID

EPAM

Linkfire

See-Docs & Thenavigo

AWARE7 GmbH

Exabeam

LLM Guard

ServiceTitan

AWS

EY Italy

LOGIC PLUS

SHI

BBVA

F5

MaibornWolff

Smiling Prophet

Bearer

FedEx

Mend.io

Snyk

BeDisruptive

Forescout

Microsoft

Sourcetoad

Bit79

GE HealthCare

Modus Create

Sprinklr

Blue Yonder

Giskard

Nexus

stackArmor

BroadBand Security, Inc.

GitHub

Nightfall AI

Tietoevry

BuddoBot

Google

Nordic Venture Family

Trellix

Bugcrowd

GuidePoint Security

Normalyze

Trustwave SpiderLabs

Cadea

HackerOne

NuBinary

U Washington

Check Point

HADESS

Palo Alto Networks

University of Illinois

Cisco

IBM

Palosade

VE3

Cloud Security Podcast

iFood

Praetorian

WhyLabs Yahoo

Cloudflare

IriusRisk

Preamble

Cloudsec.ai

IronCore Labs

Precize

Zenity

IT University Copenhagen

Prompt Security

Coalfire

Supporters list, as of publication date. Find the full supporter list here.

**Source links on this page**

- [r list here.](<https://www.google.com/url?q=https://genai.owasp.org/supporters/&source=gmail-imap&ust=1754531103000000&usg=AOvVaw3gBhNEHqX7Wxu0V8UFXJOf>)
