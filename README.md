<div align="center">

<h1>Skill: Universal UI Migration</h1>

<p>Reproduce an interface while preserving the destination's architecture.</p>

<p><strong>Agent independent</strong> &nbsp;·&nbsp; <strong>Stack independent</strong> &nbsp;·&nbsp; <strong>Frontend first</strong></p>

</div>

Migrate an existing UI into another codebase with its components, responsive
layouts, dummy data, routes, validation, hooks, and frontend interactions.

> **Source:** design and frontend behavior.
>
> **Destination:** implementation and integration conventions.

[Quick start](#quick-start) · [Coverage](#what-migrates) · [API modes](#api-and-backend-modes) · [Examples](#usage-examples) · [Agents](#use-with-your-agent) · [Verification](#workflow-and-verification) · [Files](#package-files)

---

## Quick start

**1. Make the files available.** Give your agent access to the source codebase,
the destination codebase, and this complete skill folder.

**2. Replace the placeholders.** `<skill-folder>` means the directory containing
this `SKILL.md`. `<source-project>` and `<destination-project>` mean your
codebase locations. Use paths appropriate to your workspace and operating system.

**3. Send this prompt.**

```text
Read <skill-folder>/SKILL.md and its referenced analysis-and-verification guide.
Follow them for this migration.

Source codebase: <source-project>
Destination codebase: <destination-project>
Scope: the entire source frontend, including all components and dummy data.

Analyze both frontend configurations before making changes.
Match the source design, responsive behavior, routes, hooks, and validation.
Preserve the destination architecture and existing integrations.
Do not migrate API or backend services.
Verify visual fidelity and frontend behavior, and report any gaps.
```

### Defaults at a glance

| Setting | Default |
| :--- | :--- |
| **Scope** | The whole source frontend unless you select specific pages, features, routes, or components |
| **Source files** | Read-only; use a temporary copy if running the source would change its files |
| **Destination** | Preserve its architecture, configuration, existing integrations, unrelated features, and user changes |
| **Source API/backend** | Excluded unless affirmatively requested with `migrate api` |
| **Missing backend capability** | Working local behavior in an isolated fixture/demo mode |
| **Completion** | Verified visual and frontend behavior parity, with destination regression checks |

## What migrates

Everything within the selected scope is inventoried and mapped into the
**destination's existing conventions**.

| Area | Included |
| :--- | :--- |
| **Design** | Layout, text, fonts and weights, colors, spacing, borders, shadows, icons, images, charts, and tables |
| **Responsive UI** | Breakpoints, container behavior, fluid layouts, mobile navigation, visibility, scrolling, and touch interactions |
| **Components and assets** | Shared/custom components, nested dependencies, lazy and conditional UI, fonts, and media |
| **Themes and motion** | Light/dark selection and persistence, animations, transitions, hover/focus/active/disabled states, and reduced motion |
| **Dummy data** | All fixture records and builders, IDs, relationships, order, labels, dates, asset references, totals, and chart series |
| **Routes** | Paths, navigation, dynamic parameters, query/hash behavior, nested layouts, redirects, errors, not-found states, and history |
| **Hooks and state** | UI hooks or native equivalents, state transitions, effects/cleanup, derived values, debounce, and local persistence |
| **Validation** | Client rules, conditional/cross-field checks, defaults/coercion, exact error messages, timing, and submit behavior |
| **Interactions** | Forms, search/filter/sort/pagination, dialogs, menus, tabs, selection, local mutations, and previews |
| **Accessibility** | Semantic controls, labels, keyboard behavior, focus management, announcements, and destination accessibility requirements |

A full migration also includes reusable components, component examples, and
fixture modules that are not reachable from current routes. A selective migration
includes its shared and transitive frontend dependencies; it does not automatically
expand to every feature linked from a page.

The skill assumes no particular framework, router, styling system, state library,
form library, or package manager. Implementations are translated to the
existing destination stack. Native UIs use their own navigation, lifecycle,
styling, and emulator/preview tools.

**Destination integrity remains a requirement.** Existing authorization,
required validation, service contracts, tooling, and unrelated features must
survive the migration. If source behavior conflicts with these requirements,
the agent surfaces the specific conflict for a scoped decision.

## API and backend modes

### Default: frontend-only

| Integration concern | Expected behavior |
| :--- | :--- |
| **Existing destination services** | Reuse compatible integrations through their configured clients, hooks, auth, and contracts |
| **Source API/backend services** | Do not copy, recreate, or activate them |
| **Source dummy data** | Migrate all of it for isolated demos and equivalent-data visual comparison |
| **Missing destination capability** | Preserve local frontend behavior in an isolated fixture adapter; report the live integration gap |

Excluded source integrations include API calls, database clients/connections,
backend actions, auth SDKs, realtime, payments, remote uploads, analytics, and
other backend or remote service wiring. Pure local calculations remain included,
even when stored in a file called a service. Images, fonts, and static fixtures
remain UI assets.

Destination endpoints, clients, payloads, credentials handling, query keys,
caching, retries, invalidation, guards, and service error semantics are preserved.
Presentation adapters can translate existing destination data for the source UI.
Live data may differ from source dummy content; visual comparison uses matching
fixtures, and connected behavior is verified separately.

> **Fixture behavior is local behavior.** It must not replace live services,
> bypass authorization, silently become a production fallback, or claim a real
> login, payment, email delivery, or server write.

Prefer pending integration instead of simulation? Add this instruction:

```text
For backend capabilities absent from the destination, keep integration pending.
Do not simulate them. Complete independent frontend work and report the missing
capabilities and any incomplete or unverified actions.
```

### Optional: migrate API

To enable source API migration, **affirmatively request `migrate api`** and
specify which integrations belong in scope. See the [API migration example](#migrate-ui-and-selected-api-operations).

- The phrase is case-insensitive and accepts ordinary whitespace variation.
- Documentation examples, quotations, questions, and negations such as
  "do not migrate api" do not authorize migration.
- "Migrate everything" or "make all hooks work" does not enable API migration.
- A later revocation restores the frontend-only boundary for remaining work.

Requested integrations use the **destination's configured client, service layer,
authentication, environment validation, data/query patterns, and error conventions**.
The agent needs real API contracts; it must not invent endpoint behavior.

The keyword does not automatically authorize backend/database implementation,
framework or auth replacement, copying secrets, production data changes,
deployment, or external transactions. Backend/database implementation is included
only when your specified scope explicitly covers it.

## Usage examples

### Migrate selected pages

```text
Read <skill-folder>/SKILL.md and its referenced verification guide.
Follow them for this migration.

Source codebase: <source-project>
Destination codebase: <destination-project>
Source view: /workspaces, including all components, hooks, validation, and dummy data.
Destination route: /app/workspaces

Preserve exact design, responsive behavior, and frontend interactions.
Use destination conventions and compatible existing integrations.
Do not migrate API or backend services.
Verify the migrated page and check unrelated destination pages for regressions.
```

### Migrate a component

```text
Read <skill-folder>/SKILL.md and its referenced verification guide.

Source codebase: <source-project>
Destination codebase: <destination-project>
Source component: <source-component-file>
Destination feature or location: <destination-feature-location>

Migrate the component's design, interactions, nested components, styles,
hooks, validation, assets, and dummy data using destination conventions.
Keep API and backend services excluded.
Add routes only if this selected scope needs them.
```

### Migrate UI and selected API operations

```text
Read <skill-folder>/SKILL.md and its referenced verification guide.

Source codebase: <source-project>
Destination codebase: <destination-project>
Scope: orders pages, including components, routes, validation, hooks, and dummy data.

migrate api for the orders read/create/update operations used by these pages.
Use the destination's configured API client, service layer, authentication,
environment validation, data/query patterns, and error conventions.
API contract: <api-contract-location>
Verify with the destination's existing safe test environment.
```

### Useful inputs

| Input | What to provide |
| :--- | :--- |
| **Required** | Source and destination codebase locations |
| **Scope** | Whole frontend, or named pages/features/routes/components; omitted scope means the whole frontend |
| **Route mapping** | Source routes and destination routes when paths should change |
| **Visual references** | Screenshots, supported devices/viewports, themes, and representative UI states |
| **Test context** | Protected-route test access, locale/timezone, destination constraints, and known baseline failures |
| **Missing backend behavior** | Isolated fixture mode, or pending integration; existing services are preserved either way |
| **API mode only** | An affirmative `migrate api` request, its scope, API contracts, and a safe test environment |

The agent discovers configuration from the codebases. It asks about concrete
conflicts or missing essentials and proceeds with independent work. Analysis is
required before edits; it is not a blanket approval stop for authorized work.

## Use with your agent

The quick-start prompt works with any agent that can read files, edit the
destination, run its checks, and inspect rendered output.

| Agent | Invocation |
| :--- | :--- |
| **Codex** | Use the file-reading prompt. If registered/discoverable in your setup, invoke `$universal-ui-migration` with source, destination, and scope. |
| **Claude Code** | Use the same prompt from a workspace with access to both codebases. Native skill registration is optional. |
| **Antigravity** | Provide the skill and codebase paths through workspace/file context. A rule or workflow can load `SKILL.md` before implementation. |
| **Other agents** | Load or attach `SKILL.md` and its referenced guide, then supply source, destination, and scope. Use equivalent tools. |

Native installation and discovery depend on your agent setup. Direct file loading
requires no agent-specific registration or slash-command syntax. If file access
is restricted, expose the repositories through permitted workspace roots or
copies, or supply their files as context. Skill instructions do not grant
filesystem, network, or execution permissions.

## Workflow and verification

| Stage | What the agent does |
| :--- | :--- |
| **1. Analyze** | Compare both frontend configurations, inventory every in-scope dependency, map routes, record destination contracts, and establish baseline checks |
| **2. Adapt** | Translate assets, styling, components, routes, hooks, validation, and data adapters using destination conventions |
| **3. Verify** | Compare rendered UI, exercise frontend behavior, run destination checks, and check shared consumers and unrelated features |
| **4. Report** | Deliver changed files, route/coverage mappings, adaptations, evidence, fixture/connected behavior, and remaining gaps |

### Acceptance checks

- [ ] Destination build, typecheck, lint, relevant tests, and required checks run.
- [ ] Source and destination are compared under matching fixtures, state, theme,
  and narrow/tablet/wide viewports, including actual breakpoint boundaries.
- [ ] Migrated controls, validation, routes/deep links, UI states, and
  accessibility behave as expected.
- [ ] Unrelated views, shared components/styles, guards, and integration contracts
  pass regression checks.
- [ ] No unintended source backend traffic is introduced into the destination.
- [ ] The report identifies evidence, pre-existing failures, and every blocked or
  unverified item.

**A passing build does not establish visual or behavioral parity.** Without
rendered preview access, visual verification remains unverified. Fixture parity
and live integration verification are reported separately; real backend outcomes
require compatible destination services or explicitly scoped API migration.

## Package files

```text
universal-ui-migration/
├── SKILL.md
├── README.md
├── references/
│   └── analysis-and-verification.md
└── agents/
    └── openai.yaml
```

| File | Purpose |
| :--- | :--- |
| [SKILL.md](SKILL.md) | Portable instructions for the migration agent |
| [Analysis and verification guide](references/analysis-and-verification.md) | Discovery checklist, coverage ledger, and acceptance evidence |
| [Codex metadata](agents/openai.yaml) | Optional Codex display and invocation metadata; other agents can ignore it |

Keep the folder together so relative references resolve. No companion skill,
particular agent, or agent-specific metadata is required to follow the Markdown
instructions.
