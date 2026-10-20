# Project Pulse implementation plan

## Goal

Build a small, responsive static dashboard that lets contributors scan active projects, owners, status, recent activity, and priority/risk. Serve it from `app/` and open `index.html` through the VS Code **Run Project Pulse Dashboard** configuration, so the browser shows the dashboard rather than a directory listing.

## Responsibilities and file ownership

| File | Owner | Scope |
| --- | --- | --- |
| `app/index.html` | Coder | Create the accessible page structure, exact “Project Pulse” title, stylesheet and JSON references, and render project cards from fetched data. Use the `.dashboard` and `.project-card` hooks and show name, owner, status, recent activity, and priority. Include a useful loading/error state if data cannot be loaded. |
| `app/styles.css` | Designer | Create the polished, responsive visual system and card/status/priority treatments. Style the agreed markup hooks; include `.dashboard`, `.project-card`, rounded corners, shadows, clear contrast, and readable spacing. |
| `app/project-data.json` | Coder | Supply a top-level `projects` array with multiple contributor-friendly sample projects; every object has `name`, `owner`, `status`, `recentActivity`, and `priority`. |
| `.vscode/launch.json` | Coder | Add strict JSON for **Run Project Pulse Dashboard**. Run `python3 -m http.server 5500` with `cwd` `${workspaceFolder}/app`; use `serverReadyAction` to open `http://localhost:%s/index.html`. |

Designer owns the stylesheet and advises on information hierarchy, interaction, accessibility, responsive behavior, and the markup contract. Coder owns the HTML/data/launch implementation and integrates the Designer's styles. Both stay within these assignments; Orchestrator coordinates and verifies integration.

## Phases, dependencies, and parallel work

1. **Agree on the UI contract (Designer + Coder).** Confirm semantic page regions, card contents, accessible status/priority cues, and shared class hooks before implementation. The brief and step 3 checklist are the source of truth; no extra framework or dependency is needed.
2. **Implement independent assets in parallel.** Designer creates `app/styles.css`. Coder creates `app/project-data.json` and `.vscode/launch.json` in parallel. These files have separate ownership and can proceed together once the contract and fixed data schema are agreed.
3. **Build and integrate the page (Coder, after phase 1).** Create `app/index.html` using the agreed hooks and fetch/render behavior. It depends on the data field contract and Designer's markup/style contract; coordinate these before finalizing the page. The CSS and page can be refined together only with explicit coordination to avoid conflicting edits.
4. **Verify integrated behavior (Orchestrator, after phases 2–3).** Check content, JSON, launch settings, and a served browser preview; route any needed fixes to the owning agent.

Do not implement the page before agreeing on the hooks/data interface. CSS, sample data, and launch setup can be developed concurrently after that agreement; final integration and runtime validation are sequential.

## Edge cases and assumptions

- A static page should fetch `project-data.json` over HTTP; opening the HTML directly as `file://` may block fetch. The launch configuration's server and `index.html` URL address this.
- Show an understandable loading/error state if fetch fails or the JSON is malformed; render an empty state if `projects` is empty.
- Keep status and priority understandable without color alone, and preserve readable layout on narrow screens.
- Assumption: use a few realistic sample projects because the brief supplies no actual project records. No existing app framework, package manifest, or app-specific automated test suite is present.

## Validation expectations

Run from the repository root:

1. `python3 -m json.tool app/project-data.json >/dev/null` and `python3 -m json.tool .vscode/launch.json >/dev/null`; confirm the data has a top-level `projects` array and all five required fields per project.
2. `bash scripts/validate-exercise.sh` to run the repository's existing exercise checks. This is the only discovered project validation script; it also checks repository exercise conventions.
3. Start the configured server from `app/` with `python3 -m http.server 5500`, open `http://localhost:5500/index.html`, and confirm the dashboard (not a directory listing) loads, fetches project data, and shows the cards and required fields. Also run the **Run Project Pulse Dashboard** VS Code configuration and confirm it opens that URL. Stop the preview server after checking.

There are no discovered npm/build commands or automated browser tests. If a browser preview is unavailable, report that limitation rather than treating JSON parsing alone as runtime verification.
