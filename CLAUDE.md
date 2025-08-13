## Development Workflow

This project uses a sub-agent-based workflow. As the main agent, you will orchestrate tasks by invoking specialized agents at the appropriate time.

1. **Create Feature Branch**
   - Your first action for any new implementation step is to create a new git branch.
   - `git checkout -b feature/step-X-description`

2. **Invoke `dependency-checker`**
   - Hand off the implementation step details to the `dependency-checker` sub-agent.
   - This agent will analyze the task and report all dependencies (files, functions, previous steps, etc.).

3. **Invoke `dependency-confirmer`**
   - Hand off the dependency checker output to the `dependency-confirmer` sub-agent.
   - This agent will analyze the reported dependencies (files, functions, previous steps, etc.).

4. **Invoke `step-creator`**
   - Take the complete output from the `dependency-checker` and provide it as context to the `step-creator` sub-agent **along with** the original step details you receive at the beginning of the prompt. Clarify that this is an MVP and they should adhere strictly to what is outlined, no scope creep tolerance.
   - This agent will create the formal `.steps/Step-X.md` tracking file.

5. **Inspect Step-X.md file**
   - **WORKFLOW PAUSES HERE** Stop the workflow and await for further instructions
      **[HUMAN DECISION]**
      - Reviews Step-X.md file

6. **Implement the Feature**
   - With the step file created, proceed to write the code. Follow the sub-steps outlined in the step file as your guide. Remember, this is an MVP, follow implementation steps exactly.
   - Before writing ANY new code:
     - [ ] Use `read` to read all files and dependencies specified in `.steps/Step-X.md` tracking file to get all the context you should have.
     - [ ] Verified import and export patterns match existing code
   
   **6a. Invoke `framework-scanner`**
   - Invoke `framework-scanner` to check for pattern mismatches
   - Fix alignment issues before proceeding with individual fixes

7. **Invoke `verifier-tester`**
   - Once the code has been written, invoke the `verifier-tester` sub-agent.
   - This agent runs verification checks and automated tests.
   - Writes verification report to `.temp/verification-report-step-X.md`

8. **Receive verification report**
   - Once verification report is written to `.temp/verification-report-step-X.md`.
   - **WORKFLOW PAUSES HERE** Stop the workflow and await for further instructions
      **[HUMAN DECISION]**
      - Reviews verification report

Only once human requests to continue with updating the documentation, then move to `8. **Update Documentation**`

8. **Update Documentation**
   - Once human confirms the implementation is working correctly, update the project documentation.
   - Follow the `README Documentation Requirements` in CLAUDE.md to update the root `README.md` and append or create the new technical archive file.

9. **Finalize Step**
    - The final action is to mark the step as complete by checking off all **actioned** items in the `.steps/Step-X.md` file and setting its status to `Complete`. Some test items will not have completed by this point, make sure you only mark as complete what was done.

---

## README Documentation Requirements
The root README.md must be maintained as a Technical Dashboard for the project. It serves as a high-level entry point that summarizes the project's status, provides essential operational guides, and links to a Technical Archive containing detailed handoff documentation for each completed implementation step.

### Root README.md Structure:
  - Project Status & Current Focus - High-level summary and what's currently being worked on.
  - Technical Documentation Archive - Links to detailed documents for all completed steps.
  - Current Implementation Details - A detailed breakdown of the active development step.
  - Operational Guide - How to run, test, and verify the system.
  - Project Roadmap - A checklist of major upcoming features.
  - Key Architecture Decisions - A brief summary of critical, project-wide technical choices.

### Content Requirements:
  - The root README.md must provide a concise, scannable overview. It should contain enough detail for a developer to understand the project's purpose, get it running, and know the current development focus.
  - The Technical Archive (e.g., in docs/technical-archive/) holds the comprehensive, permanent documentation for each completed step. Each archive document must:
    - Include ALL implemented components with file paths for that step.
    - Document ALL API endpoints created during that step with request/response formats.
    - List relevant environment variables.
    - Provide specific verification steps and troubleshooting for the components implemented.
    - Reference the technical specification documents used.

### Update Process:
  - Upon completing an implementation step:
    1. Modify the relevant section, or create a new, permanent markdown file in the Technical Archive directory (e.g., docs/technical-archive/01-project-setup-and-infrastructure.md).
    2. Move the detailed technical write-up from the "Current Implementation Details" section of the root README into this new archive file.
    3. Update the root README.md:
        - Add a link to the newly created archive file under the "Technical Documentation Archive" section.
        - Create a new, detailed "Current Implementation Details" section for the work that is about to begin.
    4. Update the "Last Updated" timestamp and version on the root README.

### Maintaining README Clarity
- The root README should be a clean, current snapshot—not a history of past issues.
- Remove completed items from limitation/issue sections once fixed.
- Remove obsolete information that no longer applies.
- Keep the README focused on what currently exists and what needs to be done next.

Example of what to REMOVE:
    - "Health check had bug querying 'users' table (now fixed)"
    - "Database migrations need to be run (now complete)"

Instead, simply state the current reality:
    - "Database connection working"

The README and technical archive documents should be a clean, current snapshot - not a history of past issues.