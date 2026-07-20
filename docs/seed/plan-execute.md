# Plan and Execute — Agentic AI for Software Engineering Technique

**Author:** Maciej Małecki | Capgemini | V1.0

---

## 1. Overview and Rationale

### 1.1 Purpose and Target Audience

This document describes a **Plan and Execute** technique for Agentic AI in software engineering.

**Target audience:** Software Architects, Tech Leads, Software Engineers, AI Leads/Champions, Business Analysts, Quality Engineers.

> **Warning:** The technique is in the experimental phase and may be subject to change.

### 1.2 Definition

Plan and Execute is a context engineering technique consisting of:

- **Planning phase:** Creates plans, plan refinement
- **Execution phase:** Changes/creates source files, runs CI tools

Both phases are delivered via an initial **Orchestration phase** that bootstraps the project with dedicated prompts and agents.

### 1.3 Why Separation of Planning and Execution?

**Planning phase:**
- Strategic thinking (architecture first)
- Iterative refinement
- Detailed blueprint

**Execution phase:**
- Implementation focus
- Reduced context switching
- Consistent quality
- Output is working code

### 1.4 SDLC Integration

Agent workflow: **Architect agent → Designer agent → Developer agent → Reviewer agent**

Outputs at each stage:

| Stage | Output |
|-------|--------|
| Architect agent | High level plan |
| Designer agent | Low level plan |
| Developer agent | Code base |
| Reviewer agent | Review report |

Includes Refine/Iterate loops between stages.

### 1.5 Human in the Loop

All artefacts are human-readable. Review at each stage is mandatory.

> **Golden rule:** AI agent only runs when all uncertainties are resolved.

### 1.6 Hierarchy of Plans

| Agent | Output |
|-------|--------|
| Enterprise agent* | Enterprise arch spec |
| Analyst agent* | Requirement spec |
| Architect agent** | High level plan |
| Designer agent | Low level plan(s) |

Plans are self-contained at each level.

(\* future, \*\* partially implemented)

### 1.7 Feedback Loop Between Execution and Planning

Teams can switch between planning and execution phases during feature development. Reviewer agent generates reports that serve as input for the next planning iteration.

### 1.8 Application Areas

**Tested:**
- Medium code bases (~100K LOC)
- Mainstream technologies

**Not tested:**
- Large code bases (~1M+ LOC)
- Proprietary technologies

**Not applicable:**
- Green field projects (no existing code base to analyse)

### 1.9 Known Limitations

1. Context window limits restrict the amount of code that can be analysed at once.
2. Large plans need step-by-step execution rather than all-at-once.
3. Obfuscated code needs compensating instructions to help the agent understand the codebase.
4. The anthropomorphic agent approach may be too broad for some specialised tasks.

---

## 2. Orchestration

### 2.1 Preconditions

- Legal disclaimer for GitHub Copilot usage
- Agent onboarding with `copilot-instructions.md` file
- GitHub Copilot can generate initial instructions by scanning the repo
- Use `applyTo` pattern for larger projects to scope instructions to specific folders

### 2.2 Orchestration Phase

Creates dedicated prompts for the project. Two variants:

1. Custom agents
2. Prompt templates

Meta prompts described in Appendix A (prompts) and Appendix B (agents).

**Guidelines:**

1. Use strong model (Claude Sonnet 4.5)
2. Start new conversation for each meta-prompt
3. Paste meta-prompt at top of conversation
4. Execute in order
5. Can customise outputs

### 2.3 Orchestrating via Prompt Templates

Prompt templates are `.md` files in `.github/prompts/` folder, invoked with slash (`/`) notation.

**How to orchestrate:**
- Use Appendix A meta-prompts
- Execute one by one with Claude Sonnet 4.5
- Accept file creation, commit to repo

Three prompt templates: `/plan`, `/plan-update`, `/execute`

- Claude Sonnet 4.5 for planning
- Claude Haiku 4.5 or Sonnet 4.5 for execution

### 2.4 Orchestrating via Custom Agents

Custom agents (previously 'chat modes') in GitHub Copilot, VS Code only.

**How to orchestrate:**
- Use Appendix B meta-prompts
- Execute one by one
- Accept file creation in `.github/agents/` folder, commit

Three agents: **Designer**, **Developer**, **Reviewer**

- Sonnet 4.5 for Designer/Reviewer
- Haiku 4.5 or Sonnet 4.5 for Developer

---

## 3. Planning Phase

### 3.1 Introduction

Two steps:

1. Create plan
2. Refine plan (#1, #2, ... #N)

Refinement is needed because initial information is usually insufficient. Information gaps lead to hallucinations in generated code.

### 3.2 Creating a New Low-Level Development Plan

- Switch AI to planning phase for structured information gathering
- Result is a low-level development plan document (structured `.md` file)
- Plan can be shared across conversations

> **Note:** GitHub Copilot's built-in 'planning mode' is different — use regular agent mode instead.

### 3.3 How to Switch AI Agent into Planning Phase

**Prompt-template variant:**

1. Select Agent mode
2. Choose Claude Sonnet 4.5
3. Use `/plan` template

**Custom agent variant:**

1. Select Designer agent
2. Choose Claude Sonnet 4.5
3. Type task description as prompt

### 3.4 Why Does It Work?

The meta-prompt doesn't teach AI planning — the vendor has built-in planning capability.

**Key enhancement:** interaction part + order to externalise plan into MD file with fixed structure.

Plan includes:

- Feature description
- Root cause analysis
- Relevant code parts
- Questions (self-reflection + questions for others)
- Detailed execution plan

Root cause analysis reveals existing shortcomings in the codebase.

### 3.5 How to Properly Describe a Feature

Heuristics:

1. Be specific with language matching code concepts
2. Define purpose of functionality
3. Define edge cases
4. For GUI features, refer to existing features
5. For GUI features, attach mock-up screenshots

### 3.6 Task Granularity Recommendations

Works best with small, focused tasks.

Heuristics:

1. Single use case with edge cases as scope
2. Don't combine different functionalities in one plan
3. For larger features, use separate plans (backend first, then frontend)

> **Risk:** Separate plans may not integrate well. Future versions will address this via high-level design documents.

### 3.7 The Purpose of Refining Stage

- Plan Refining stage adds detail to the initial plan
- AI detects gaps and formulates open questions
- Keeps human in the loop for decision-making
- Can provide additional information iteratively
- Can refine multiple times, with execution phases following
- Recommended to stop execution and return to planning when discovering missing elements

### 3.8 Answering Questions

The MD plan document has a Questions section. Human answers open questions precisely.

- **Prompt-template variant:** Use `/plan-update` with interactive mode or provide all answers at once.
- **Custom agent variant:** Use Designer agent, attach plan, ask interactively or provide all answers.

### 3.9 Adding New or Missing Information

- Can add information at any time using the appropriate agent or prompt template
- New information may trigger new questions from the AI
- Both variants use the plan-update workflow

---

## 4. Execution Phase

### 4.1 Introduction

The execution phase consists of a series of execution requests followed by a review.

```
Execute #1 → Execute #2 → ... → Execute #N → Review
```

- Smaller plans: single execution request usually sufficient
- Larger plans: execute each step individually
- Commit changes after each execution
- Review phase at the end is highly recommended

### 4.2 The Purpose of Execution Phase

Generate code from the low-level development plan. 'Generation' means writing new code or modifying existing code.

**Recommended LLMs:** Claude models, CODEX models, Grok Code

- Plans can be executed whole or step-by-step
- Start slowly with step-by-step, speed up as confidence grows
- Initially review each change individually, then larger portions

### 4.3 Executing the Plan

**Prompt-template variant:**

1. Select Agent mode
2. Select Claude Haiku 4.5 (Sonnet for difficult cases)
3. Use `/execute` prompt template

**Custom agent variant:**

1. Select Developer agent
2. Select Claude Haiku 4.5
3. Type task content as prompt

**Example commands:**

- `/execute` Execute whole plan autonomously
- `/execute` Execute step 2
- `/execute` Execute whole plan but ask for confirmation after each step

### 4.4 Important Notes on Code Generation

This technique can result in significant code generation.

**Caveats:**

1. Pull requests still require human review — reserve time for this
2. Quality is ensured by providing tools (compilers, linters, test runners) and ensuring the AI agent uses them
3. Execute/Developer agent must always work with a development plan attached; update plan with status after each step

### 4.5 The Purpose of Review Step

AI review supplements mandatory manual code review.

The Reviewer agent checks for:

- Missing test coverage
- Duplicate code
- Implementation completeness against the plan (if requested)

Generates a review report in MD format. This report can be fed to the Designer agent to generate a plan for corrective actions.

### 4.6 Reviewing the Implementation

**Prompt-template variant:**

1. Select Agent mode
2. Select Claude Sonnet 4.5
3. Use `/review` prompt template

**Custom agent variant:**

1. Select Reviewer agent
2. Select Claude Sonnet 4.5
3. Specify review scope

Three predefined scopes: **branch**, **plan**, **folders**

**Examples:**

- `/review` Review current branch from develop junction
- `/review` Review implementation based on development plan attached
- `/review` Review content of `src/orders` package

---

## Appendix A: Meta Prompts (Prompt Templates)

### A.1 Meta-prompt: plan

Creates the `/plan` prompt template (`.github/prompts/plan.prompt.md`).

**Requirements:**

1. Plan stored in `.ai` folder as `.md` file
2. Normalised structure (one-shot)
3. Agent asks for issue number / feature name / task spec
4. Agent scans codebase and docs
5. Plan includes: feature description, root cause analysis, relevant code, questions, execution plan
6. Steps contain deliverable increments safe to merge
7. Agent asks interactively if info is missing or unclear

### A.2 Meta-prompt: plan-update

Creates the `/plan-update` prompt template.

**Requirements:**

1. Ask for plan to update
2. Ask for scope of update (spec change, answer questions, acceptance criteria)
3. Consistently update all parts
4. Mark answered questions
5. Ask interactively for clarifications

### A.3 Meta-prompt: execute

Creates the `/execute` prompt template.

**Requirements:**

1. Ask for plan to execute
2. Ask which steps/phases to implement (ranges or 'all')
3. Ask if agent should confirm after each step
4. Update plan after execution (mark executed/skipped steps)

### A.4 Meta-prompt: review

Creates the `/review` prompt template.

**Requirements:**

1. Three review scopes (plan / branch / folders)
2. Check adherence to coding/architecture rules
3. Check plan adherence
4. Verify test coverage
5. Check separation of concerns
6. Check for code duplications
7. Verify code comments
8. Fixed-structure report
9. Store report in `.ai` folder

---

## Appendix B: Meta Prompts (Custom Agents)

### B.1 Meta-prompt: Designer

Creates the Designer custom agent (`.github/agents/designer.agent.md`).

Agent creates/updates development plans.

**Features:**
- New plan or existing plan workflow
- Q&A mode for interactive refinement
- Normalised plan structure
- Interactive clarification

**When creating:** asks for issue number, feature name, scans codebase.
**When updating:** finds existing plan, asks scope, ensures consistency.

### B.2 Meta-prompt: Developer

Creates the Developer custom agent (`.github/agents/developer.agent.md`).

Agent implements development plans.

**Requirements:**

1. Ask for plan to execute
2. Ask which steps/phases (ranges or 'all')
3. Offer confirmation after each step option
4. Update plan with execution status (mark executed/skipped)

### B.3 Meta-prompt: Reviewer

Creates the Reviewer custom agent (`.github/agents/reviewer.agent.md`).

Agent reviews generated code. Same 9 review checks as the prompt template version:

1. Plan/branch/folders scope
2. Coding rules adherence
3. Plan adherence
4. Test coverage
5. Separation of concerns
6. No duplications
7. Correct comments
8. Fixed report structure
9. Store in `.ai` folder

---

## Closing

### About Capgemini

Capgemini is a global business and technology transformation partner. 340,000+ team members in 50+ countries. 55+ year heritage. 2023 global revenues: EUR 22.5 billion.

**Contacts:**

- **Alexander Vogel** — Team Lead, Agentic Software Engineering Bootstrap
- **Maciej Małecki** — Agentic Software Engineering Bootstrap Team
- **Stuart Williams** — Agentic Software Engineering Bootstrap Team
