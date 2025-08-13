# Claude Code Sub-Agent Workflow
A development workflow for Claude Code that uses specialised sub-agents to manage complex implementation steps while maintaining context and preventing scope creep.

## Why This Workflow?
As projects grow, Claude Code struggles with context management. Initially, I tried cramming all instructions into CLAUDE.md, but this led to confusion and lost context. This workflow splits responsibilities across specialised sub-agents, each handling a specific part of the development process.

## The Workflow

### 1. **Feature Branch Creation**
Each implementation step starts with a new git branch (`feature/step-X-description`)

### 2. **Dependency Analysis**
- **dependency-checker**: Analyses the task to identify all dependencies (files, functions, previous steps)
- **dependency-confirmer**: Validates the checker's output (was necessary because the checker sometimes hallucinated dependencies)

### 3. **Step Planning** 
- **step-creator**: Creates a formal `.steps/Step-X.md` file with implementation details
- **Workflow Pause**: Had to add as the step creator tended to add unrequested features, that are either planned for in future steps, or just unwanted features.

### 4. **Implementation**
- Main agent implements based on the approved step file, and gets only the context it needs based on the Step-X file
- **framework-scanner**: Checks for pattern mismatches before proceeding

### 5. **Verification**
- **verifier-tester**: Runs tests and creates verification report
- **Workflow Pause**: Human reviews verification report

### 6. **Documentation & Finalisation**
Updates documentation and marks step as complete as per README documentation requirements in CLAUDE.md.

## Sub-Agents

| Agent | Purpose | Why It Exists |
|-------|---------|--------------|
| `dependency-checker` | Identifies all dependencies for a task | Prevents missing context errors |
| `dependency-confirmer` | Validates dependency checker output | Catch hallucianted dependencies |
| `step-creator` | Creates detailed step implementation files | Formalises the plan before coding |
| `framework-scanner` | Detects pattern/framework mismatches | Prevents systematic errors across files |
| `verifier-tester` | Runs verification and tests | Ensures implementation works |
| `debugger` | Root cause analysis when errors occur | Specialised debugging when things break |

## Known Limitations
1. **Scope Creep**: The step-creator agent consistently adds features not in the original request. Manual review checkpoints are necessary.

2. **Hallucinated Dependencies**: The dependency-checker sometimes identifies dependencies that don't exist, hence the need for dependency-confirmer.

4. **Manual Checkpoints**: Found necessary to prevent runaway implementations.

## Documentation Strategy

The workflow includes structured documentation requirements (detailed in CLAUDE.md):
- **Root README**: Maintained as a technical dashboard with current project status
- **Technical Archive** (`docs/technical-archive/`): Detailed documentation for each completed step
- **Step Files** (`.steps/`): Implementation plans and tracking

This ensures knowledge transfer and context gathering for each agent.

## Project Structure

```
.claude/
├── agents/
│   ├── dependency-checker.md
│   ├── dependency-confirmer.md
│   ├── step-creator.md
│   ├── framework-scanner.md
│   ├── verifier-tester.md
│   └── debugger.md
├── CLAUDE.md        # Main workflow orchestration & documentation requirements
├── README.md        # Root technical dashboard
│
.steps/              # Step tracking files
│
.temp/               # Temporary verification reports
│
docs/
└── technical-archive/  # Detailed step documentation (when using full workflow)
```

## Usage
This workflow is designed for MVP development where scope control is critical. The manual checkpoints, while slowing automation, ensure that only requested features are implemented efficiently.

## Note
This is a working solution, not a perfect one. It evolves from real pain points in AI-assisted development and will continue to be refined based on actual usage.