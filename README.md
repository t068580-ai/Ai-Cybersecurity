# AI Cybersecurity

Security for software, SaaS, serverless and AI projects, plus an engineering skills pack.

## What it includes

- **ai-cybersecurity** skill: read-only, evidence-based audit against 84 controls (SEC-01 to SEC-84) and an AI appendix (AI-01 to AI-12), with scoring, a release decision and a report. It loads the live watch list at https://claude.ai/artifact/6jtTqMXVxPyQx9W3XYVcop, which a daily scheduled task keeps current. Formerly the security-audit-84 skill.
- **secure-coding** skill: secure implementation and quick diff reviews using OWASP Top 10:2025, ASVS 5.0, LLM Top 10 2026 and Agentic Top 10 2026.
- OWASP GenAI reference texts: LLM Top 10 2026, Securing Agentic Applications v2, GenAI Data Security Best Practices Part 1, and LLM Exploit Generation Phase 2.
- The full addyosmani/agent-skills pack: engineering skills (spec, planning, TDD, review, performance, CI/CD, shipping and more), four agents including security-auditor, and slash commands.

## Notes

- The agent-skills hooks are included but not active, as upstream intends for Claude.
- Watch-list proposals change the ai-cybersecurity skill only after you approve a repackaged plugin file.
- See THIRD_PARTY_NOTICES.md for sources and licenses.
