---
name: wrap-up
description: Finish a baseball-note development session by checking the working tree, verification, tickets and docs, then report deployment needs and a concrete next-session handoff. Use when the user asks to wrap up, end, hand off, or summarize completed repository work.
---

# Wrap Up

Close the current work without committing or pushing unless the user explicitly requests it.

## Procedure

1. Read `AGENTS.md`, `docs/plan.md`, and the active ticket. If no ticket exists for substantial work, create or identify one before finishing.
2. Inspect `git status --short`, `git diff --stat`, `git diff --check`, and the relevant diff. Do not revert unrelated user changes.
3. Run the verification appropriate to the change:
   - Code or configuration: `npm run lint`, `npm run typecheck`, and `npm run build` unless there is a documented reason to omit one.
   - Documentation only: check local Markdown links, file paths, contradictions, and stale references. Code verification may be omitted.
   - Auth, authorization, RLS, secrets, or personal data: add a security-focused diff review and state which manual role checks remain.
4. Update the active ticket status, checklist, and progress log with the real result. Update requirements, design, operations, or ADR files only when the change made them stale.
5. Determine whether Vercel Preview, Production, Supabase migration, backup, external-service configuration, or device testing is still required. Never claim an external check that was not performed.
6. Check that no credential, token, account detail, or secret value was added to tracked files or the report.

## Final Report

Keep the report concise and include:

- Completed changes and important file paths
- Verification run, success/failure, and any unverified scope
- Preview/Production reflection required, including backup or migration order
- Remaining work or blocker
- Whether the current session can continue or a new session is recommended, with a short reason
- Suggested session name: `bbnote_NNN-topic`
- A ready-to-use Japanese prompt for the next session that names the ticket, objective, constraints, and first files to read

Use the active ticket number for `NNN`; use `000` for work with no ticket. Use a short ASCII kebab-case topic. Do not commit, push, merge, deploy, change external settings, or expose environment values without explicit user direction.
