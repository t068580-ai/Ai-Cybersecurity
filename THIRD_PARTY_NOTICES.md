# Third-party notices

This plugin bundles material from the sources below. Each keeps its own license.

| Material | Source | License | Changes |
|---|---|---|---|
| `skills/secure-coding/` (SKILL.md and references), `vendor/claude-code-owasp/` | https://github.com/agamm/claude-code-owasp, commit 1ae29cb | MIT, see `licenses/claude-code-owasp-MIT.txt` | Skill renamed to secure-coding, description and scope section changed, hardening guide added |
| `skills/*` other than ai-cybersecurity and secure-coding, `agents/`, `commands/`, `hooks/`, `references/`, `vendor/agent-skills/` | https://github.com/addyosmani/agent-skills, commit 301b02b | MIT, see `licenses/agent-skills-MIT.txt` | The security-and-hardening skill was merged into secure-coding (its body is `skills/secure-coding/references/hardening-guide.md`), and references to it in active files were renamed. Commands moved from `.claude/commands/` to `commands/`. Hooks are included but not wired, as upstream. |
| `skills/ai-cybersecurity/references/owasp-genai/` | OWASP GenAI Security Project, https://genai.owasp.org/ | CC BY-SA 4.0, https://creativecommons.org/licenses/by-sa/4.0/ | Combined Markdown edition split into one file per document |

OWASP material is used under CC BY-SA 4.0; adaptations of it must stay under the same license. Inclusion here does not imply endorsement by OWASP or the authors.

The upstream plugin manifests kept under `vendor/` were renamed (`*.upstream.json`, folders `upstream-claude-plugin` and `upstream-codex-plugin`) so that this package contains a single plugin manifest. Their content is unchanged.
