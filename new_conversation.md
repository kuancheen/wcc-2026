# Core Objective
You are an expert software engineering agent. To maximize token efficiency, prevent context bloat, and maintain an accurate project state across sessions, you must manage and rely on two specific repository artifacts: `implementation_plan.md` and `walkthrough.md`. **Do not rely on chat history for long-term project memory.**

---

# The Two Artifacts
1. `implementation_plan.md`: A strict checklist of pending technical tasks, architecture decisions, and remaining deployment/verification steps.
2. `walkthrough.md`: A living technical document explaining the current system architecture, active file structures, completed features, environment setup, and lessons learned.

---

# Operational Workflow (Every Conversation)

## Step 1: Initialize Context (Read State)
At the very beginning of any new session or interaction:
- Locate and read `implementation_plan.md` and `walkthrough.md`.
- Read them to understand the exact state of the project, architecture, and current blockers.
- **Do not ask the user for context that is already documented in these files.**

## Step 2: Planning & Execution
- Before writing code or running deployment commands: if the current request changes the scope, update `implementation_plan.md` with new granular steps first.
- Execute tasks one specific, verifiable block at a time.

## Step 3: Atomic State Handover (Clean-up & Move)
As soon as a feature or task is successfully implemented, tested, and verified, you must immediately update both files in a single pass:
1. **CHECK OFF / REMOVE** the completed task from `implementation_plan.md`.
2. **ADD** the technical details of the completed feature (file paths, how it works, configuration changes, external endpoints) into `walkthrough.md`.
3. **LOG** any new gotchas, architectural decisions, or lessons learned into the *Lessons Learned* section of `walkthrough.md`.
4. Proceed immediately to the **Git Commit & Sync Phase**.

---

# Git Commit & Sync Rule
Every time a task block or sub-task is completed:
- **What to stage**: Stage and commit the updated `implementation_plan.md`, `walkthrough.md`, all newly created files, and modified source files.
- **Commit Message Format**: Use clear, descriptive conventional commit messages (e.g., `feat: deploy static site to /wcc and update project artifacts` or `fix: resolve relative asset paths in index.html`).
- **Sync**: Push changes to the remote repository (`origin/main`). Note: For repositories with large binary assets (e.g., high-res gallery images), ensure `git config http.postBuffer 524288000` is configured if pushing over HTTPS.

---

# Project-Specific Rules & Guidelines
- **Project Structure**:
  - `wcc/`: Standalone static website tracked by GitHub (`https://github.com/kuancheen/wcc-2026`).
  - `wcc_site.zip`: Single-archive package ready for SFTP extraction.
- **Server Environment**: Apache 2.4 hosting WordPress at `/var/www/html/` with the conference site served from `/var/www/html/wcc/`.
- **Key Integrations**:
  - Registration form must always route to Luma (`https://luma.com/o6mqew3f`).
  - Contact links must be clean `mailto:ywa@actschurch.org` without proprietary CDN dependencies.
  - Open Graph preview metadata must point to `/assets/images/card.jpg`.

---

# Formatting Rules
- Keep `implementation_plan.md` concise: a crisp list of checkboxes (`- [ ] task`) grouped by phase or priority.
- Keep `walkthrough.md` structured and descriptive: include code blocks, directory trees, configuration snippets, and entry-point descriptions so any fresh agent can read it and instantly understand the entire codebase.
