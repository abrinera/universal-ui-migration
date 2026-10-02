# Analysis and verification reference

Read the discovery sections before editing and the acceptance sections while
verifying. Maintain one inventory/coverage ledger across the migration. Store
reports in the destination's existing documentation location if a durable report
is needed; otherwise keep the ledger in working notes and summarize it at handoff.
Do not impose a new application directory structure to store migration paperwork.

## Frontend configuration discovery

Inspect both roots and the actual frontend workspace when a root is a monorepo.
Read applicable project instructions and existing implementations. Use `rg` or
equivalent file/text search; exclude dependencies, build output, caches, generated
clients, and secret values from inventories.

| Area | Establish from files and observed behavior |
| --- | --- |
| Project boundary | Workspace packages, frontend root, shared packages, app entry, writable/source boundaries, starting changes |
| Dependencies | Manifest and lockfile, package manager/version policy, installed framework/runtime versions, peers, scripts |
| Build/runtime | Bundler/framework config, plugins, SSR/SSG/client boundaries, hydration, base/public path, lazy loading |
| Language/imports | TypeScript/JavaScript settings, module mode, aliases, code generation, lint/format conventions |
| Routing | File/code/server routing, route manifests and generation, parameter/query schemas, guards, loaders, redirects, errors |
| Styling | CSS strategy/version, tokens, resets/preflight, layers/order, breakpoints, container queries, themes, fonts, animation |
| Components | Primitive locations, registries if any, local modifications, variants, provider/context dependencies, portals |
| State/hooks | Local/global state, UI hooks or equivalents, query conventions, persistence, effects/cleanup, browser-only APIs |
| Forms/validation | Form library, UI schemas versus DTO/server schemas, transformations, messages, timing, submission mapping |
| Data/integrations | Existing client/service/auth patterns, query keys, environment key names, external SDK boundaries, fixture/demo modes |
| Assets/content | Public/imported assets, media/fonts/icons, asset URLs, localization, fixtures/builders, seeds, chart datasets |
| Quality checks | Actual build/type/lint/test commands, browser tooling, visual baselines, safe test environment |

Record differences as adaptations, not upgrade instructions. Examples:

- Different router: translate route/navigation contracts using the installed
  destination router; retain guard and query validation behavior.
- Different utility CSS version: translate token/class generation and output;
  account for changed reset, defaults, and animation setup.
- Different framework: translate lifecycle, state, composition, event semantics,
  and accessibility into its native patterns; do not install the source framework.
- Different runtime: keep browser work out of server execution, preserve
  serializable boundaries, and load fonts/assets using destination conventions.
- Different primitive: evaluate rendered geometry, interactions, focus, and
  theming before choosing reuse, scoped customization, or a local implementation.

## Coverage ledger

Trace transitive frontend dependencies from each view. Include lazy/conditional
components, inherited styles, hidden dialogs, menus, responsive branches, hooks,
validation, route metadata, fixtures, and asset references. Every source item
in scope needs a destination mapping; do not sample the inventory.
For whole-frontend scope include component catalogs/stories, unreferenced reusable
components and fixture modules, not just artifacts reachable from current routes.

Use these columns, adapting the format to the project:

| Source item and location | Source route/state | Destination location/route | Adaptation and data mode | Preservation dependency | Implementation status | Verification evidence |
| --- | --- | --- | --- | --- | --- | --- |
| Inventory each real item | Include query/params/theme/role when relevant | Include reused destination equivalents | Native rewrite, wrapper, copy; connected or isolated fixture | Guard, service contract, shared style, DTO, etc. | Pending during work; final status below | Check result, exercised flow, screenshot or explicit gap |

For each frontend action record its trigger, preconditions, validation, state
transition, visible result, persistence, and backend dependency. For dummy data
record all records/builders and dependent totals/relationships. For routes record
source/destination paths, dynamic/query/hash semantics, redirects, layout and
guards. A backend exclusion is not permission to exclude its associated UI.

Final statuses:

- `verified`: implemented with relevant observed acceptance evidence.
- `implemented-unverified`: implemented, but a required check could not run.
- `blocked`: missing input, conflict, dependency, or an observed failure remains.
- `excluded-by-scope`: outside the user's selected scope or intentionally excluded
  by the API boundary; include the reason. Do not exclude required frontend work
  merely because implementation is difficult.

## Destination preservation map

Record the contracts that must survive and where they are implemented:

- API clients, endpoints, interceptors, auth headers/cookies, and error mapping.
- Data hooks, query keys, cache policy, retries, invalidation and mutation payloads.
- Authentication/session providers, protected route/role guards, redirects.
- Environment validation and key names; never record credentials or secret values.
- Required schemas, DTOs, backend/server rules, middleware and remote SDK setup.
- Existing route behaviors, unrelated views, shared primitives/styles, providers.
- Package/runtime/tooling versions, aliases, generation and workspace boundaries.
- User changes and baseline failures present before migration.

Default-mode service code should remain unchanged. A shared file may contain UI
registration and a protected contract: review its relevant diff and behavior,
rather than treating its whole filename as either forbidden or replaceable.
Necessary frontend config/route edits must preserve the recorded contracts.
In API mode explicitly list the requested integration contracts allowed to change.

## Visual comparison protocol

1. Start both interfaces using their own supported commands and browser or native
   preview/emulator tools. Use a temporary source
   copy if running it would otherwise change tracked application/config files.
2. Select identical fixtures, view, params/query/hash, form values, theme, locale,
   timezone, clock/random seed, role/permissions, viewport and device scale.
   Exercise protected views through valid test facilities, preserving guards.
3. Wait for fonts, assets and data state to settle. For static screenshots capture
   the same animation point or consistently pause animations on both sides;
   separately verify motion duration/easing, transitions and reduced motion.
4. Capture both outputs and compare side by side, overlay, or pixel diff. Check
   whole-page geometry and component details. Investigate mismatches in fonts,
   wrapping, spacing, defaults, CSS order, images, icons, shadows and stacking.
5. Verify narrow/tablet/wide layouts and both sides of each relevant actual
   breakpoint/container threshold. When viewport sizes are unspecified, suitable
   initial probes are 360, 390, 768, 1024 and 1440 CSS pixels; adjust for source
   behavior. Check continuous resizing, overflow, long content and touch controls.
6. Repeat for every in-scope view and distinct relevant states, including overlays,
   validation, empty/loading/error/success and theme/responsive variants. Shared
   component evidence may be reused only when render context adds no new behavior.

Keep screenshot paths and tested viewport/state in the evidence. Do not claim
pixel equality from matching classes/tokens, a successful build, or a single
desktop screenshot. Explain unavoidable rasterization differences if present;
do not classify actual design deviations as rendering noise. No universal
percentage threshold establishes parity across all designs.

If rendered preview access or source rendering is unavailable, continue static adaptation
and available checks, but mark visual evidence unverified and identify what is
needed. Images supplied by the user can verify their visible states only.

## Behavioral and regression acceptance

| Check | Observable evidence |
| --- | --- |
| Routes | Link navigation, direct URL/refresh, dynamic/query/hash handling, back/forward, redirects, not-found, nested layouts, guards |
| Forms | Valid/invalid/edge submissions, normalization, conditional/cross-field rules, exact messages/timing, error focus, correct destination payload |
| Controls | Search/filter/sort/page, selections, tabs, menus, dialogs, steppers and all other inventoried actions produce source-equivalent transitions |
| Hooks/state | Defaults, derived values, debounce/timers, cleanup/unmount, persistence and rerender behavior; no duplicated stores/providers |
| Local data | Exact fixture content and relationships, deterministic totals/charts, create/edit/delete/undo where present; connected data remains separate |
| Media/files | Assets load correctly, crop/size match, previews and local import/export work; remote upload is handled by the API boundary |
| Themes/motion | Correct selection/persistence, light/dark values, actual animation behavior, responsive and reduced-motion states |
| Accessibility | Keyboard paths, names/labels, focus trap/return, semantic controls, validation/status announcements |
| Build/runtime | Destination build/type/lint and relevant required tests, console/hydration errors, missing assets, responsive overflow |
| Destination regressions | Unrelated pages and shared consumers still render/work, guards effective, providers/config intact, baseline user changes preserved |
| API boundary | No copied source transport/SDK side effect through any dependency; unchanged destination contracts; isolated fixture behavior makes no remote success claim |
| API migration, if enabled | Requested contracts mapped/tested using destination infrastructure; existing integration behavior outside scope preserved |

Do not trigger external writes just to verify a UI. Use isolated fixtures, mocks,
existing test services, or an authorized safe environment. Network inspection
must distinguish destination integrations, static asset requests, and prohibited
source backend traffic. Check more than `fetch`: imports, providers, loaders,
server actions and SDK initialization can initiate remote work indirectly.

Run the destination's actual scripts with its own package manager; do not assume
`npm run build` or fabricate commands. Add focused tests when adaptations introduce
meaningful risk, especially route/query translation, validation, state transitions
and adapter mappings. New broad test infrastructure is not a prerequisite.

## Handoff evidence

Summarize these fields in a size appropriate to the migration:

```text
Scope: source/destination roots, whole frontend or selected features
Modes: frontend-only or explicitly requested API scope; fixture/connected by feature
Configuration: detected stacks/versions and necessary adaptations
Routes and coverage: source-to-destination map and ledger status counts
Changes: meaningful files, dependencies, configuration and UI behavior
Integrity: preserved service/auth/validation/config contracts and regression evidence
Visual evidence: screenshot locations, viewports, states and comparison results
Behavior/checks: commands/results, exercised flows, pre-existing failures
Gaps: each blocked/unverified item, reason and concrete next input/check
```

State separately whether frontend parity was verified in fixture mode and whether
connected backend behavior was verified. The source backend remaining excluded
is expected in frontend-only mode; a missing required frontend state is not.
