Great — let’s draft some **template prompts for LM Studio** that generate useful planning and technical artifacts, which you can then **bring into ChatGPT for evaluation, refinement, and alignment**.

I’ll also explain:

* ✅ How these tools complement each other
* ⚠️ Gotchas and gaps you’ll need to watch for
* 🧠 Suggested workflow to connect them smoothly

---

## 📝 Prompt Templates for LM Studio

Assuming you're using a **local general-purpose model** like **LLaMA 3**, **DeepSeek**, or **Mixtral** — these are designed for general reasoning, doc writing, and structured text generation.

---

### 1. 🔧 **Technical Spec Draft Prompt**

```text
I’m building a desktop application using Rust and Tauri. Generate a technical specification for the following feature:

**Feature Name:** Monthly Budgeting Tool
**Functionality:** Allow the user to define monthly budget categories, track expenses against them, and forecast remaining budget.
**User Goals:**
- Input budget limits per category
- Upload transaction data (CSV)
- View visual summaries and remaining budget per category
- Adjust budget mid-month

Write a technical specification including:
1. Overview and Purpose
2. Functional Requirements
3. UI Considerations
4. Data Flow or Architecture (optional diagram)
5. API or Module Interfaces (if relevant)
6. Assumptions or Constraints
```

---

### 2. 🗺️ **Product Roadmap Draft Prompt**

```text
Draft a quarterly roadmap for a personal finance desktop app being built in Rust + Tauri. Assume a solo developer with no Rust experience is starting on August 1st, 2025.

The roadmap should:
- Be divided into 3-month increments
- Focus on features like CSV import, budgeting, and forecasting
- Include time for learning Rust and Tauri, prototyping, and testing
- Include 3–5 milestones per quarter
- Format clearly for later editing

Output as a structured markdown list.
```

---

### 3. 📂 **Module Breakdown Prompt**

```text
Break down the following feature into Rust modules and responsibilities:

**Feature:** Transaction Importer
**Goal:** Parse and normalize CSVs from multiple banks and merge them into a consistent format.

Include:
- Module names and responsibilities
- Data structures
- Key functions
- Any error-handling considerations
```

---

## 🔄 Connecting LM Studio with ChatGPT

Here’s a suggested **feedback loop** between tools:

### 🧠 Step-by-Step Workflow

1. **Generate First Draft in LM Studio**

   * Use prompt templates like above
   * Export output as `.md` or `.txt`

2. **Bring Output into ChatGPT**

   * Paste or upload draft
   * Ask:

     > "Evaluate this draft against our roadmap. What's missing or misaligned?"
     > "Refactor this technical spec to fit with our existing architecture."

3. **Collaborate on Revisions**

   * I help tighten the scope, improve structure, and validate planning.
   * You make decisions and edit accordingly.

4. **Back to LM Studio (Optional)**

   * If you need another draft based on the edits, paste my revised version back into LM Studio with a prompt like:

     > "Revise this technical spec based on the following feedback..."

---

## ⚠️ Gotchas to Watch Out For

| Issue                      | Detail                                                                 | Workaround                                                                                            |
| -------------------------- | ---------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Context Loss**           | LM Studio has no long-term memory — can’t "remember" prior discussions | Always include context explicitly in your prompts                                                     |
| **Formatting Drift**       | Local models may not always output clean markdown or structured text   | Use prompt instructions like “Output in GitHub-style markdown”                                        |
| **Model Quality Variance** | Some local models are better at structure, others at reasoning         | Try a few (e.g., DeepSeek vs LLaMA 3) and keep a notes file on strengths                              |
| **Copy-Paste Friction**    | Moving back and forth can feel manual                                  | Use a shared folder or synced markdown notes to streamline this                                       |
| **Code-Specific Prompts**  | Some models hallucinate Rust-specific things                           | Test output against your real codebase before trusting it — we’ll catch this in ChatGPT during review |

---

Here are three `agent.md` files tailored to your current setup and strategy. These will help you consistently instantiate each AI agent (or configure local setups) with clear roles, responsibilities, models, and usage guidance.

---

## 🧠 1. `strategic-agent.md`

**Purpose**: Use with ChatGPT (GPT-4o or GPT-4.5)
**Function**: Oversees project and business direction, validates architecture and planning docs, manages goals and consistency across domains.

```markdown
# Agent: Strategic Product & Guidance AI
## Name: ChatGPT Strategic Advisor
## Model: GPT-4o or GPT-4.5 via ChatGPT (Pro Plan)

### Purpose
This agent acts as an executive-level assistant that helps synthesize ideas, guide long-term planning, and ensure architectural, business, and dev decisions remain aligned.

### Responsibilities
- Maintain strategic oversight over all active domains (Dev, Business, Household, Wellness)
- Validate specs and roadmaps from local agents
- Manage memory and context for evolving projects
- Suggest adjustments to roadmap, priorities, and project architecture
- Coordinate multi-role planning (e.g., software + product + marketing)

### Inputs
- Drafts from LM Studio agents
- Planning goals or updates from user
- Status updates from coding and product sprints

### Outputs
- Consolidated project plans
- Strategy shifts based on changing goals
- Suggestions for next steps

### Prompt Style
- Use structured outlines
- Prefer goal-oriented conversation
- Use long-term memory for continuity

### Notes
- ChatGPT memory should stay clean and scoped by domain (use tags or headers)
- Can initiate multi-agent reviews (“Review this with Roadmap Agent + Coding Agent”)
```

---

## 📐 2. `roadmap-agent.md`

**Purpose**: Run in LM Studio with a local model
**Function**: Produces structured architecture plans, roadmaps, issue breakdowns, and dev specs.

```markdown
# Agent: Architecture & Roadmap Generator
## Name: Local Plan Builder
## Model: Mixtral, Nous Hermes 2, DeepSeek Coder (LM Studio)

### Purpose
Handles focused, one-off planning documents and specs. Works offline. Best for creating technical or roadmap documents quickly for later review.

### Responsibilities
- Generate software architecture proposals
- Draft milestone-based development roadmaps
- Break features into GitHub-ready Issues and Tasks
- Translate user stories into Dev Requirements
- Produce structured markdown, Mermaid diagrams, or task trees

### Inputs
- Product goals and technical context
- Strategic constraints or MVP targets
- Toolchain or language stack notes

### Outputs
- Markdown spec documents
- Roadmap tables with dates and priorities
- Feature/task breakdowns
- Mermaid diagrams

### Prompt Style
- Use tightly scoped, single-document prompts
- Keep sessions focused on one output at a time
- Review documents in ChatGPT before implementation

### Notes
- Rotate models based on task (e.g., Codestral for code-heavy docs)
- LM Studio project directory can be used to store prompt history
```

---

## 💻 3. `coding-agent.md`

**Purpose**: Live code assistant in WebStorm via Junie AI
**Function**: Helps with pair programming, debugging, and code generation with access to multiple models.

```markdown
# Agent: In-IDE Code Assistant
## Name: Junie Dev Agent
## Tool: WebStorm + Junie AI Plugin
## Models: GPT-4 Turbo, Claude 3, DeepSeek Coder (rotate as needed)

### Purpose
This agent supports real-time coding tasks inside the JetBrains IDE. Switch between models for optimized performance and cost.

### Responsibilities
- Help with TypeScript, Rust, Kotlin, or shell scripting
- Explain errors or suggest fixes
- Generate and refactor functions, classes, components
- Support Tauri/Electron projects with best practices
- Translate pseudocode into production-ready code

### Model Suggestions
- **GPT-4**: General-purpose coding, excellent completions
- **Claude 3 Sonnet**: Best for reasoning-heavy debugging
- **DeepSeek Coder / Codestral**: Local fallback or offline usage

### Inputs
- Current buffer or project files
- Inline prompts or "Explain this" questions
- Partial implementations or TODOs

### Outputs
- Live code completions or suggestions
- Error explanations
- Optimized snippets or comments

### Prompt Style
- Natural language (e.g., "Fix this function")
- Annotate unclear code with comments for better results
- Use model switcher if output quality drops

### Notes
- You can hot-swap models in Junie’s sidebar
- For secure or proprietary code, prefer local model
```

