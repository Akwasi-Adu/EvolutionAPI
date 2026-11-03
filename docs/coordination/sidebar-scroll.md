# Documentation sidebar scrolling

- Objective: Make the docs endpoint sidebar scroll independently while preserving the current live site as the maintained baseline.
- Repository/worktree: `C:\Users\Akwas\Documents\EVOAPI PROJECT\GEM-HIS` (`akwasi-adu/evolutionapi`).
- Branch: `codex/docs-sidebar-scroll`.
- Base: `b83c34b8a15e2a0497f9bd38dcc57c8ee49caf2a` (`origin/main`, fetched 2026-11-03).
- Completed commits: `6cbf4eb1ae3c3aeca677dddc7c19e39b56441a76` (`fix(docs): make endpoint sidebar scroll independently`), `05d2d09e754e89c528870392948912320bde0ccd` (`docs: record sidebar baseline and verification`), and `c925c99c93cc64a51580e41d8a67526a15c55496` (`docs: record sidebar branch publication status`). The PR approval and publication tracking links are recorded in the subsequent ledger commit.
- Scope: Sidebar layout in `docs.html`; this coordination document. No API schema, homepage, pricing, or download changes.
- Migrations: None.

## Baseline and discarded work

- All 18 publicly served tracked files match `origin/main` (text compared with normalized line endings; binary assets byte-for-byte), including `index.html`, `docs.html`, `swagger.json`, and all buy pages. `CNAME` is not served (HTTP 404); its tracked domain matches the live hostname.
- At initial inspection there were no local commits ahead of `origin/main`; the new sidebar fix is the only implementation commit in this workstream.
- User authorized discarding previous unpublished local work: restored `buy/getting-started.html` and `swagger.json`; moved the previous untracked `docs/` artifacts outside the repository.
- Recovery archive outside the working tree: `C:\Users\Akwas\.codex\backups\EvolutionAPI\2026-11-03-sidebar-112129.zip`. Original untracked artifacts are also in adjacent `discarded-docs-2026-11-03/`.
- Live-file comparison evidence: `C:\Users\Akwas\AppData\Local\Temp\evoconnect-sidebar-20261103-adzrxsen\baseline-comparison.json`.

## Findings and verification

- Existing `.nav-scroll` has `overflow-y: auto`, but its `#sidebarContent` parent is an unconstrained block, allowing it to grow beyond the viewport instead of giving the list a scrollable height.
- No repository AGENTS.md, contributing guide, package scripts, or tracked CI workflow was found. This is a static GitHub Pages site.
- Required verification for this change: reproduce the desktop overflow; verify independent sidebar scrolling and final endpoint access; verify search, navigation, resizing, and mobile menu scrolling; inspect desktop/mobile screenshots and run `git diff --check`.
- Changed files: `docs.html` and `docs/coordination/sidebar-scroll.md` only. The docs change makes `#sidebarContent` a bounded flex column, keeps the logo/search from shrinking, and gives the endpoint list the remaining scrollable height. The mobile menu uses the same layout and is capped at the current viewport height.
- Verification completed 2026-11-03 on the final local `docs.html` change, based on commit `b83c34b8a15e2a0497f9bd38dcc57c8ee49caf2a`:
  - Verified `docs.html` tree content (LF-normalized SHA-256): `c2254a9c819744576550cf5d57ddea1b214fcba9a2c16acf0ffd1d3993d09492`.
  - Chromium 141 through the existing Portfolio Puppeteer installation, serving this checkout through a temporary loopback-only HTTP server. No dependencies or test tooling added to this repository. Analytics requests blocked in the verification browser.
  - Command: `node C:\Users\Akwas\AppData\Local\Temp\evoconnect-sidebar-20261103-adzrxsen\check-sidebar.cjs verify` (quote the path when running in a shell). Passed.
  - Original desktop reproduction: navigation grew to 4,935px in a 900px viewport; wheel scrolling did not scroll the endpoint list.
  - Fixed 1440x900 desktop: list viewport 723px, content 4,935px; wheel scroll moved the list 3,000px while the main content, page, and search position stayed unchanged.
  - All 133 endpoint links remain present. The final Warehouse endpoint can be reached and clicked to navigate the main content.
  - Search filtering, no-results message, clearing the filter, and `/` focus shortcut passed. Sidebar drag resizing to 460px retained scrolling.
  - Scrolling and final-endpoint navigation passed at 996x760, 769x600, 390x844, 740x360, and 375x480. The mobile menu opens/closes and closes on endpoint selection; the search stays visible while the list scrolls, including landscape.
  - Visually inspected desktop top/bottom and mobile portrait/landscape screenshots. Evidence and full metrics are in the temporary directory above (`verify-results.json`, `verified-desktop-top.png`, `verified-desktop-bottom.png`, `verified-mobile-menu.png`, and `verified-mobile-landscape.png`).
  - `git diff --check`: Passed. Homepage, API schema, download pages, and all other tracked content remain identical to the live baseline.
  - Publication preflight: fetched `origin/main` again on 2026-11-03; target remained `b83c34b8a15e2a0497f9bd38dcc57c8ee49caf2a`. Reviewed the full candidate diff and reran the browser checks before committing implementation `6cbf4eb`; all checks above passed again.
  - The first rerun exposed a font-loading timing issue in the temporary browser verification script (search top changed from 111px to 107px as webfonts loaded). The script now waits for `document.fonts.ready` before measuring. The unchanged application code then passed the full rerun; no assertions were removed or relaxed.
- Known pre-existing failure: The unchanged live `renderDocs()` replaces `#mainContent` (including `#loader`), then attempts to remove the missing loader. Its catch block also accesses that missing loader, producing `Cannot set properties of null (setting 'innerHTML')`. Reproduced before and after the CSS-only fix; docs rendering and the verified interactions work. Kept outside this sidebar task's scope.
- CNAME HTTP 404 is expected hosting behavior, not a candidate failure. GitHub Pages is configured to build from `main` at `/` using the legacy Pages flow. No PR workflow is tracked; the existing generated `pages build and deployment` run on the target base passed. The release verification section of [PR #1](https://github.com/Akwasi-Adu/EvolutionAPI/pull/1) is the maintained publication record for the merge SHA, Pages run, and final live verification; consult it before assuming the feature-branch push published the site.

## Remaining work and authorization

- Implementation, local verification, and feature-branch publication are complete. Pushed `codex/docs-sidebar-scroll` through `c925c99` before the final approval update. Local and remote branch heads matched and the checkout was clean.
- Publication: [PR #1](https://github.com/Akwasi-Adu/EvolutionAPI/pull/1), targeting `main`. Refreshed `origin/main` still matches the verified base; the complete outgoing diff contains only `docs.html` and this ledger. GitHub reported the PR mergeable with a clean merge state and no configured PR checks. No checks were bypassed.
- Remaining implementation work: None. Publication follow-through (merge, Pages build, and live browser verification) and final completion evidence are maintained in the PR description's release verification section. Use the PR's current merge/deployment evidence to resolve release state when resuming.
- Authorized: Discard prior unpublished edits in this repository; implement and locally verify sidebar scrolling on the live baseline; commit and push the feature branch (user's "proceed" on 2026-11-03); open and merge the PR to publish (user's explicit "Yes—open and merge the PR to publish" on 2026-11-03).
- Outside scope: Changes to other repositories, force-pushes, and repository protection changes. No additional implementation or unrelated loader cleanup is authorized by this publication approval.
