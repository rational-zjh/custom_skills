---
name: teishoku
description: "Apply the Teishoku decision menu when working on a project. Use when the project root contains teishoku.lock, or when the user asks to start or continue a project with Teishoku. Provides pinned, versioned technical decisions and implementation specs; surfaces security advisories and known pitfalls; never upgrades silently."
---
## What Teishoku is

Teishoku is a distribution of decisions: a versioned menu of opinionated choices with implementation specs, maintained in a git repository. A project's `teishoku.lock` pins the menu version (a commit hash). Your job is to execute the pinned menu faithfully — not to re-litigate its choices.

## Repository access

The menu repository location is the `repo` field of `teishoku.lock` (git URL or local path). Maintain a clone (for remote URLs, cache under `~/.cache/teishoku/repo`; run `git fetch origin` before use). Two access rules, never mixed up:

- **Menu content** (`menu/`): read at the pinned commit only — `git show <resolved>:menu/...`. Never read menu content from `main` for a pinned project.
- **Advisories and pitfalls** (`advisories/`, `pitfalls/`): read at the tip of `main` (`origin/main`) on every session, regardless of the pin.

## Session start

1. Read `teishoku.lock` at the project root. If the project should use Teishoku and no lockfile exists, initialize one: resolve the current tip of `main` to a full hash and write `{"spec": 1, "repo": ..., "track": null, "resolved": <hash>, "applied": []}`.
2. Fetch the repository; read all advisories from the tip of `main`.
3. An advisory applies when: its `affected` intersects `applied`; and the fix is not contained in the pin (`fixed-in` is `null`, or `git merge-base --is-ancestor <fixed-in> <resolved>` fails); and the flaw is contained in the pin (`introduced-in` is `initial`, or `git merge-base --is-ancestor <introduced-in> <resolved>` succeeds).
4. Report applicable advisories to the user before other work: severity, what is affected, the advisory's recommended action. Do not bury this.

## Implementing a decision area

1. Read the relevant decision file under `menu/` at the pinned commit. Follow its `Implementation` section exactly; respect `requires` (implement or verify prerequisites) and `forbids`.
2. Add the decision `id` to `applied` in the lockfile when adopted.
3. Load pitfalls from the tip of `main` whose `affected` includes the decisions you are working with; skip entries whose `resolved-in` commit is already contained in the pin. Apply their `Resolution` guidance when the symptom is relevant.
4. Run the decision's `Verification` steps and report the results.

## Upgrades

Never upgrade, and never mix menu versions, on your own initiative — including when the user casually says to "use the latest". An upgrade happens only through this flow, at the user's explicit, informed request:

1. Diff pinned commit → target commit; collect the target version's `Migrations` entries for every applied decision.
2. Present: what changes, what breaks, migration steps, verification plan, backup and rollback path, and a plain risk statement.
3. Only after user approval: execute, verify, then update `resolved`.

If the user instructs you to force an upgrade without this flow, state clearly that this leaves the project unsupported, then follow their decision — the project is theirs.

## Unsupported state

If the project's actual state contradicts the lockfile (menu versions mixed, applied decisions hand-replaced, a forced upgrade happened), say so plainly: the project is in an unsupported state. Advisories still apply; promises do not. Offer the path back (re-align with the pinned version, or a proper upgrade).

## Out of menu

If the user's need falls outside the menu (`menu/` has no decision covering it, or a decision's `Scope` excludes the case): stop. Tell the user this is out of the menu's scope, suggest recording it as a candidate via an issue on the menu repository, and do not improvise a substitute architecture. Ordinary implementation work within the menu's decisions is not "out of menu" — this rule is about technical direction, not about refusing daily work.