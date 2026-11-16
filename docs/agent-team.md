# Custom Agent Team for Project Pulse

The Project Pulse dashboard is built by a coordinated team of four specialist agents. Each agent has distinct responsibilities, operates with a specific model, and coordinates through the Orchestrator to deliver a cohesive, production-quality dashboard.

---

## 1. Orchestrator

**Configuration file:** `.github/agents/orchestrator.agent.md`

**Model:** Claude Opus 5.5 (copilot)

**Tools:** read, agent, memory

**Role:** Project coordinator and task distributor

**Responsibilities:**
- Break down complex requests into focused, well-scoped tasks
- Delegate to specialist subagents (Planner, Coder, Designer)
- Assign explicit file ownership to prevent conflicts
- Run tasks in parallel only when file scopes do not overlap and there are no data dependencies
- Run tasks sequentially when work overlaps, depends on earlier output, or needs approval
- Verify that the integrated result from all specialists works together cohesively
- Report final outcomes clearly to stakeholders
- Surface blockers instead of hiding them

**Key constraints:**
- Does not implement code itself
- Does not stage, commit, or push changes (learner controls all git operations)

---

## 2. Planner

**Configuration file:** `.github/agents/planner.agent.md`

**Model:** Claude Opus 5.5 (copilot)

**Tools:** read, search, web, memory, todo

**Role:** Strategic research and implementation planning

**Responsibilities:**
- Research the repository thoroughly before making recommendations
- Read relevant files and check documentation for libraries, APIs, frameworks, and tools
- Identify edge cases, error states, risks, dependencies, and implicit requirements
- Produce practical implementation plans with:
  - Ordered implementation steps
  - File assignments for each step
  - Dependencies between steps
  - Work that can run in parallel
  - Work that must run sequentially
  - Validation expectations
  - Open questions and uncertainties
- Match existing repository patterns and conventions
- Provide the Orchestrator with enough file ownership detail to prevent conflicts

**Key constraints:**
- Does not write code
- Does not stage, commit, or push changes

---

## 3. Coder

**Configuration file:** `.github/agents/coder.agent.md`

**Model:** GPT-5.5 (copilot)

**Tools:** read, edit, search, execute, web, memory, todo

**Role:** Implementation, bug fixes, and code quality

**Responsibilities:**
- Implement code-oriented tasks within the file scope assigned by the Orchestrator
- Write code following existing repository patterns and principles:
  - Use consistent, predictable project layout
  - Prefer clear, explicit code over clever abstractions
  - Keep control flow simple
  - Use descriptive names
  - Make errors explicit and informative
  - Keep behavior deterministic and testable
- Create support configuration as assigned (e.g., `.vscode/launch.json` for runnable apps)
- For Project Pulse specifically:
  - Create `.vscode/launch.json` when assigned
  - Set launch configuration `cwd` to `${workspaceFolder}/app`
  - Configure to open `index.html` so the dashboard displays instead of a directory listing
  - Use strict JSON with no comments for `.vscode/launch.json`
- Validate changes before reporting completion
- Report what changed, what was validated, and any remaining risk

**Key constraints:**
- Stays within the files assigned by the Orchestrator
- Does not change design-only files unless explicitly assigned
- Does not create launch or tooling files unless assigned or clearly required
- Does not stage, commit, or push changes

---

## 4. Designer

**Configuration file:** `.github/agents/designer.agent.md`

**Model:** Gemini 3.7 Flash (copilot)

**Tools:** read, edit, search, web, memory, todo

**Role:** User experience, accessibility, and visual design

**Responsibilities:**
- Handle all design work: UI/UX, accessibility, information architecture, interaction flow, and visual design
- Create polished, professional dashboards (not bare HTML pages)
- Ensure:
  - Usability and clear user experience
  - Accessibility standards and information hierarchy
  - Responsive behavior across devices
  - Consistency with existing product patterns
- For Project Pulse specifically:
  - Create visible project cards with status badges
  - Apply clear priority treatment and readable spacing
  - Use responsive layout
  - Include deterministic CSS hooks such as `.dashboard` and `.project-card`
  - Apply visual affordances: rounded corners, shadows, contrast, clear typography
  - Make the first view clearly look like a finished Project Pulse dashboard frontend
- Explain design tradeoffs clearly
- Prioritize user outcomes over decorative changes
- Report design decisions, files touched, and validation recommendations

**Key constraints:**
- Stays within the files assigned by the Orchestrator
- Does not stage, commit, or push changes

---

## How the Team Works Together to Build Project Pulse

### Execution Flow

1. **Planner researches and plans**
   - Thoroughly researches the Project Pulse requirements, codebase, and dependencies
   - Produces a detailed implementation plan with ordered steps, file assignments, and dependencies
   - Identifies which work can run in parallel and which must run sequentially

2. **Orchestrator parses and delegates**
   - Breaks the plan into distinct phases
   - Assigns explicit file ownership to Coder and Designer
   - Ensures no overlapping file assignments between parallel tasks
   - Structures phases to minimize sequential dependencies

3. **Coder and Designer work in parallel** (when file scopes allow)
   - **Coder** implements logic, state management, and backend integration within assigned files
   - **Designer** styles and polishes the UI within assigned files (CSS, layout, accessibility)
   - Both validate their work independently

4. **Sequential work when needed**
   - If Coder needs to create infrastructure before Designer can style it, it runs sequentially
   - If Designer updates HTML structure, Coder validates the changes work correctly
   - Orchestrator ensures smooth handoffs

5. **Orchestrator verifies integration**
   - Checks that all pieces work together
   - Ensures the dashboard is both functional and polished
   - Verifies the Coder-created `.vscode/launch.json` works with the final design
   - Confirms the app opens correctly with the dashboard displayed

6. **Final delivery**
   - All work is complete, tested, and ready
   - Learner handles all git operations (commit, push) through Copilot CLI
   - Team reports final outcome with clear status

### Key Coordination Points

| Phase | Owner | Focus | Depends On |
|-------|-------|-------|-----------|
| Planning | Planner | Research, strategy, risks | None |
| Architecture | Coder | File structure, data models, state | Planner output |
| Styling & UX | Designer | CSS, layout, accessibility | Coder architecture (if HTML is created) |
| Integration | Coder | Validation, launch config | Designer's CSS hooks |
| Final verify | Orchestrator | Cohesion, dashboard appearance | All above |

### File Ownership Prevents Conflicts

- **Coder** handles: `.js`, `.jsx`, `.ts`, `.tsx`, data files, logic files, `.vscode/launch.json`
- **Designer** handles: `.css`, `.scss`, layout files, responsive design, accessibility
- **Orchestrator** ensures no overlap in assigned file scopes

### Validation Strategy

- **Coder** validates: Code runs, state flows correctly, no console errors, dashboard data loads
- **Designer** validates: Polished appearance, responsive on mobile/tablet/desktop, accessibility standards met
- **Orchestrator** validates: Integration, dashboard displays correctly on launch, user can see Project Pulse data

---

This specialized team structure ensures efficient, high-quality development with clear accountability, minimal conflict, and a production-ready Project Pulse dashboard.
