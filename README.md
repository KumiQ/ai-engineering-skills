# AI Engineering Skills for Claude Code

**Opinionated role-based skills for AI-assisted software engineering: Product Owner, Architect, Developer/QA and Code Reviewer - ready to drop into Claude Code, the Claude Agent SDK, or any Claude-based coding workflow.**

Think of these as the four cross-functional roles a well-run engineering team already has, written as Claude Code skills you invoke with a slash command. They turn an ambiguous feature idea into a reviewed, tested, merge-ready pull request through four discrete, auditable stages - each stage with its own contract, output format and quality gates.

- `/product-owner` - turn a raw idea into a testable spec (user stories, acceptance criteria, edge cases, non-goals)
- `/architect` - turn the spec into a minimal, implementation-ready design (components, data flow, interfaces, risks, tradeoffs)
- `/dev-qa` - implement with tests, enforce acceptance criteria as a contract, keep CI green
- `/code-reviewer` - structured review for correctness, architecture, security, performance, maintainability

Each skill has one job, one output format and clear handoffs to the next stage. No role expansion. No overlap. No silent scope creep.

---

## Why role-based skills?

Most AI coding assistants collapse "specify, design, build, review" into a single conversation. That works for throwaway work. It breaks on anything that has to ship.

- **Specs invented by the model** → requirements no human agreed to.
- **Architecture decided inline during coding** → inconsistent patterns, no documented tradeoffs.
- **Reviews skipped because "I'm the one who wrote it"** → regressions that only surface in production.

Separating the four roles - even inside a single Claude Code session - forces each stage to produce an artifact the next stage can hold accountable. The Product Owner's acceptance criteria become the Dev/QA's test contract. The Architect's design becomes the thing the Code Reviewer measures the diff against. Ambiguity surfaces at the stage that can cheaply resolve it, instead of leaking downstream.

This mirrors the practices strong engineering organizations already use. These skills make those practices available to every engineer, regardless of their experience with AI tools.

---

## Install

### Claude Code (recommended)

Clone the repo somewhere and symlink each skill into your Claude Code skills directory:

```bash
git clone https://github.com/YOUR-ORG/ai-engineering-skills.git
cd ai-engineering-skills

mkdir -p ~/.claude/skills
ln -s "$(pwd)/skills/product-owner"  ~/.claude/skills/product-owner
ln -s "$(pwd)/skills/architect"      ~/.claude/skills/architect
ln -s "$(pwd)/skills/dev-qa"         ~/.claude/skills/dev-qa
ln -s "$(pwd)/skills/code-reviewer"  ~/.claude/skills/code-reviewer
```

Restart Claude Code. The slash commands `/product-owner`, `/architect`, `/dev-qa` and `/code-reviewer` are now available in any project. Prefer scoping to a single project? Symlink into `.claude/skills/` inside the project instead.

### Claude Agent SDK

Point your agent's skill loader at the `skills/` directory, or copy the four `SKILL.md` files into your own skill bundle. The frontmatter is the standard Claude Code skill format.

### Any other Claude surface

Each `SKILL.md` is a self-contained system prompt. Paste the body into your own agent prompt, custom instruction, or tool definition - the role, workflow and output format transfer directly.

---

## A typical flow

```
User: /product-owner add a per-team usage dashboard with weekly token spend

Product Owner →  PROBLEM
                 TASK SPEC
                 USER STORIES
                 ACCEPTANCE CRITERIA (6 testable checks)
                 EDGE CASES
                 OUT OF SCOPE
                 OPEN QUESTIONS (2 - answered by user)

User: /architect take the spec above

Architect     →  APPROACH, COMPONENTS, DATA FLOW,
                 INTERFACES, CONSTRAINTS, RISKS, TRADEOFFS
                 (or: splits into 3 sub-tasks because the diff would exceed 400 lines)

User: /dev-qa implement sub-task 1

Dev/QA        →  Writes failing tests from acceptance criteria,
                 implements minimal correct solution,
                 commits per logical chunk, pushes green.

User: /code-reviewer review the PR

Reviewer      →  VERDICT, CRITICAL / IMPORTANT / SUGGESTIONS,
                 WHAT'S DONE WELL, VERIFICATION STORY
```

Each stage is auditable in the transcript. If the final diff drifts from the spec, the drift is traceable to a specific stage. That's the point.

---

## Compatibility

- **Claude Code** - primary target. Each skill is invoked with its slash command and auto-triggers when the user's intent matches the `description` field.
- **Claude Agent SDK** - drop the `SKILL.md` files into any SDK-managed skill directory.
- **Claude Desktop / web / API** - paste the skill body as a system prompt or custom instruction.
- **MCP coding guidelines workflows** - pairs well with Model Context Protocol servers that supply organizational engineering standards into the context window. The skills consume whatever standards are in context; the MCP layer decides what gets in.

---

## Design principles

These skills were designed, not improvised. The guiding rules:

1. **One job per skill.** No skill does what another skill does better. Overlap is debt.
2. **Output format is a contract.** Every skill has a MANDATORY output structure so downstream skills (and humans) can parse the output deterministically.
3. **Ambiguity is surfaced, not papered over.** Each skill escalates (asks, flags open questions, requests human review) rather than invent requirements, architecture, or implementation details.
4. **Smallest viable change.** The Dev/QA skill explicitly bans drive-by refactors. The Architect skill explicitly splits oversized tasks before designing them.
5. **CI is a hard gate.** Dev/QA never pushes red. The Code Reviewer never approves a failing build. These are the only automatic kill-switches.

---

## Who this is for

- **Engineering leaders** rolling out AI-assisted coding across multiple teams who want consistent practice, not uneven adoption.
- **Platform and tech leads** maintaining shared engineering standards and looking for repeatable AI workflows that honor them.
- **Individual engineers** who want their AI assistant to behave like a team - specifying, designing, building and reviewing in distinct, inspectable steps instead of one undifferentiated blob.
- **Quality, security and regulated-environment owners** who need review practices and policies to show up in everyday AI work.

---

## What this is not

- Not an AI coding model. These are skills that run on top of Claude.
- Not a replacement for your engineering judgment. The skills surface decisions; humans still make them.
- Not a compliance product. They support consistency and traceability. They don't certify it.
- Not opinionated about your tech stack. The workflow applies to any language or framework.

---

## Related work

- **[Claude Code](https://docs.claude.com/en/docs/claude-code/overview)** - Anthropic's official CLI for Claude. These skills are first-class Claude Code skills.
- **[Claude Agent SDK](https://docs.claude.com/en/api/agent-sdk)** - build custom agents on the same primitives.
- **[Model Context Protocol (MCP)](https://modelcontextprotocol.io/)** - the open protocol for giving models access to your organization's standards, docs and tools.
- **[KumiQ](https://www.kumiq.app/)** - the standards layer behind AI-assisted engineering. KumiQ provides the organizational, team and repository engineering standards these skills consume through an MCP server, so every engineer draws on the same practices in AI-assisted work. This repo was built as part of that work.

---

## Contributing

Issues and PRs welcome. The bar is:

- Every change must preserve each skill's single-job scope.
- No new skills without a clearly distinct role (don't invent a `/qa` skill that overlaps with `/dev-qa`).
- Documentation stays implementation-agnostic - these skills must keep working on any stack.

---

## License

MIT. See [LICENSE](./LICENSE).

---

<sub>Keywords: AI coding standards, AI engineering workflow, Claude Code skills, Claude Code plugins, MCP coding guidelines, AI code review, AI pair programming, AI software development agents, product owner agent, architect agent, dev QA agent, code reviewer agent, autonomous engineering agents, AI adoption engineering teams, Claude Agent SDK, Model Context Protocol, software engineering AI governance.</sub>
