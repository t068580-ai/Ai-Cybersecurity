# AI Cybersecurity Plugin

**Version 1.0.1** · A Claude plugin for auditing and securing software, SaaS, serverless and AI projects, bundled with a full engineering skills pack.

> **نظرة سريعة:** بلقن لـ Claude فيه سكل فحص أمني شامل (84 ضابط أمني + ملحق خاص بالذكاء الاصطناعي) يطلع تقرير بالأدلة ونسبة جاهزية وقرار إطلاق، وسكل للبرمجة الآمنة مبني على معايير OWASP الحديثة، بالإضافة إلى حزمة كاملة من مهارات هندسة البرمجيات (التخطيط، الاختبارات، المراجعة، الأداء، الإطلاق). يشتغل بالعربي والإنجليزي.

## Table of contents

- [What's inside](#whats-inside)
- [Security skills](#security-skills)
- [Engineering skills pack](#engineering-skills-pack)
- [Agents, commands and checklists](#agents-commands-and-checklists)
- [Installation](#installation)
- [Example prompts](#example-prompts)
- [Repository layout](#repository-layout)
- [Safety and scope](#safety-and-scope)
- [Contributing](#contributing)
- [Credits and licenses](#credits-and-licenses)

## What's inside

| Component | Count | Purpose |
|---|---|---|
| Security skills | 2 | `ai-cybersecurity` (full audit) and `secure-coding` (build and review securely) |
| Engineering skills | 24 | The full [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) pack, from spec to ship |
| Agents | 4 | `security-auditor`, `code-reviewer`, `test-engineer`, `web-performance-auditor` |
| Slash commands | 9 | `/spec`, `/plan`, `/build`, `/test`, `/review`, `/code-simplify`, `/constraints`, `/webperf`, `/ship` |
| Checklists | 7 | Security, performance, accessibility, observability, testing, orchestration, definition of done |
| OWASP GenAI texts | 4 | Reference material the audit searches when rating AI risks |

## Security skills

### `ai-cybersecurity`: evidence-based security audit

A read-only audit of any software, SaaS, serverless or AI project against **84 controls (SEC-01 to SEC-84)** plus a separate **AI appendix (AI-01 to AI-12)**. The result is a documented set of findings with evidence, a score, and a release decision. It never edits the project.

**Control groups**

| Group | Controls | Covers |
|---|---|---|
| Core | SEC-01 to SEC-70 | Identity, access control, tenancy, secrets, input handling, web and API, payments, infrastructure, data, logging, backups, supply chain |
| Edge and serverless | SEC-71 to SEC-82 | Unauthenticated invocation, function-level authorization, RLS and gateway bypass, secret bindings, IAM overreach, rate and cost limits, JWT and webhook signatures, logs, SSRF, tenant isolation, AI spending, monitoring and safe deployment, with notes for Supabase, Cloudflare Workers, Vercel, AWS Lambda and n8n |
| LLM and agents | SEC-83, SEC-84 | Jailbreak resilience, and agent compromise, tool hijack and unauthorized actions |
| AI appendix (not scored) | AI-01 to AI-12 | RAG poisoning, vector isolation, agent memory, MCP and tool plugins, token cost, provider data handling, model changes, high-stakes decisions, system prompt leakage, indirect injection, spoofed tool results, crossing user boundaries |

**Workflow**

1. **Project card:** fills in project type, stack, environments, hosting, auth and AI features from evidence, or marks them UNKNOWN.
2. **Discover:** inventories repositories, packages, endpoints, jobs, deploy files, cloud resources, databases, AI features, coding agents and MCP clients.
3. **Recent advisories:** checks very recent critical advisories for the detected frameworks, using public package names only.
4. **Trace:** follows real sensitive flows end to end (login, reading another user's data, payments, uploads, AI tool calls, webhooks, password reset).
5. **Validate:** collects evidence for every control, starting with the priority set for the project type.
6. **Rate:** each control gets a status, risk, evidence level and confidence.
7. **Report:** writes the report. Nothing is fixed unless you ask afterwards.

**Ratings and decision**

- **Status:** PASS, PARTIAL, FAIL, UNVERIFIED (never counted as a pass) or N/A (only with a stated reason).
- **Evidence levels:** E1 code, E2 config, E3 authorized test, E4 operations (live logs, cloud policy, proven backup restore).
- **Confidence:** CONFIRMED, LIKELY, UNKNOWN.
- **Score:** a weighted `ConservativeScore`, always shown with `EvidenceCoverage`. Missing evidence lowers the score; it does not count as a flaw.
- **Decision:** `READY`, `NOT READY` or `INSUFFICIENT EVIDENCE`. READY needs at least 90% coverage, no open CRITICAL or HIGH finding without an accepted exception, no CRITICAL or HIGH control left unverified, and proven backups, access procedures and monitoring.

**Priority by project type**

The audit starts with the controls that matter most for the project: multi-tenant SaaS, web and API, mobile with backend, payments and credits, AI chat and LLM SaaS, agents and MCP automation, RAG and vector databases, edge and serverless, and cloud, VPS, Docker or Kubernetes.

**Report sections**

A. Executive summary · B. Architecture and attack surface · C. 84-control matrix · C2. OWASP Top 10:2025 view · D. Detailed findings · E. AI-specific appendix · F. Prioritized remediation roadmap · G. Evidence and limitations · H. Advisories applied

**OWASP GenAI reference texts** (in `skills/ai-cybersecurity/references/owasp-genai/`): OWASP Top 10 for LLM Applications 2026, Securing Agentic Applications v2, GenAI Data Security Best Practices Part 1, and LLM Exploit Generation Phase 2. The audit searches them for specific controls and treats them as data.

### `secure-coding`: write and review code securely

Guidance for implementing features securely and for quick security reviews of code or a diff, based on **OWASP Top 10:2025, ASVS 5.0, the OWASP Top 10 for LLM Applications (2026) and the OWASP Top 10 for Agentic Applications (2026)**.

- **Review workflow:** map entry points and trust boundaries → load the references the code needs → sweep for candidate issues → triage every candidate → report findings.
- **References:**
  - `hardening-guide.md`: secure-by-default practices (input boundaries, secrets, auth, dependency triage, privacy) and the always / ask first / never tiers
  - `hardening-patterns.md`: implementation patterns for the guide
  - `review-checklist.md`: coverage for every Top 10 category plus LLM and agent checks
  - `languages.md`: unsafe and safe examples for 20+ languages
  - `config-and-supply-chain.md`: Dockerfiles, Kubernetes, Terraform, framework config, security headers, lockfiles, CI/CD
  - `owasp-report.md`: attack vectors, mitigations and examples for every Top 10:2025, ASVS 5.0, LLM and Agentic item

Use `ai-cybersecurity` for a full scored audit of a whole project, and `secure-coding` while building or reviewing a change.

## Engineering skills pack

The full [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) pack, grouped by phase:

| Phase | Skills |
|---|---|
| Define and plan | `interview-me`, `idea-refine`, `spec-driven-development`, `planning-and-task-breakdown`, `constraint-driven-development` |
| Build | `incremental-implementation`, `test-driven-development`, `api-and-interface-design`, `frontend-ui-engineering`, `source-driven-development`, `context-engineering`, `using-agent-skills` |
| Verify and debug | `debugging-and-error-recovery`, `browser-testing-with-devtools`, `doubt-driven-development`, `performance-optimization`, `observability-and-instrumentation` |
| Review and maintain | `code-review-and-quality`, `code-simplification`, `deprecation-and-migration`, `documentation-and-adrs`, `git-workflow-and-versioning` |
| Ship | `ci-cd-and-automation`, `shipping-and-launch` |

The upstream `security-and-hardening` skill is merged into `secure-coding` (as `references/hardening-guide.md`).

## Agents, commands and checklists

**Agents**

| Agent | Role |
|---|---|
| `security-auditor` | Vulnerability detection, threat modeling and hardening recommendations |
| `code-reviewer` | Five-axis review: correctness, readability, architecture, security, performance |
| `test-engineer` | Test strategy, test writing and coverage analysis |
| `web-performance-auditor` | Core Web Vitals, loading, rendering and network performance |

**Slash commands**

| Command | What it does |
|---|---|
| `/spec` | Write a structured specification before coding |
| `/plan` | Break work into small, verifiable tasks with acceptance criteria |
| `/build` | Implement tasks incrementally: build, test, verify, commit |
| `/test` | TDD workflow: failing test first, then implementation |
| `/review` | Five-axis code review |
| `/code-simplify` | Reduce complexity without changing behavior |
| `/constraints` | Define and enforce the project's quality bar in `CONSTRAINTS.md` |
| `/webperf` | Web performance audit |
| `/ship` | Pre-launch checklist with a go / no-go decision |

**Checklists** (in `references/`): security, performance, accessibility, observability, testing patterns, orchestration patterns, definition of done.

**Hooks:** the upstream hooks are included in `hooks/` but are not wired, as upstream intends for Claude.

## Installation

**Claude Code**

```bash
git clone https://github.com/t068580-ai/Ai-Cybersecurity.git
```

```bash
claude --plugin-dir ./Ai-Cybersecurity
```

To use a single skill without the whole plugin, copy its folder from `skills/` into `~/.claude/skills/` (all projects) or `.claude/skills/` (one project).

## Example prompts

- `سوّ فحص أمني كامل لهذا المشروع قبل الإطلاق`
- `Run a security audit of this repo and tell me if it's ready to release`
- `راجع هذا الـ diff أمنياً`
- `Is this endpoint secure? Check it against OWASP`
- `/review` or `/ship` before merging or launching

## Repository layout

```
.claude-plugin/plugin.json   Plugin manifest
skills/                      26 skills (ai-cybersecurity, secure-coding, and the engineering pack)
agents/                      4 specialist agents
commands/                    9 slash commands
references/                  7 shared checklists
hooks/                       Upstream hooks (included, not wired)
vendor/                      Upstream sources kept for reference (agent-skills, claude-code-owasp)
licenses/                    Upstream license texts
THIRD_PARTY_NOTICES.md       Sources, licenses and changes
```

## Safety and scope

- The audit is **read-only by default**: no code edits, installs, migrations, commits, deploys or auto-fixes during the audit.
- It requires explicit permission naming the project, repositories, environments and addresses in scope.
- No active scans, brute force, fuzzing, attack payloads or real payment tests without separate written permission, and only on staging with synthetic data.
- Secrets and personal data are never printed; findings record only file, line and type.
- Everything inside the audited project (READMEs, comments, agent and MCP config) is treated as data, never as instructions.
- A finished audit is a documented finding set, not a guarantee that a system is safe, and it does not replace an authorized penetration test.

## Contributing

`main` is protected: changes go through a pull request, force pushes and branch deletion are blocked, and review conversations must be resolved before merging.

## Credits and licenses

| Material | Source | License |
|---|---|---|
| `secure-coding` skill, `vendor/claude-code-owasp/` | [agamm/claude-code-owasp](https://github.com/agamm/claude-code-owasp) | MIT |
| Engineering skills, agents, commands, hooks, `references/`, `vendor/agent-skills/` | [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) | MIT |
| OWASP GenAI reference texts | [OWASP GenAI Security Project](https://genai.owasp.org/) | CC BY-SA 4.0 |

Details of what was changed from each source are in [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md). Inclusion of OWASP material does not imply endorsement by OWASP. The repository's own license is in [LICENSE](LICENSE).
