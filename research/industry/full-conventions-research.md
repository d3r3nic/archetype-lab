

# Comprehensive Frontend Coding Conventions, Standards & Best Practices

Below is the most exhaustive catalog I can compile, drawn from the full breadth of modern frontend engineering knowledge through mid-2025. I have organized it using your categories and added several you did not list.

---

## 1. PROJECT SETUP CONVENTIONS

### Folder / Directory Structure

- **Feature-based structure** — Group files by feature/domain rather than by file type (`/features/auth/`, `/features/dashboard/`).
- **Colocation principle** — Place tests, styles, types, and stories next to the component they belong to.
- **Flat-over-nested rule** — Avoid directory nesting deeper than 3–4 levels; flatten when possible.
- **`src/` root convention** — All source code lives under a single `src/` directory, keeping config files at the project root.
- **`public/` or `static/` convention** — Unprocessed static assets go in a dedicated top-level folder.
- **Index barrel files** — Use `index.ts` re-exports per feature folder for cleaner imports (but beware tree-shaking pitfalls).
- **Path aliasing** — Configure `@/` or `~/` aliases (via tsconfig paths or bundler config) to avoid deep relative imports.
- **Shared/common directory** — Cross-feature utilities, types, and components live in `/shared/` or `/common/`.
- **Config directory** — Centralize all tool configs (ESLint, Prettier, Jest, etc.) in a `/config/` or root-level dotfiles.
- **Scripts directory** — Custom build/dev/deploy scripts live in `/scripts/`.
- **Generated code directory** — Auto-generated files (API clients, GraphQL types) go in `/generated/` and are gitignored or committed separately.
- **Layout vs. page vs. component separation** — Distinguish between layout shells, routable pages, and reusable components.

### Monorepo vs. Polyrepo

- **Monorepo workspace convention** — Use npm/yarn/pnpm workspaces or Nx/Turborepo to manage multiple packages in one repo.
- **Package boundary convention** — Each workspace package has its own `package.json`, `tsconfig.json`, and entry point.
- **Internal package naming** — Prefix internal packages with `@org/` scope.
- **Shared config packages** — Extract ESLint, TypeScript, and Prettier configs into shared workspace packages.
- **Independent versioning vs. fixed versioning** — Choose whether packages version independently or share a single version.
- **Dependency hoisting convention** — Understand and configure hoisting rules to avoid phantom dependencies.
- **Polyrepo contract testing** — When using separate repos, maintain contract tests or shared schema packages published to a registry.
- **Changesets convention** — Use Changesets or similar tooling for versioning and changelog generation in monorepos.

### Package Management

- **Lockfile commitment** — Always commit `package-lock.json`, `yarn.lock`, or `pnpm-lock.yaml`.
- **Exact versions convention** — Pin exact dependency versions (or use lockfiles strictly) for reproducible builds.
- **Engine locking** — Specify `engines` field in `package.json` and use `.nvmrc` / `.node-version` for Node version.
- **Corepack convention** — Use Corepack to enforce a specific package manager and version.
- **Peer dependency convention** — Libraries declare peer deps; apps install them directly.
- **Dependency classification** — Correctly separate `dependencies`, `devDependencies`, and `peerDependencies`.
- **No-install audit** — Run `npm audit` / `pnpm audit` in CI before every deploy.
- **Renovate / Dependabot convention** — Automate dependency update PRs with grouping and scheduling rules.
- **License compliance** — Scan dependencies for license compatibility; block copyleft in proprietary projects.

### Environment Configuration

- **`.env` file hierarchy** — Use `.env`, `.env.local`, `.env.development`, `.env.production` with clear override precedence.
- **Prefix convention** — Only expose variables with a specific prefix to the client (e.g., `NEXT_PUBLIC_`, `VITE_`).
- **`.env.example` convention** — Commit a `.env.example` with all required keys (no values) as documentation.
- **Runtime vs. build-time env vars** — Distinguish variables baked in at build time from those injected at runtime.
- **No secrets in client bundles** — Never put API secrets, database credentials, or private keys in frontend env vars.
- **Type-safe env vars** — Validate env vars at startup with Zod or `t3-env` to catch missing/malformed values early.
- **Env-specific feature configuration** — Use env vars to toggle features per environment rather than code branches.

### Git Conventions

- **Conventional Commits** — Use `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`, `test:`, `perf:`, `ci:`, `style:`, `build:` prefixes.
- **Scoped commits** — Add scope in parentheses: `feat(auth): add login form`.
- **Atomic commits** — Each commit represents one logical change that passes all checks.
- **Trunk-based development** — Merge short-lived feature branches into `main`; avoid long-lived branches.
- **Git Flow alternative** — For release-heavy projects: `main`, `develop`, `feature/*`, `release/*`, `hotfix/*`.
- **Branch naming convention** — `feature/TICKET-123-short-description`, `fix/TICKET-456-bug-name`.
- **PR size convention** — Keep pull requests under 400 lines of diff; split larger changes.
- **Squash merge convention** — Squash feature branch commits into a single commit on merge.
- **Pre-commit hooks (Husky + lint-staged)** — Run linting, formatting, and type-checking on staged files before commit.
- **Commit message body convention** — First line under 72 chars; blank line; then detailed body if needed.
- **Co-author attribution** — Use `Co-Authored-By` trailer for pair/mob programming.
- **Signed commits** — Require GPG/SSH-signed commits for verified identity in sensitive projects.
- **`.gitignore` conventions** — Ignore `node_modules/`, `dist/`, `.env.local`, OS files (`.DS_Store`), IDE files.
- **`.gitattributes` convention** — Normalize line endings (`* text=auto`) and mark binary files.
- **Protected branches** — Require PR reviews, passing CI, and no force-push on `main`.
- **CODEOWNERS file** — Define code ownership for automatic PR review assignment.

---

## 2. CODE ARCHITECTURE CONVENTIONS

### Design Patterns

- **Factory pattern** — Centralize complex object/component creation behind a factory function.
- **Observer / Pub-Sub pattern** — Decouple event producers from consumers using event emitters or reactive streams.
- **Strategy pattern** — Encapsulate interchangeable algorithms behind a common interface (e.g., different validation strategies).
- **Adapter pattern** — Wrap third-party APIs to conform to your application's interface.
- **Facade pattern** — Provide a simplified interface to a complex subsystem (e.g., an API client facade).
- **Decorator pattern** — Add behavior to components/functions without modifying them (HOCs, middleware).
- **Singleton pattern (with caution)** — Use sparingly for truly global services (analytics, logging); prefer DI.
- **Command pattern** — Encapsulate actions as objects for undo/redo, queuing, or logging.
- **Mediator pattern** — Centralize communication between components through a mediator (event bus, state store).
- **Repository pattern** — Abstract data access behind a repository interface to decouple from API implementation.
- **Proxy pattern** — Intercept access to objects for caching, logging, or lazy initialization.
- **Builder pattern** — Construct complex objects step by step (useful for query builders, form builders).
- **Module pattern / Revealing module** — Encapsulate private state and expose a public API.
- **Middleware pattern** — Chain processing steps for requests, state changes, or side effects.

### Component Architecture

- **Atomic Design** — Organize into atoms, molecules, organisms, templates, and pages.
- **Feature-based architecture** — Group by domain feature, each feature owning its components, hooks, state, and tests.
- **Domain-Driven Design (DDD) in frontend** — Organize around bounded contexts with clear domain models.
- **Presentational vs. container split** — Separate pure UI components from data-fetching/logic components.
- **Smart/dumb component convention** — "Smart" components manage state/effects; "dumb" components are pure renderers.
- **Component composition over inheritance** — Build complex UIs by composing small components, never extending classes.
- **Single Responsibility Principle for components** — Each component does one thing well.
- **Headless component pattern** — Provide logic/state via hooks or render props with zero UI opinion.
- **Provider pattern** — Wrap subtrees with context providers for dependency injection.
- **Layout component pattern** — Dedicate components to spatial arrangement (Stack, Grid, Flex wrappers).

### State Management

- **Local-first state** — Default to component-local state; lift only when needed.
- **Server state vs. client state separation** — Use dedicated tools (React Query, SWR, Apollo) for server state; keep client state separate.
- **Global state minimalism** — Only globalize truly app-wide state (auth, theme, locale).
- **Normalized state shape** — Store entities in flat lookup tables (`{ byId: {}, allIds: [] }`) to avoid duplication.
- **Immutable state updates** — Never mutate state directly; use spread, `structuredClone`, or Immer.
- **Finite state machines** — Model complex UI states (loading, error, success, idle) as explicit state machines (XState).
- **Selector pattern** — Derive computed state through memoized selectors instead of storing derived values.
- **Action/reducer pattern** — Describe state changes as actions dispatched to reducers for predictability.
- **Optimistic state convention** — Update UI immediately, then reconcile when the server responds.
- **State colocation** — Keep state as close to where it's used as possible in the component tree.
- **URL as state** — Use URL params and search params as the source of truth for navigation and filter state.
- **Form state convention** — Use controlled components with a form library (React Hook Form, Formik) or uncontrolled with native form data.
- **Ephemeral vs. persisted state** — Clearly distinguish transient UI state from state that survives refresh (localStorage, sessionStorage).

### Data Flow

- **Unidirectional data flow** — Data flows down via props; events flow up via callbacks.
- **Event-driven architecture** — Use custom events or an event bus for cross-cutting communication.
- **Reactive streams** — Use RxJS or signals for complex asynchronous data transformations.
- **Signal-based reactivity** — Use fine-grained reactivity (Solid, Angular signals, Preact signals) for surgical DOM updates.
- **Props drilling threshold** — If props pass through more than 2–3 intermediary components, introduce context or state management.
- **Callback stability convention** — Memoize callbacks passed to children to prevent unnecessary re-renders.

### Module Organization

- **Barrel exports** — Re-export public API from `index.ts` per module; keep internals private.
- **Explicit public API** — Only export what consumers need; keep helper functions module-private.
- **Circular dependency prevention** — Use dependency analysis tools; refactor shared code into a lower-level module.
- **Side-effect-free modules** — Modules should not execute side effects on import.
- **Tree-shakeable exports** — Use named exports over default exports for better dead-code elimination.

### Dependency Injection

- **Context-based DI** — Use framework context (React Context, Angular DI, Vue provide/inject) to inject services.
- **Hook-based DI** — Wrap service access in custom hooks for testability and swappability.
- **Interface-first DI** — Define TypeScript interfaces for services; swap implementations in tests.
- **Service locator anti-pattern avoidance** — Prefer explicit injection over global service registries.

### API Layer Architecture

- **Dedicated API layer** — Centralize all HTTP calls in an `/api/` or `/services/` directory.
- **Client instance pattern** — Create a configured Axios/fetch instance with base URL, interceptors, and defaults.
- **Request/response interceptors** — Handle auth tokens, error normalization, and logging in interceptors.
- **API function naming** — Name API functions by domain action: `getUser()`, `createOrder()`, not `fetchData()`.
- **Type-safe API clients** — Generate or manually type request params and response shapes for every endpoint.
- **API abstraction layer** — Wrap HTTP details so business logic never imports `axios` or `fetch` directly.

### Separation of Concerns

- **Logic/view separation** — Extract business logic into hooks, utilities, or services; keep components as thin view layers.
- **No business logic in components** — Components orchestrate; they don't calculate, validate, or transform data.
- **Style separation** — Keep styling in dedicated files, CSS modules, or styled-components; avoid inline styles for complex rules.
- **No direct DOM manipulation** — Use framework APIs; avoid `document.querySelector` in component code.

---

## 3. LANGUAGE-LEVEL CONVENTIONS

### TypeScript / JavaScript Naming

- **PascalCase** for types, interfaces, enums, classes, and React components.
- **camelCase** for variables, functions, methods, and hook names.
- **SCREAMING_SNAKE_CASE** for true constants and env vars.
- **kebab-case** for file names and CSS class names.
- **`I`-prefix avoidance** — Do not prefix interfaces with `I` (prefer `User` over `IUser`); this is a legacy C# convention.
- **`T`-prefix for generic type params** — Use `T`, `TResult`, `TProps` for generic type parameters.
- **Boolean naming** — Prefix with `is`, `has`, `should`, `can`: `isLoading`, `hasError`.
- **Handler naming** — Prefix event handlers with `handle`; prefix callback props with `on`: `onClick` prop, `handleClick` handler.
- **Plural for collections** — `users` (array), `user` (single item), `userMap` (dictionary).
- **Descriptive over short** — `getUserById` over `getUser`; `calculateTotalPrice` over `calc`.
- **No abbreviations (mostly)** — `button` not `btn`, `property` not `prop` (except where universally understood like `props`, `params`, `args`).

### Type Safety

- **Strict mode** — Enable `"strict": true` in `tsconfig.json` (includes `strictNullChecks`, `noImplicitAny`, etc.).
- **No `any`** — Ban `any`; use `unknown` and narrow with type guards.
- **Discriminated unions** — Model variants with a shared literal discriminant field.
- **Branded/opaque types** — Use branded types to prevent accidental misuse of structurally identical primitives (e.g., `UserId` vs. `OrderId`).
- **Const assertions** — Use `as const` for literal inference on arrays and objects.
- **Exhaustive checks** — Use `never` in switch defaults to catch unhandled cases at compile time.
- **Utility types mastery** — Use `Partial`, `Required`, `Pick`, `Omit`, `Record`, `Exclude`, `Extract`, `ReturnType`, `Parameters`.
- **Zod/Valibot runtime validation** — Validate unknown data (API responses, form input, env vars) with a schema library that infers TS types.
- **No type assertions unless necessary** — Avoid `as` casts; prefer type guards or generics.
- **Function return types** — Explicitly annotate public function return types for documentation and safety.
- **`satisfies` operator** — Use `satisfies` to check type conformance without widening.
- **Template literal types** — Use template literal types for string patterns (routes, event names).
- **Generic constraints** — Constrain generics with `extends` to ensure minimum shape.
- **Index signature caution** — Prefer `Record<string, T>` or `Map` over loose index signatures.

### Error Handling

- **Error-first pattern avoidance** — Use try/catch or Result types instead of Node-style error-first callbacks.
- **Custom error classes** — Extend `Error` with domain-specific subclasses (`NetworkError`, `ValidationError`).
- **Result/Either pattern** — Return `{ ok: true, data } | { ok: false, error }` instead of throwing in pure functions.
- **Typed error responses** — Define error shape types for API responses.
- **Never swallow errors** — Every `catch` must log, re-throw, or handle; never leave empty catch blocks.
- **Error messages for users vs. developers** — Keep user-facing messages friendly; include stack traces and context only in logs.
- **Fail fast** — Validate inputs at boundaries and throw/return early.

### Async Patterns

- **`async/await` over `.then()` chains** — Use async/await for readability; avoid mixing paradigms.
- **Concurrent execution** — Use `Promise.all()` for independent async operations; `Promise.allSettled()` when partial failures are acceptable.
- **Race condition avoidance** — Use AbortController or request IDs to cancel stale async operations.
- **Loading/error/success state triads** — Every async operation maps to idle, loading, success, or error states.
- **No floating promises** — Every promise must be awaited, returned, or explicitly voided.
- **Timeout convention** — Wrap long-running promises with a timeout utility.
- **Retry with backoff** — Implement exponential backoff for transient failures.
- **AbortController convention** — Pass `AbortSignal` to fetch calls; abort on component unmount or user cancellation.

### Immutability

- **`const` by default** — Use `const` for all declarations; use `let` only when reassignment is required; never use `var`.
- **Object/array spread for updates** — `{ ...obj, key: newValue }` and `[...arr, newItem]`.
- **`Object.freeze` for deep constants** — Freeze configuration objects that must never change.
- **Readonly types** — Use `Readonly<T>`, `ReadonlyArray<T>`, and `readonly` modifier in TypeScript.
- **Immer for complex updates** — Use Immer's `produce` for deeply nested immutable updates.
- **No mutating array methods in place** — Prefer `toSorted()`, `toReversed()`, `toSpliced()`, `with()` (ES2023+) or spread-then-mutate.

### Null / Undefined Handling

- **Prefer `undefined` over `null`** — Use `undefined` for absent values; reserve `null` for intentional emptiness from APIs.
- **Optional chaining** — Use `?.` for safe property access.
- **Nullish coalescing** — Use `??` instead of `||` to avoid falsy-value bugs (0, '', false).
- **Non-null assertion avoidance** — Avoid `!` postfix; narrow types instead.
- **Early return for null checks** — Guard against null/undefined at the top of functions.
- **Optional vs. nullable** — In types, distinguish `field?: string` (may be missing) from `field: string | null` (explicitly null).

### Import / Export Conventions

- **Named exports preferred** — Use named exports for tree-shaking and refactoring clarity.
- **One component per file** — Exception: tightly coupled small helper components.
- **Import ordering** — External deps first, then internal aliases, then relative imports, then styles; enforce with ESLint.
- **Type-only imports** — Use `import type { Foo }` to avoid runtime import of type-only modules.
- **No wildcard imports** — Avoid `import * as` in application code (acceptable for namespaced utilities).
- **Absolute imports** — Use path aliases (`@/components/Button`) instead of `../../../components/Button`.
- **Dynamic imports for code splitting** — Use `import()` for route-level and heavy-library splitting.
- **Side-effect imports** — Mark side-effect imports explicitly (`import './polyfill'`); set `sideEffects: false` in `package.json` for libraries.

---

## 4. CSS / STYLING CONVENTIONS

### CSS Methodologies

- **BEM (Block Element Modifier)** — `.block__element--modifier` naming for predictable, flat specificity.
- **OOCSS (Object-Oriented CSS)** — Separate structure from skin; compose reusable style objects.
- **SMACSS** — Categorize rules into Base, Layout, Module, State, Theme.
- **ITCSS (Inverted Triangle CSS)** — Layer styles by specificity: settings, tools, generic, elements, objects, components, utilities.
- **Atomic CSS / Utility-first** — Single-purpose utility classes (Tailwind CSS, UnoCSS).
- **CSS Modules** — Locally scoped class names via build-tool hashing to prevent collisions.
- **CSS-in-JS (runtime)** — Styled-components, Emotion: co-locate styles with components, dynamic styling via props.
- **CSS-in-JS (zero-runtime)** — Vanilla Extract, Panda CSS, StyleX: type-safe, build-time extraction, no runtime cost.
- **Scoped styles** — Framework-native scoping (Vue `<style scoped>`, Svelte `<style>`).
- **No `!important`** — Avoid `!important` except for utility overrides as a last resort.
- **Low specificity convention** — Keep selectors flat (single class); avoid nesting beyond 2 levels.
- **No ID selectors for styling** — Reserve `id` for JS hooks and anchors; use classes for styling.
- **No tag selectors for components** — Avoid styling bare `div`, `span`, `p` globally; scope to class names.

### Theming

- **CSS custom properties for theming** — Define theme tokens as `--color-primary`, `--spacing-md` on `:root` or a provider.
- **Theme object convention** — Centralize theme definition in a single tokens file or design-token JSON.
- **Semantic token naming** — Name tokens by role, not value: `--color-text-primary`, not `--color-gray-900`.
- **Multi-theme support** — Swap themes by toggling a class or `data-theme` attribute on `<html>`.
- **Theme type safety** — Type your theme object in TypeScript for autocomplete and compile-time checks.
- **Component-level theming** — Allow components to accept theme overrides via props or CSS variable scoping.

### Design Token Systems

- **Token hierarchy** — Primitive tokens (raw values) → semantic tokens (role-based aliases) → component tokens.
- **Single source of truth** — Store tokens in JSON, YAML, or a design-tool plugin; generate code for all platforms.
- **Style Dictionary / Token Studio** — Use tooling to transform and distribute tokens across platforms.
- **Token naming convention** — `{category}-{property}-{variant}-{state}`: `color-background-surface-hover`.
- **Breakpoint tokens** — Define responsive breakpoints as tokens, not magic numbers.

### Responsive Design

- **Mobile-first** — Write base styles for small screens; add complexity with `min-width` media queries.
- **Breakpoint scale** — Use a consistent set: `sm` (640px), `md` (768px), `lg` (1024px), `xl` (1280px), `2xl` (1536px) (or similar).
- **Container queries** — Use `@container` for component-intrinsic responsiveness over viewport media queries where applicable.
- **Fluid typography / spacing** — Use `clamp()` for smooth scaling between breakpoints.
- **No fixed widths** — Prefer percentage, `max-width`, `min-width`, and flex/grid for layout.
- **Responsive images** — Use `srcset`, `sizes`, and `<picture>` for resolution and art-direction switching.
- **Viewport unit caution** — Prefer `dvh`/`svh`/`lvh` over `vh` for mobile viewport issues.
- **Touch target sizing** — Minimum 44x44px interactive targets (WCAG 2.5.8).
- **Aspect ratio convention** — Use `aspect-ratio` property instead of padding-hack.

### Animation

- **Prefer `transform` and `opacity`** — Animate only compositor-friendly properties for 60fps.
- **`prefers-reduced-motion`** — Respect user motion preferences; disable or simplify animations accordingly.
- **CSS transitions for simple state changes** — Hover, focus, expand/collapse.
- **CSS `@keyframes` for complex sequences** — Multi-step animations.
- **Web Animations API** — Use WAAPI for programmatic, interruptible, and performant animations from JS.
- **Animation library convention** — Framer Motion (React), GSAP, Motion One for complex orchestration.
- **`will-change` sparingly** — Hint GPU promotion only when needed; remove after animation.
- **No layout thrashing** — Batch DOM reads and writes; use `requestAnimationFrame` for visual updates.

### Accessibility Styling

- **Visible focus indicators** — Never remove outline without providing an equivalent custom focus style.
- **`:focus-visible`** — Use `:focus-visible` to show focus rings only for keyboard users.
- **High-contrast mode support** — Test with `forced-colors` media query; ensure semantics survive.
- **Minimum contrast ratios** — 4.5:1 for normal text, 3:1 for large text (WCAG AA).
- **No color-only communication** — Pair color cues with icons, patterns, or text labels.
- **Scalable text** — Use `rem`/`em` for font sizes; never lock to `px` for body text.
- **Skip link convention** — Provide a "Skip to main content" link as the first focusable element.

### Dark Mode

- **`prefers-color-scheme` detection** — Use the media query to default to system preference.
- **Manual toggle with persistence** — Allow user override stored in `localStorage`; apply a `data-theme` attribute.
- **Semantic colors only** — Never hardcode `#fff` or `#000`; always reference semantic tokens that swap per theme.
- **Image/media adaptation** — Adjust image brightness/contrast or swap assets for dark mode.
- **Transition on theme switch** — Apply a brief CSS transition to background and color when toggling.

### CSS Custom Properties

- **Namespace convention** — Prefix with project or component name: `--app-color-primary`, `--btn-padding`.
- **Fallback values** — Always provide a fallback: `var(--color-primary, #3b82f6)`.
- **Scope narrowly** — Define component-specific vars on the component selector, not `:root`.
- **Computation** — Use `calc()` with custom properties for dynamic spacing/sizing.

### Z-Index Management

- **Z-index scale** — Define a finite scale in tokens: `base(0)`, `dropdown(100)`, `sticky(200)`, `overlay(300)`, `modal(400)`, `popover(500)`, `toast(600)`, `tooltip(700)`.
- **No magic z-index numbers** — Always reference named tokens or constants.
- **Stacking context awareness** — Understand that `transform`, `opacity < 1`, `filter`, `will-change` create new stacking contexts.
- **Isolation property** — Use `isolation: isolate` to create explicit stacking contexts.

### Spacing / Typography Scales

- **Spacing scale** — Use a consistent scale (4px base): 0, 1 (4px), 2 (8px), 3 (12px), 4 (16px), 6 (24px), 8 (32px), 12 (48px), 16 (64px).
- **Typographic scale** — Use a modular scale (e.g., 1.25 ratio): xs, sm, base, lg, xl, 2xl, 3xl, 4xl.
- **Line height convention** — Pair each font size with an appropriate line height (1.2 for headings, 1.5–1.75 for body).
- **Font stack convention** — Define system-font stacks as fallbacks; load custom fonts with `font-display: swap`.
- **Vertical rhythm** — Align spacing to a baseline grid (typically 4px or 8px).
- **`ch` unit for measure** — Constrain paragraph width with `max-width: 65ch` for optimal readability.

---

## 5. COMPONENT CONVENTIONS

### Composition Patterns

- **Composition over configuration** — Pass children/slots instead of configuring everything via props.
- **Compound component pattern** — Related components that share implicit state (`<Select>`, `<Select.Option>`).
- **Render prop pattern** — Pass a function-as-child or render prop for flexible rendering.
- **Higher-Order Component (HOC)** — Wrap components to inject behavior (legacy; prefer hooks).
- **Custom hook extraction** — Move stateful logic into `use*` hooks for reuse and testing.
- **Polymorphic `as` prop** — Let components render as different HTML elements or other components.
- **Slot pattern** — Named content insertion points (Vue slots, React children + props).
- **Wrapper/decorator pattern** — Wrap components with additional markup or behavior without modifying them.
- **Inversion of control** — Components yield control to consumers (headless UI, render delegation).

### Props Design

- **Minimal prop surface** — Expose only what consumers need; derive internally when possible.
- **Object prop grouping** — Group related props into an object when there are many: `pagination={{ page, pageSize, total }}`.
- **Discriminated union props** — Use unions to type mutually exclusive prop combinations.
- **Default prop values** — Use destructuring defaults or framework defaults rather than runtime checks.
- **Callback props return `void`** — Event callbacks should not return values that the component depends on.
- **Spread props convention** — Spread remaining props onto the root element for flexibility (`...rest`).
- **`children` as primary content** — Use `children` for primary content; use named props for auxiliary content.
- **Ref forwarding** — Always forward refs on reusable components for DOM access.
- **Strict boolean props** — Boolean props default to `false`; name positively (`isOpen`, not `isNotClosed`).
- **Avoid prop-type overload** — Don't accept `string | number | ReactNode | () => void` for one prop; split into named props.

### Component API Design

- **Consistent sizing** — Use a standard `size` prop enum: `xs`, `sm`, `md`, `lg`, `xl`.
- **Consistent variants** — Use `variant` prop: `primary`, `secondary`, `outline`, `ghost`, `destructive`.
- **Consistent state props** — Use `isDisabled`, `isLoading`, `isReadOnly`, `isRequired`, `isInvalid`.
- **Consistent `className` / `style` passthrough** — Always accept and merge `className` and `style` props.
- **Controlled + uncontrolled support** — Accept `value` + `onChange` for controlled; `defaultValue` for uncontrolled.
- **Slot-based customization** — Expose `classNames`/`slots` object for styling sub-elements (Radix, NextUI pattern).

### Controlled vs. Uncontrolled

- **Controlled components** — Component value is driven by props; parent owns the state.
- **Uncontrolled components** — Component manages its own internal state; parent reads via ref or callback.
- **Hybrid/mixed convention** — Support both modes with `value` / `defaultValue` pattern; detect which is provided.
- **Single source of truth** — Never allow both `value` and `defaultValue` to drive state simultaneously.

---

## 6. TESTING CONVENTIONS

### Unit Testing

- **Test behavior, not implementation** — Assert what the user sees/experiences, not internal state.
- **Arrange-Act-Assert (AAA)** — Structure every test into setup, action, and assertion phases.
- **One assertion per logical concept** — Cluster related assertions; avoid testing unrelated things in one test.
- **Test naming: `should [expected behavior] when [condition]`** — `should display error message when email is invalid`.
- **Pure function testing** — Unit-test utilities, transformers, and validators in isolation.
- **Component unit testing** — Render with Testing Library; assert DOM output, user interactions, and accessibility.
- **Avoid testing implementation details** — Don't test state variables, lifecycle methods, or internal function calls.
- **No `waitFor` overuse** — Prefer `findBy*` queries that wait automatically.

### Integration Testing

- **Test feature workflows** — Test multi-component flows (form submission, navigation, data fetching).
- **Mock at boundaries** — Mock HTTP (MSW), not internal modules; let real code run.
- **MSW (Mock Service Worker)** — Use MSW for realistic API mocking that works in tests and development.
- **Test user journeys** — Simulate complete user scenarios from start to finish.
- **Test component integration with state management** — Render with real store/context, not mocked providers.

### E2E Testing

- **Playwright or Cypress** — Use a modern E2E framework for browser-level testing.
- **Test critical paths** — Cover the most business-critical user journeys: auth, checkout, core workflows.
- **Page Object Model** — Encapsulate page interactions in reusable page objects.
- **Stable selectors** — Use `data-testid` attributes instead of CSS selectors or text content.
- **Retry and flakiness mitigation** — Use built-in retries; avoid hard `sleep`; wait for specific conditions.
- **Parallel execution** — Run E2E tests in parallel across browsers/shards.
- **Visual baseline convention** — Capture and compare screenshots for visual regression.
- **Environment isolation** — E2E tests run against a dedicated test environment with seeded data.

### Test File Organization

- **Colocated test files** — Place `Component.test.tsx` next to `Component.tsx`.
- **`__tests__/` directory alternative** — Group tests in a sibling `__tests__/` folder if preferred.
- **Test utilities directory** — Shared helpers, custom renders, and mocks live in `/test-utils/` or `/tests/helpers/`.
- **Fixture files** — Store test data fixtures in `__fixtures__/` or `__mocks__/` directories.

### Mocking

- **Minimal mocking** — Mock only external dependencies (API, timers, browser APIs); test real logic.
- **MSW over `jest.mock`** — Prefer MSW for API mocking over manual fetch/axios mocks.
- **Factory functions for test data** — Use `createUser()`, `createOrder()` factories (or `@faker-js/faker`) for realistic test data.
- **Mock module convention** — Place manual mocks in `__mocks__/` adjacent to the module.
- **Reset mocks between tests** — Use `afterEach` to restore mocks and prevent test pollution.

### Test Naming

- **Describe blocks by component/function** — `describe('LoginForm', () => { ... })`.
- **Nested describe for scenarios** — `describe('when user is authenticated', () => { ... })`.
- **`it` or `test` consistency** — Pick one and stick with it across the codebase.
- **Human-readable test names** — `it('renders a disabled submit button when form is invalid')`.

### Coverage

- **Sensible coverage thresholds** — Aim for 80%+ on critical paths; don't chase 100% everywhere.
- **Coverage for new code** — Require coverage on new/changed lines in CI.
- **Coverage reports in CI** — Generate and publish reports; block PRs that decrease coverage.
- **Ignore generated code** — Exclude auto-generated files from coverage metrics.
- **Branch coverage** — Track branch coverage, not just line coverage, for conditional logic.

### Snapshot Testing

- **Use sparingly** — Reserve for stable, complex output (SVG, serialized data structures).
- **Small, focused snapshots** — Prefer inline snapshots for small assertions.
- **Review snapshot changes** — Treat snapshot updates as code changes requiring review.
- **No snapshot testing for component DOM** — Prefer explicit assertions over DOM snapshots.

### Visual Regression Testing

- **Chromatic / Percy / Playwright visual comparisons** — Capture and diff screenshots on every PR.
- **Component-level visual tests** — Test components in Storybook with visual regression tooling.
- **Threshold configuration** — Set acceptable pixel-diff thresholds to reduce false positives.
- **Responsive visual tests** — Capture screenshots at multiple viewport sizes.

### Accessibility Testing

- **`jest-axe` for automated a11y checks** — Run axe-core rules against rendered components in unit tests.
- **Playwright/Cypress axe integration** — Run a11y audits in E2E tests.
- **Storybook a11y addon** — Show real-time a11y violations during component development.
- **Manual testing with screen readers** — Automated tools catch ~30-40% of issues; manual testing is essential.
- **Keyboard-only navigation testing** — Verify all flows are completable without a mouse.

---

## 7. PERFORMANCE CONVENTIONS

### Code Splitting

- **Route-based splitting** — Lazy-load each route as a separate chunk.
- **Component-based splitting** — Lazy-load heavy components (modals, charts, editors) on demand.
- **Library-level splitting** — Dynamically import large libraries only when needed (date-fns, chart.js).
- **Named chunks** — Use `webpackChunkName` or equivalent for debuggable chunk names.
- **Preloading critical chunks** — Use `<link rel="preload">` or framework-level preloading for anticipated navigation.

### Lazy Loading

- **Image lazy loading** — Use `loading="lazy"` on below-the-fold images.
- **Intersection Observer pattern** — Trigger loading when elements enter viewport.
- **Suspense boundaries** — Wrap lazy components in Suspense with meaningful fallback UI.
- **Skeleton screens** — Show content-shaped placeholders while loading, not spinners.
- **Below-the-fold deferral** — Defer non-critical content below the fold.

### Memoization

- **`React.memo` for expensive renders** — Wrap components that receive stable props but re-render from parent changes.
- **`useMemo` for expensive computations** — Memoize derived data that is costly to compute.
- **`useCallback` for stable references** — Memoize callbacks passed to memoized children or effects.
- **Don't memoize everything** — Memoization has overhead; profile before applying.
- **Memoized selectors (reselect)** — Derive store-based state through memoized selectors.
- **Compiler-driven memoization** — React Compiler (React 19+) auto-memoizes; manual memoization becomes unnecessary.

### Bundle Size

- **Bundle analysis** — Regularly run `webpack-bundle-analyzer`, `source-map-explorer`, or `vite-bundle-visualizer`.
- **Import cost awareness** — Use `import cost` IDE extensions to see per-import size.
- **Tree-shaking verification** — Verify unused exports are eliminated; use `sideEffects: false` in `package.json`.
- **Dependency size evaluation** — Prefer lightweight alternatives (date-fns over moment, zustand over redux if simpler needs).
- **No duplicate dependencies** — Deduplicate with `npm dedupe` or pnpm's strict mode.
- **Bundle budget** — Set maximum bundle sizes in CI; fail builds that exceed them.

### Image Optimization

- **Modern formats** — Serve WebP or AVIF with fallbacks.
- **Responsive images** — Use `srcset` and `sizes` for resolution-appropriate delivery.
- **Image CDN** — Use Cloudinary, imgix, or Vercel Image Optimization for on-the-fly resizing.
- **Blur placeholder** — Show a blurred low-res placeholder (LQIP) while the full image loads.
- **SVG for icons** — Use inline SVG or SVG sprite sheets; avoid icon fonts.
- **`width` and `height` attributes** — Always set dimensions to prevent layout shift (CLS).

### Font Loading

- **`font-display: swap`** — Show fallback font immediately; swap when custom font loads.
- **`font-display: optional`** — For non-critical fonts: use if cached, skip if not loaded in time.
- **Preload critical fonts** — `<link rel="preload" as="font" crossorigin>` for above-the-fold fonts.
- **Subset fonts** — Include only needed character sets (Latin, extended) via unicode-range.
- **Variable fonts** — Use a single variable font file instead of multiple weight/style files.
- **Font fallback metrics** — Use `size-adjust`, `ascent-override`, `descent-override` to minimize layout shift.

### Core Web Vitals

- **LCP (Largest Contentful Paint)** — Optimize hero image/text loading; target under 2.5s.
- **INP (Interaction to Next Paint)** — Keep main thread unblocked; break long tasks; target under 200ms.
- **CLS (Cumulative Layout Shift)** — Set explicit dimensions on media; avoid inserting content above the fold dynamically; target under 0.1.
- **TTFB optimization** — Use CDN, edge rendering, streaming SSR to minimize Time to First Byte.
- **Long task breaking** — Use `scheduler.yield()`, `requestIdleCallback`, or chunked processing to break up work over 50ms.

### Virtualization

- **Virtual scrolling** — Use `react-window`, `react-virtuoso`, `@tanstack/virtual` for long lists (1000+ items).
- **Windowing convention** — Render only visible rows plus a buffer; recycle DOM nodes.
- **Infinite scroll with virtualization** — Combine data fetching (cursor/offset pagination) with virtual scrolling.

### Caching

- **HTTP caching** — Leverage `Cache-Control`, `ETag`, and immutable asset hashing.
- **Service Worker caching** — Cache static assets and API responses offline with Workbox.
- **Application-level cache** — Use React Query/SWR `staleTime` and `gcTime` for data cache management.
- **Persistent cache** — Store expensive computations or API data in IndexedDB for cross-session caching.
- **Content hashing for cache busting** — Use `[contenthash]` in filenames so browsers cache until content changes.

---

## 8. ACCESSIBILITY CONVENTIONS

### ARIA Patterns

- **No ARIA is better than bad ARIA** — Use semantic HTML first; add ARIA only when HTML falls short.
- **ARIA roles for custom widgets** — `role="tablist"`, `role="dialog"`, `role="menu"`, etc. with full keyboard support.
- **ARIA attributes naming** — Use `aria-label`, `aria-labelledby`, `aria-describedby`, `aria-expanded`, `aria-haspopup`, `aria-live` correctly.
- **APG (ARIA Authoring Practices Guide)** — Follow WAI-ARIA APG patterns for all custom interactive widgets.
- **Live regions** — Use `aria-live="polite"` for status updates and `aria-live="assertive"` for critical alerts.
- **`aria-hidden` for decorative elements** — Hide purely decorative content from screen readers.
- **`role="presentation"` / `role="none"`** — Remove semantic meaning from wrapper elements when needed.

### Keyboard Navigation

- **All interactive elements are focusable** — Buttons, links, form controls; add `tabindex="0"` only to custom interactive elements.
- **No positive tabindex** — Never use `tabindex > 0`; use DOM order for focus sequence.
- **Tab, Shift+Tab navigation** — Ensure logical tab order throughout the page.
- **Arrow keys for widget navigation** — Use arrow keys within toolbars, tabs, menus, and grids.
- **Escape to close** — All overlays (modal, popover, dropdown) close on Escape.
- **Enter/Space to activate** — Buttons activate on both Enter and Space; links activate on Enter.
- **Focus trap in modals** — Keep focus within modal dialogs until dismissed.
- **Roving tabindex** — Within composite widgets, manage a single `tabindex="0"` that moves between children.
- **Skip navigation links** — Provide skip links to bypass repetitive navigation.

### Screen Reader Conventions

- **Meaningful alt text** — Describe the purpose/content of images; use `alt=""` for decorative images.
- **Heading hierarchy** — Use `h1`–`h6` in logical order; don't skip levels.
- **Landmark regions** — Use `<main>`, `<nav>`, `<header>`, `<footer>`, `<aside>`, `<section>` with labels.
- **Visually hidden text** — Use `.sr-only` class for screen-reader-only text (not `display: none`).
- **Announce dynamic content** — Use live regions or focus management to announce content changes.
- **Table accessibility** — Use `<th>`, `scope`, `<caption>` for data tables; avoid tables for layout.
- **Form label association** — Every input has a visible `<label>` with `htmlFor`/`for`, or `aria-label`.

### Focus Management

- **Focus on route change** — Move focus to the new page's main heading or content area after SPA navigation.
- **Return focus after modal close** — Return focus to the element that triggered the modal.
- **Focus management on dynamic content** — When content appears (toast, alert), decide: announce via live region or move focus.
- **`inert` attribute** — Use `inert` to disable interaction with background content when a modal is open.
- **Focus indicator contrast** — Focus rings must have 3:1 contrast against adjacent colors.

### Semantic HTML

- **`<button>` for actions, `<a>` for navigation** — Never use `<div>` or `<span>` as interactive elements.
- **Form semantics** — Use `<form>`, `<fieldset>`, `<legend>`, `<label>` properly.
- **List semantics** — Use `<ul>`, `<ol>`, `<li>` for lists; screen readers announce count and position.
- **`<time>` element** — Use `<time datetime="...">` for dates and durations.
- **`<details>` / `<summary>`** — Use native disclosure widgets when appropriate.
- **`<dialog>` element** — Use native `<dialog>` for modal/non-modal dialogs with built-in focus management.

### Form Accessibility

- **Label every input** — Use visible `<label>` elements or `aria-label` for all form controls.
- **Error messages linked to fields** — Use `aria-describedby` to associate error text with the input.
- **Required field indication** — Use `aria-required="true"` and a visual indicator (not color alone).
- **`aria-invalid` for error state** — Set `aria-invalid="true"` on fields with validation errors.
- **Group related controls** — Use `<fieldset>` and `<legend>` for checkbox/radio groups.
- **Autocomplete attribute** — Use `autocomplete` for common fields (name, email, address) to aid autofill.
- **Input type specificity** — Use `type="email"`, `type="tel"`, `type="url"` for appropriate mobile keyboards and validation.

### Error Announcement

- **Live region for form errors** — Announce validation errors via `aria-live="polite"` region.
- **Error summary** — Show a summary of all errors at the top of the form; link each to the offending field.
- **Inline error convention** — Display error text immediately below the field; associate with `aria-describedby`.
- **Focus first error** — After submit, focus the first field with an error.

---

## 9. SECURITY CONVENTIONS

### XSS Prevention

- **Framework auto-escaping** — Rely on React/Vue/Angular's default HTML escaping; never bypass without review.
- **No `dangerouslySetInnerHTML`** — Avoid rendering raw HTML; if unavoidable, sanitize with DOMPurify.
- **No `eval()`, `new Function()`, `innerHTML`** — Never execute dynamic strings as code.
- **Sanitize user input before rendering** — Run untrusted HTML through DOMPurify or similar.
- **Strict template rendering** — Never construct HTML strings from user input.

### CSRF Protection

- **SameSite cookies** — Set `SameSite=Strict` or `SameSite=Lax` on auth cookies.
- **CSRF tokens** — Include anti-CSRF tokens in state-mutating requests when using cookie-based auth.
- **Origin header validation** — Server should validate `Origin` and `Referer` headers.

### Content Security Policy

- **CSP headers** — Implement `Content-Security-Policy` to restrict script sources, style sources, and frame ancestors.
- **Nonce-based CSP** — Use per-request nonces for inline scripts instead of `unsafe-inline`.
- **Report-only mode** — Deploy CSP in report-only mode first to detect violations without breaking functionality.
- **`Strict-Transport-Security`** — Enforce HTTPS with HSTS headers.

### Input Sanitization

- **Validate on both client and server** — Client validation is for UX; server validation is for security.
- **Schema validation** — Validate all user input against a schema (Zod, Yup) before processing.
- **URL validation** — Check URL schemes (`javascript:` attacks); whitelist `http:` and `https:`.
- **File upload validation** — Check MIME types, file size limits, and extensions on the frontend; re-validate server-side.

### Auth Patterns (Frontend)

- **Token storage** — Store JWTs in `httpOnly` cookies (preferred) or in-memory; avoid `localStorage` for sensitive tokens.
- **Token refresh** — Implement silent token refresh before expiry; queue requests during refresh.
- **Auth state management** — Maintain auth state centrally; redirect to login on 401.
- **Logout cleanup** — Clear all tokens, cached data, and sensitive state on logout.
- **Route protection** — Guard authenticated routes with middleware/wrappers that redirect unauthenticated users.
- **OAuth/OIDC PKCE flow** — Use authorization code flow with PKCE for SPAs; never implicit flow.

### Secure Storage

- **No sensitive data in `localStorage`** — `localStorage` is synchronous and accessible to any script on the domain.
- **`httpOnly` cookies for tokens** — Prevents JavaScript access to auth tokens.
- **`Secure` flag on cookies** — Cookies only sent over HTTPS.
- **Encrypt sensitive client-side data** — If you must store sensitive data locally, encrypt it.
- **Clear on session end** — Wipe session-specific data from storage on logout/session expiry.

### CORS Handling

- **Specific origin allowlists** — Never use `Access-Control-Allow-Origin: *` with credentials.
- **Preflight awareness** — Understand that non-simple requests trigger OPTIONS preflight.
- **Credentials mode** — Set `credentials: 'include'` in fetch only when needed.

### Dependency Security

- **Automated vulnerability scanning** — Run `npm audit`, Snyk, or Socket.dev in CI.
- **Lock file integrity** — Verify lockfile integrity in CI to detect supply-chain attacks.
- **Minimal dependencies** — Evaluate necessity before adding any package; prefer native APIs.
- **Subresource Integrity (SRI)** — Use `integrity` attributes on CDN-loaded scripts.
- **No running postinstall scripts blindly** — Audit lifecycle scripts in new dependencies.

---

## 10. API INTEGRATION CONVENTIONS

### REST Conventions

- **Resource-based URLs** — `/users`, `/users/:id`, `/users/:id/orders`.
- **HTTP method semantics** — GET (read), POST (create), PUT (replace), PATCH (partial update), DELETE (remove).
- **Standard status code handling** — 200 (ok), 201 (created), 204 (no content), 400 (bad request), 401 (unauthorized), 403 (forbidden), 404 (not found), 409 (conflict), 422 (unprocessable), 429 (rate limited), 500 (server error).
- **Consistent response envelope** — `{ data, meta, errors }` or flat resource per team convention.
- **Query parameter conventions** — `?page=2&limit=20&sort=name&order=asc&filter[status]=active`.

### GraphQL Conventions

- **Collocated queries** — Define queries/mutations next to the component that uses them.
- **Fragment composition** — Components define their own data requirements as fragments; parent queries compose them.
- **Operation naming** — Name every operation: `query GetUserProfile`, `mutation CreateOrder`.
- **Generated types** — Use GraphQL Code Generator for type-safe operations.
- **No over-fetching** — Request only the fields needed for each view.
- **Persisted queries** — Use automatic persisted queries (APQ) for production to reduce payload and block arbitrary queries.

### Data Fetching Patterns

- **Server state library** — Use TanStack Query, SWR, Apollo Client, or URQL for caching, deduplication, and revalidation.
- **Stale-while-revalidate** — Show cached data immediately; revalidate in background.
- **Prefetching** — Preload data on hover/focus or route preload for perceived instant navigation.
- **Deduplication** — Deduplicate identical in-flight requests automatically.
- **Placeholder data** — Show previously cached or partial data while fresh data loads.
- **Suspense for data fetching** — Use React Suspense boundaries to handle loading states declaratively.
- **Server Components (RSC) data fetching** — Fetch data in server components; pass serialized data to client components.

### Error Handling from API

- **Typed error responses** — Define an error schema: `{ code, message, details }`.
- **Error mapping** — Transform API errors into user-friendly messages via a mapping layer.
- **Retry on 5xx** — Automatically retry server errors with exponential backoff.
- **Don't retry 4xx** — Client errors (except 429) should not be retried automatically.
- **Global error interceptor** — Handle 401 (redirect to login) and 500 (show error boundary) globally.
- **Network error detection** — Detect offline state and show appropriate UI.

### Request/Response Transformation

- **Camel-case transformation** — Convert `snake_case` API responses to `camelCase` at the API layer boundary.
- **Date parsing** — Convert ISO string dates to Date objects at the API layer.
- **Normalization** — Flatten nested API responses into normalized state.
- **Request serialization** — Convert camelCase to snake_case for outgoing requests if the API expects it.

### Caching Strategies

- **Cache keys** — Use consistent, unique cache keys based on endpoint + params.
- **Cache invalidation** — Invalidate related caches after mutations (e.g., invalidate user list after creating a user).
- **Garbage collection** — Configure cache TTL and garbage collection to prevent memory bloat.
- **Offline cache** — Persist critical data to IndexedDB for offline-first capabilities.

### Optimistic Updates

- **Immediate UI update** — Update the UI before the server confirms; roll back on failure.
- **Rollback mechanism** — Snapshot previous state; restore on error.
- **Conflict resolution** — Handle cases where the server response differs from the optimistic prediction.
- **User notification on rollback** — Inform the user if an optimistic action failed.

### Pagination Patterns

- **Offset-based** — `?page=2&limit=20` for simple use cases.
- **Cursor-based** — `?cursor=abc123&limit=20` for real-time data or large datasets.
- **Infinite scroll** — Load next page when the user scrolls near the bottom.
- **Load-more button** — Explicit user action to load additional data.
- **Total count handling** — Display total results when available; handle unknown totals gracefully.

### Real-time Data

- **WebSocket conventions** — Establish a single connection per client; multiplex channels.
- **Server-Sent Events (SSE)** — Use for server-to-client unidirectional streams (notifications, live updates).
- **Reconnection strategy** — Implement automatic reconnection with exponential backoff.
- **Heartbeat/ping-pong** — Keep connections alive and detect disconnection.
- **State synchronization** — Merge real-time updates with cached state without duplication.
- **Event sourcing on client** — Replay events to reconstruct state after reconnection.

### API Versioning (Frontend Handling)

- **Version in URL path** — `/api/v1/users`, `/api/v2/users`.
- **Version in header** — `Accept: application/vnd.api+json;version=2`.
- **Feature detection over version checking** — Check for capability, not version number, when possible.
- **Graceful degradation** — Handle missing fields from older API versions with defaults.

---

## 11. INTERNATIONALIZATION CONVENTIONS

### i18n Patterns

- **Framework library** — Use `react-intl`, `next-intl`, `i18next`, `vue-i18n`, or equivalent.
- **Key-based translation** — Reference translations by key (`t('auth.login.title')`) not by source string.
- **Namespace/scope separation** — Separate translations by feature: `auth.json`, `dashboard.json`.
- **ICU message format** — Use ICU syntax for plurals, selects, and interpolation.
- **Lazy-load locale data** — Only load the active locale's translations; don't bundle all locales.
- **Default language fallback** — Fall back to base language when a translation key is missing.
- **Extraction tooling** — Use automatic key extraction from source code to keep translation files in sync.
- **Context for translators** — Provide context/description for ambiguous keys.

### RTL Support

- **Logical properties** — Use `margin-inline-start`, `padding-inline-end`, `inset-inline` instead of left/right.
- **`dir="rtl"` attribute** — Set direction on `<html>` or per-component for bidirectional content.
- **Mirror icons and layouts** — Flip directional icons (arrows, progress bars) for RTL.
- **Tailwind RTL plugin** — Use `rtl:` and `ltr:` variants if using Tailwind.
- **Test RTL rendering** — Visually verify layouts in RTL mode during development.

### Date/Time/Number Formatting

- **`Intl` API** — Use `Intl.DateTimeFormat`, `Intl.NumberFormat`, `Intl.RelativeTimeFormat`.
- **Timezone handling** — Store and transmit in UTC; display in user's local timezone.
- **Locale-aware formatting** — Format dates, numbers, and currencies based on user locale, not hardcoded patterns.
- **Relative time** — Display "2 hours ago" using `Intl.RelativeTimeFormat` for recent timestamps.
- **Calendar system awareness** — Support non-Gregorian calendars when required by the audience.

### Translation File Organization

- **JSON or YAML per locale** — `/locales/en/common.json`, `/locales/fr/common.json`.
- **Flat keys vs. nested keys** — Choose a convention and be consistent: `"auth.login.title"` vs. `{ auth: { login: { title } } }`.
- **Shared + feature-scoped files** — Common strings in `common.json`; feature-specific strings in `feature-name.json`.
- **Pluralization rules** — Handle plural forms correctly per locale (some languages have 6+ plural forms).

### Locale Handling

- **Locale detection** — Detect from browser (`navigator.language`), URL, cookie, or user profile in order of priority.
- **Locale in URL** — Use subpath (`/en/about`) or subdomain (`en.example.com`) for SEO.
- **Locale persistence** — Store user's locale preference in a cookie or user profile.
- **Locale switching without reload** — Support dynamic locale switching in SPAs.
- **`lang` attribute** — Set `<html lang="en">` for screen readers and search engines.

---

## 12. ERROR HANDLING CONVENTIONS

### Error Boundaries

- **Top-level error boundary** — Wrap the entire app to catch unhandled rendering errors.
- **Route-level error boundaries** — Each route has its own error boundary to isolate failures.
- **Feature-level error boundaries** — Wrap independent features so one failure doesn't crash the whole page.
- **Custom fallback UI** — Show contextual error UI (not just "Something went wrong") with retry action.
- **Error boundary reset** — Provide a mechanism to reset error boundary state (retry button, navigation).

### Global Error Handling

- **`window.onerror`** — Catch uncaught runtime errors globally.
- **`window.onunhandledrejection`** — Catch unhandled promise rejections.
- **Error reporting service** — Send errors to Sentry, Datadog, LogRocket, or Bugsnag.
- **Source maps in error reporting** — Upload source maps to error tracking service for readable stack traces.
- **Error deduplication** — Group identical errors to avoid alert fatigue.
- **User context in errors** — Attach user ID, session ID, and route to error reports.

### User-Facing Error Messages

- **Friendly language** — "We couldn't load your data. Please try again." not "Error: ECONNREFUSED".
- **Actionable messages** — Tell the user what they can do: retry, contact support, check input.
- **No technical details in UI** — Log technical details to console/service; show human text in UI.
- **Categorized error messages** — Differentiate network errors, validation errors, auth errors, and server errors.
- **Error codes for support** — Display an error reference code the user can quote to support.

### Retry Patterns

- **Automatic retry for transient errors** — Retry network errors and 5xx with exponential backoff.
- **Manual retry affordance** — Show a retry button for failed data fetches.
- **Max retry limit** — Cap retries (typically 3) before showing a final error state.
- **Idempotency awareness** — Only auto-retry idempotent operations (GET); confirm retry for mutations.
- **Circuit breaker pattern** — Stop retrying after repeated failures; show degraded UI.

### Fallback UI

- **Skeleton fallback** — Show content placeholders while loading.
- **Partial failure** — Render available data and show inline error for failed sections.
- **Offline fallback** — Show cached content or an offline page when the network is unavailable.
- **Empty state** — Distinguish "no data" from "error loading data" with different UI.
- **404 page** — Custom 404 with navigation back to known routes.
- **Maintenance page** — Display a maintenance page when the backend is deploying.

---

## 13. DOCUMENTATION CONVENTIONS

### Code Comments

- **Comment "why", not "what"** — Code shows what; comments explain the non-obvious reason.
- **TODO/FIXME/HACK tags** — Use `// TODO(author):`, `// FIXME:`, `// HACK:` with context.
- **JSDoc for public APIs** — Document exported functions, types, and components with JSDoc.
- **Deprecation comments** — Use `@deprecated` JSDoc tag with migration instructions.
- **No commented-out code** — Delete dead code; version control preserves history.
- **Inline documentation for regex** — Always comment non-trivial regular expressions.
- **Link to external context** — Reference ticket numbers, RFCs, or external docs when relevant.
- **Parameter documentation** — Document non-obvious parameters, especially callbacks and option objects.

### Component Documentation

- **Storybook stories** — Write stories for every UI component showing all variants and states.
- **Args/controls** — Use Storybook controls for interactive prop exploration.
- **Storybook play functions** — Add interaction tests within stories.
- **Story organization** — Organize stories mirroring component hierarchy: `Components/Button`, `Features/Auth/LoginForm`.
- **Documentation pages (MDX)** — Write usage guides in Storybook MDX docs.
- **Auto-generated prop tables** — Use Storybook's auto-doc to generate prop tables from TypeScript types.

### README Conventions

- **Project README** — Include: project description, prerequisites, setup instructions, development commands, project structure overview, and deployment notes.
- **Quick start** — The fastest path from clone to running app should be clear in 30 seconds.
- **Contributing guide** — `CONTRIBUTING.md` with PR process, code style, and testing requirements.
- **Feature-specific READMEs** — Optional `README.md` per feature directory for complex domains.

### Changelog

- **Keep a Changelog format** — Sections: Added, Changed, Deprecated, Removed, Fixed, Security.
- **Automated changelog** — Generate from Conventional Commits with `standard-version` or Changesets.
- **Semantic versioning** — Tie changelog entries to semver releases.

### Architecture Decision Records (ADRs)

- **ADR format** — Title, Status (proposed/accepted/deprecated/superseded), Context, Decision, Consequences.
- **ADR directory** — Store in `/docs/adr/` or `/docs/decisions/`.
- **Number ADRs sequentially** — `001-use-react-query-for-data-fetching.md`.
- **Record significant decisions** — Framework choices, state management approach, API strategy, testing strategy.
- **Link superseding ADRs** — When a decision is revised, link the old and new ADRs.

---

## 14. BUILD & DEPLOYMENT CONVENTIONS

### Build Configuration

- **Bundler convention** — Use Vite, Next.js, or Turbopack as of 2025/2026; Webpack for legacy projects.
- **TypeScript compilation** — Use `tsc` for type-checking; let the bundler handle transpilation.
- **Source maps** — Generate source maps for development and error tracking; do not expose to users in production.
- **Asset hashing** — Use content hashing in filenames for cache-busting.
- **Target configuration** — Set browserslist to define supported browser targets.
- **Minification** — Minify JS (Terser/SWC) and CSS in production builds.
- **Compression** — Serve Brotli and gzip pre-compressed assets from CDN.

### Environment-Specific Builds

- **No environment logic in application code** — Use env vars or build-time replacement, not `if (process.env.NODE_ENV === 'production')` scattered throughout.
- **Dead code elimination** — Ensure dev-only code (debug logs, mock data) is stripped in production.
- **Separate build targets** — Dev (fast, HMR, source maps), staging (production-like, with debug tools), production (optimized, no debug).

### CI/CD Conventions

- **Pipeline stages** — Install → Lint → Type-check → Unit test → Build → Integration test → E2E test → Deploy.
- **Parallel jobs** — Run lint, type-check, and unit tests in parallel.
- **Cache dependencies** — Cache `node_modules` and build artifacts across CI runs.
- **Preview deployments** — Deploy every PR to a unique preview URL (Vercel, Netlify, Cloudflare Pages).
- **Automated checks as merge gates** — Require all CI checks to pass before merging.
- **Build artifact promotion** — Promote the same artifact from staging to production; don't rebuild.
- **Canary/gradual rollout** — Deploy to a small percentage of traffic first.

### Feature Flags

- **Feature flag service** — Use LaunchDarkly, Unleash, Statsig, or a custom solution.
- **Boolean and multivariate flags** — Support simple on/off and multi-variant experiments.
- **Default-off for new features** — New features ship behind flags turned off in production.
- **Flag cleanup** — Remove flag checks and dead code after a feature is fully rolled out.
- **Server-evaluated flags** — Evaluate flags server-side to prevent flickering and avoid exposing logic to clients.
- **Type-safe flags** — Type flag names and expected value types for compile-time safety.

### Deployment Strategies

- **Static site hosting** — Deploy SPAs and static sites to CDN (Vercel, Netlify, Cloudflare Pages, S3+CloudFront).
- **SSR hosting** — Deploy server-rendered apps to serverless functions or container platforms.
- **Edge rendering** — Deploy to edge runtime (Cloudflare Workers, Vercel Edge) for low-latency responses.
- **Blue/green deployment** — Maintain two identical environments; switch traffic after verification.
- **Rollback capability** — Ensure any deployment can be instantly rolled back to the previous version.
- **Immutable deploys** — Each deploy creates a new immutable version; old versions remain accessible.

### Asset Pipeline

- **Static asset CDN** — Serve all static assets from a CDN with appropriate cache headers.
- **Asset fingerprinting** — All built assets have content hashes in filenames.
- **Image pipeline** — Optimize, resize, and convert images at build time or via image CDN.
- **SVG pipeline** — Inline SVGs or use SVG sprites; optimize with SVGO.
- **Web font subsetting** — Subset fonts at build time to include only needed glyphs.

---

## 15. CODE QUALITY CONVENTIONS

### Linting

- **ESLint with strict config** — Extend `eslint:recommended`, `plugin:@typescript-eslint/strict`, framework-specific configs.
- **Import order linting** — Enforce consistent import ordering with `eslint-plugin-import` or `eslint-plugin-simple-import-sort`.
- **Accessibility linting** — `eslint-plugin-jsx-a11y` (React) or equivalent for catching a11y issues at author time.
- **No unused variables/imports** — Error on unused variables, imports, and function parameters.
- **No console in production** — Warn or error on `console.log`; allow `console.error` and `console.warn`.
- **Consistent function style** — Enforce arrow functions or function declarations consistently per project.
- **Flat config** — Use ESLint's flat config format (`eslint.config.js`) as of ESLint 9+.
- **Custom rules** — Write project-specific ESLint rules for domain conventions.
- **`eslint-plugin-react-hooks`** — Enforce Rules of Hooks and exhaustive deps.

### Formatting

- **Prettier for formatting** — Let Prettier handle all formatting; don't configure formatting rules in ESLint.
- **Format on save** — Configure editors to format on save.
- **Consistent config** — Share `.prettierrc` across the team: print width, tab width, semicolons, quotes, trailing commas.
- **Format in CI** — Check formatting in CI; fail on unformatted code.
- **`.editorconfig`** — Include `.editorconfig` for cross-editor basics (indent style, charset, newlines).

### Code Review Conventions

- **PR template** — Use a PR template with sections: Summary, Changes, Testing, Screenshots.
- **Review checklist** — Check for: functionality, edge cases, accessibility, performance, security, tests, naming.
- **Two approvals** — Require at least two reviewer approvals for critical code paths.
- **No self-merge** — PRs must be merged by someone other than the author (for non-trivial changes).
- **Review response time** — Agree on SLAs for review turnaround (e.g., 24 hours).
- **Nit convention** — Prefix non-blocking suggestions with "nit:" to distinguish from required changes.
- **Suggest changes** — Use GitHub's "suggested changes" feature for small fixes.
- **Review automation** — Use Danger.js or similar to automate repetitive review checks.

### Technical Debt

- **Tech debt tagging** — Use `// TECH-DEBT:` comments with ticket references.
- **Debt register** — Maintain a backlog of known tech debt items with severity and effort estimates.
- **Dedicated debt reduction sprints** — Allocate 10-20% of sprint capacity to tech debt.
- **Boy Scout Rule** — Leave code cleaner than you found it; fix small issues as you go.

### Dependency Updates

- **Automated update PRs** — Renovate or Dependabot with grouping (e.g., all test deps together).
- **Update schedule** — Minor/patch weekly; major monthly with dedicated testing.
- **Changelogs before merge** — Read changelogs and migration guides before merging major updates.
- **Pin critical dependencies** — Pin major versions of framework and build tool dependencies.

### Dead Code Elimination

- **Tree shaking** — Ensure bundler removes unused exports.
- **Knip / ts-prune** — Use tooling to detect unused exports, files, and dependencies.
- **Delete, don't comment** — Remove dead code; Git history preserves it if needed.
- **Feature flag cleanup** — Remove flag-gated old code paths after rollout.

---

## 16. FRONTEND-BACKEND COUPLING CONVENTIONS

### API Contract

- **OpenAPI/Swagger specification** — Define REST APIs with OpenAPI spec as the single source of truth.
- **Generated clients** — Generate TypeScript API clients from OpenAPI spec (openapi-typescript, orval, swagger-codegen).
- **GraphQL schema-first** — Define GraphQL schema first; generate resolvers and client types.
- **Contract testing** — Use Pact or similar for consumer-driven contract tests.
- **Breaking change detection** — Run schema diff in CI to detect breaking API changes.

### Shared Types/Schemas

- **Shared schema package** — Publish Zod schemas or TypeScript types as a shared npm package.
- **Type generation from backend** — Generate frontend types from backend schema (Prisma, tRPC, GraphQL codegen).
- **tRPC convention** — Use tRPC for end-to-end type safety between TypeScript backends and frontends.
- **JSON Schema as contract** — Use JSON Schema for validation on both sides when languages differ.

### Auth Token Handling

- **Access + refresh token pattern** — Short-lived access token; long-lived refresh token.
- **Token refresh interception** — Intercept 401 responses; refresh token; retry original request.
- **Concurrent refresh handling** — Queue requests during refresh; replay after token renewal.
- **Token rotation** — Implement refresh token rotation to detect token reuse attacks.

### File Upload

- **Chunked upload** — Split large files into chunks with resumability.
- **Presigned URL pattern** — Get a presigned URL from the backend; upload directly to storage (S3).
- **Progress tracking** — Show upload progress via `XMLHttpRequest.upload` or `fetch` with streams.
- **Client-side validation** — Validate file type, size, and dimensions before uploading.

### Real-time Communication

- **WebSocket with fallback** — Try WebSocket; fall back to SSE or long-polling.
- **Socket.IO convention** — Use Socket.IO for automatic reconnection, namespaces, and rooms.
- **Event naming convention** — Use `domain:action` format: `chat:message`, `order:updated`.
- **Connection state management** — Track and display connection status (connected, reconnecting, offline).

### BFF (Backend for Frontend)

- **BFF pattern** — Dedicated backend layer per frontend that aggregates and transforms API calls.
- **Next.js/Remix server layer** — Use framework server functions as an implicit BFF.
- **API route aggregation** — Combine multiple backend calls into a single BFF endpoint to reduce round trips.
- **Frontend-specific DTOs** — BFF returns exactly the shape the frontend needs; no over-fetching.

---

## 17. DESIGN SYSTEM CONVENTIONS (ADDITIONAL CATEGORY)

- **Single source of truth** — One component library consumed by all applications.
- **Versioned releases** — Semantic versioning with changelogs for design system packages.
- **Token-first design** — All visual decisions encoded in tokens before component implementation.
- **Composition API** — Primitives composable into complex patterns; avoid monolithic components.
- **Headless + styled split** — Optionally separate headless logic from styled implementations.
- **Storybook as documentation** — Storybook is the canonical component catalog and playground.
- **Figma-to-code sync** — Align component APIs with Figma component properties for design-dev handoff.
- **Contribution guidelines** — Define how teams propose, build, and release new components.
- **Deprecation policy** — Announce, mark `@deprecated`, provide migration path, remove after N versions.
- **Automated visual testing** — Every PR to the design system runs visual regression tests.
- **Cross-framework support** — Consider Web Components or framework adapters for multi-framework orgs.

---

## 18. MONITORABILITY & OBSERVABILITY CONVENTIONS (ADDITIONAL CATEGORY)

- **Real User Monitoring (RUM)** — Track Core Web Vitals, page load times, and interaction metrics from real users.
- **Synthetic monitoring** — Run automated checks from multiple locations to detect outages.
- **Error tracking** — Sentry, Datadog RUM, or LogRocket for automatic error capture with context.
- **Session replay** — Record and replay user sessions for debugging (with privacy masking).
- **Custom event tracking** — Track business events (signup, purchase, feature usage) with analytics.
- **Performance budgets** — Alert when bundle size, LCP, or INP exceeds thresholds.
- **Structured logging** — Use structured log formats (JSON) for searchability in log aggregation tools.
- **Feature flag telemetry** — Track which flag variants users receive and their impact on metrics.
- **Alerting conventions** — Define alert thresholds, escalation paths, and runbooks for frontend incidents.
- **Dashboard conventions** — Maintain dashboards for error rates, performance metrics, and deployment health.

---

## 19. DEVELOPER EXPERIENCE (DX) CONVENTIONS (ADDITIONAL CATEGORY)

- **Fast feedback loops** — HMR in under 200ms; tests run in under 10s; CI in under 10 minutes.
- **Reproducible environments** — Use Docker, devcontainers, or Nix for consistent dev environments.
- **One-command setup** — `npm install && npm run dev` (or a single `make dev`) gets a new contributor running.
- **Local HTTPS** — Use `mkcert` for local TLS to match production behavior (cookies, service workers).
- **IDE configuration sharing** — Commit `.vscode/settings.json` and `.vscode/extensions.json` (with opt-in scope).
- **Debug configuration** — Provide launch configurations for debugging in IDE and browser.
- **Generator/scaffolding tools** — Use `plop`, `hygen`, or Nx generators for consistent new-component boilerplate.
- **Task runner convention** — Standardize scripts in `package.json`: `dev`, `build`, `test`, `lint`, `format`, `typecheck`.

---

## 20. MIGRATION & EVOLUTION CONVENTIONS (ADDITIONAL CATEGORY)

- **Strangler fig pattern** — Gradually replace legacy code by wrapping/routing to new implementations.
- **Codemods** — Use `jscodeshift` or `ts-morph` codemods for large-scale automated refactors.
- **Feature parity checklist** — Maintain a checklist comparing old and new implementations during migration.
- **Parallel running** — Run old and new systems simultaneously with traffic splitting.
- **Compatibility layer** — Build adapter layers so old and new code can coexist.
- **Incremental adoption** — New conventions apply to new code; retrofit old code on a defined schedule.
- **Migration ADRs** — Document migration strategy, phases, and rollback plan as an ADR.

---

## 21. MICRO-FRONTEND CONVENTIONS (ADDITIONAL CATEGORY)

- **Module Federation** — Use Webpack/Vite Module Federation for runtime micro-frontend composition.
- **Shared dependency management** — Deduplicate shared libraries (React, design system) across micro-frontends.
- **Independent deployment** — Each micro-frontend deploys independently with its own CI/CD pipeline.
- **Cross-MFE communication** — Use custom events or a shared event bus; avoid direct imports.
- **Consistent styling** — Share design tokens and base styles; scope component styles.
- **Routing convention** — Each micro-frontend owns a URL prefix; a shell app handles top-level routing.
- **Version compatibility** — Maintain backward compatibility in shared contracts between micro-frontends.
- **Error isolation** — One micro-frontend crashing should not take down others.

---

## 22. PROGRESSIVE WEB APP (PWA) CONVENTIONS (ADDITIONAL CATEGORY)

- **Web App Manifest** — Provide `manifest.json` with name, icons, theme color, display mode.
- **Service Worker lifecycle** — Register, install, activate, and update SW with clear cache versioning.
- **Offline strategy** — Cache-first for static assets; network-first for API data; stale-while-revalidate for semi-dynamic content.
- **Background sync** — Queue failed mutations and replay when online.
- **Push notifications** — Request permission contextually (not on first load); handle subscription management.
- **App install prompt** — Intercept `beforeinstallprompt` and show at an appropriate moment.
- **Update notification** — Prompt users to refresh when a new service worker is waiting.

---

## 23. AI-FIRST / AI-OPTIMIZED CODE CONVENTIONS (ADDITIONAL CATEGORY)

- **Descriptive naming over brevity** — AI agents parse identifiers; `calculateMonthlyRevenue` is better than `calcRev`.
- **Explicit over implicit** — Avoid magic strings, implicit dependencies, and convention-based wiring that AI cannot infer.
- **Rich type annotations** — Exhaustive TypeScript types give AI maximum context about code contracts.
- **JSDoc with examples** — Doc comments with `@example` blocks help AI understand intended usage.
- **Colocation of context** — Keep related logic, types, and tests together so AI can load minimal files to understand a feature.
- **Avoid deeply nested logic** — Flatten conditionals (early returns, guard clauses); AI handles linear flow better.
- **Standard patterns over clever code** — Prefer well-known patterns; AI recognizes idiomatic code more reliably.
- **Small, focused files** — Files under 300 lines are easier for AI to process in context windows.
- **Meaningful commit messages** — Conventional commits with clear descriptions help AI understand change history.
- **`CLAUDE.md` / `AGENTS.md` / `.cursorrules`** — Include project-level instruction files that guide AI agents on conventions, architecture, and constraints.
- **`ARCHITECTURE.md`** — A plain-English description of the system's architecture, module boundaries, and key decisions for AI consumption.
- **Structured TODO comments** — `// TODO(scope): description [ticket]` gives AI enough context to assist with or complete the task.
- **No abbreviations in domain terms** — Spell out domain concepts fully; AI may not know project-specific abbreviations.
- **Consistent file structure** — Every component follows the same file structure so AI can predict where to find things.
- **Single responsibility files** — One export per file where possible; AI can load exactly what it needs.
- **Tests as specification** — Well-named tests describe the system's behavior, serving as executable documentation for AI.
- **Avoid side-effect-on-import** — AI may analyze files without executing them; side effects on import create hidden behavior.
- **Dependency graph clarity** — Avoid circular dependencies; clear hierarchical imports help AI reason about the system.

---

## 24. FORM HANDLING CONVENTIONS (ADDITIONAL CATEGORY)

- **Form library usage** — Use React Hook Form, Formik, or framework-equivalent for complex forms.
- **Schema-based validation** — Define validation with Zod/Yup; derive both types and validation rules from one schema.
- **Field-level validation** — Validate each field on blur/change; show errors inline.
- **Form-level validation** — Validate the entire form on submit; show a summary of all errors.
- **Multi-step form pattern** — Break long forms into wizard steps with progress indication and per-step validation.
- **Dirty/pristine tracking** — Warn users about unsaved changes when navigating away.
- **Optimistic form submission** — Disable the submit button and show progress; re-enable on error.
- **Server-side validation display** — Map server validation errors back to specific form fields.
- **Default values convention** — Always provide explicit default values for all form fields.
- **Debounced validation** — Debounce expensive validation (e.g., uniqueness checks) to avoid excessive API calls.
- **Accessible error messages** — Associate errors with fields via `aria-describedby`; announce with live region.

---

## 25. ROUTING CONVENTIONS (ADDITIONAL CATEGORY)

- **File-based routing** — Use framework conventions (Next.js, Remix, Nuxt) for automatic route generation from file structure.
- **Nested routes/layouts** — Use layout routes for persistent UI (sidebar, header) across child routes.
- **Dynamic route segments** — Use `[id]` or `:id` for parameterized routes.
- **Catch-all routes** — Use `[...slug]` for flexible, deeply nested paths.
- **Route guards/middleware** — Protect routes with authentication/authorization checks before rendering.
- **Loading UI per route** — Each route segment can define its own loading state.
- **Error UI per route** — Each route segment can define its own error boundary.
- **Parallel routes** — Render multiple independent route segments simultaneously.
- **Intercepting routes** — Show modals/overlays while preserving the underlying URL structure.
- **Route prefetching** — Prefetch route data and code on link hover/viewport entry.
- **URL state management** — Encode filters, pagination, and view state in URL search params.
- **Typed routes** — Use type-safe route definitions (tanstack/router, next-safe-navigation) to prevent broken links.

---

## 26. SERVER-SIDE RENDERING & HYDRATION CONVENTIONS (ADDITIONAL CATEGORY)

- **SSR by default for content** — Server-render content-heavy pages for SEO and performance.
- **SPA for app-like experiences** — Use CSR for highly interactive, authenticated application views.
- **Streaming SSR** — Stream HTML progressively with Suspense boundaries for faster TTFB.
- **Selective hydration** — Hydrate interactive components on demand; leave static content as HTML.
- **React Server Components** — Fetch data and render on the server; send serialized UI to the client with zero JS.
- **Hydration mismatch prevention** — Ensure server and client render identical initial HTML; use `useId` for stable IDs.
- **`suppressHydrationWarning`** — Use only for known mismatches (timestamps, randomized content).
- **Progressive enhancement** — Core functionality works without JavaScript; JS enhances the experience.
- **`use client` / `use server` directives** — Explicitly mark client/server boundaries in RSC applications.
- **Streaming with `Suspense`** — Define streaming boundaries so content appears progressively as it resolves.
- **Static generation (SSG)** — Pre-render pages at build time for maximum performance where content doesn't change frequently.
- **ISR (Incremental Static Regeneration)** — Regenerate static pages on a schedule or on-demand without full rebuild.

---

## 27. ANIMATION & MOTION DESIGN CONVENTIONS (ADDITIONAL CATEGORY)

- **Motion design tokens** — Define duration, easing, and delay scales as tokens: `duration-fast (100ms)`, `duration-normal (200ms)`, `ease-out`.
- **Entrance/exit animations** — Animate elements in and out (mount/unmount) with consistent patterns.
- **Shared layout animations** — Animate elements smoothly between layout positions (Framer Motion `layoutId`).
- **Gesture-driven animations** — Respond to drag, swipe, and pinch with physics-based animations.
- **Orchestrated sequences** — Stagger child animations for lists and grids.
- **Scroll-driven animations** — Use `animation-timeline: scroll()` or Intersection Observer for scroll-triggered effects.
- **Spring physics** — Use spring-based easing for natural-feeling motion.
- **Exit before enter** — When transitioning between views, exit old content before entering new.
- **Consistent easing** — Use a project-wide easing curve convention (e.g., `cubic-bezier(0.4, 0, 0.2, 1)` for standard easing).

---

## 28. DATA VISUALIZATION CONVENTIONS (ADDITIONAL CATEGORY)

- **Chart library convention** — Use D3, Recharts, Visx, Chart.js, or ECharts consistently.
- **Responsive charts** — Charts resize fluidly with their container; use `ResizeObserver`.
- **Accessible charts** — Provide text alternatives, keyboard navigation, and ARIA labels for data visualizations.
- **Color palette for data** — Use a colorblind-safe palette with sufficient contrast between data series.
- **Consistent axis formatting** — Apply locale-aware number and date formatting to axes.
- **Tooltip conventions** — Show contextual data on hover/focus; dismiss on Escape.
- **Loading/empty/error states** — Charts show skeleton, "no data", or error states appropriately.
- **Animation in charts** — Animate data transitions smoothly; respect `prefers-reduced-motion`.

---

## 29. WEB COMPONENT / CROSS-FRAMEWORK CONVENTIONS (ADDITIONAL CATEGORY)

- **Web Components for shared primitives** — Use Custom Elements when components must work across frameworks.
- **Shadow DOM scoping** — Use Shadow DOM for style encapsulation; `part` and `::part` for external styling.
- **Custom event convention** — Dispatch `CustomEvent` with `detail` payload for component communication.
- **Attribute vs. property convention** — Use attributes for primitive values; properties for complex data.
- **Declarative Shadow DOM** — Use `<template shadowrootmode="open">` for SSR-compatible shadow roots.
- **Lit or Stencil** — Use a library for ergonomic Web Component authoring.

---

## 30. STATE MACHINE & WORKFLOW CONVENTIONS (ADDITIONAL CATEGORY)

- **XState for complex flows** — Model multi-step processes (checkout, onboarding, wizards) as state machines.
- **Statechart visualization** — Use XState Viz or Stately to visualize and share state machines.
- **Guard conditions** — Use guards (predicates) for conditional transitions.
- **Actions for side effects** — Trigger API calls, analytics, and navigation as machine actions.
- **Hierarchical states** — Use nested states for substates within a process step.
- **Parallel states** — Model concurrent concerns (form validity + network status) as parallel state regions.
- **Invoking services** — Use `invoke` for async operations (API calls, timers) within machines.
- **Testing state machines** — Test machines by asserting state transitions given sequences of events.

---

## 31. MOBILE / RESPONSIVE-FIRST CONVENTIONS (ADDITIONAL CATEGORY)

- **Touch-first interaction design** — Design for touch targets, swipe gestures, and fat-finger tolerance.
- **Viewport meta tag** — Always include `<meta name="viewport" content="width=device-width, initial-scale=1">`.
- **Native-like patterns** — Use pull-to-refresh, bottom sheets, and swipe-to-dismiss for mobile-web apps.
- **Performance on low-end devices** — Test on throttled CPU/network; optimize JS execution and memory.
- **Adaptive loading** — Serve lighter experiences on slow connections (`navigator.connection`).
- **Input mode optimization** — Use `inputmode="numeric"`, `inputmode="email"` for appropriate soft keyboards.
- **Safe area insets** — Use `env(safe-area-inset-*)` for notched/rounded device screens.
- **Haptic feedback** — Use Vibration API sparingly for confirming actions on mobile.

---

## 32. DEPENDENCY & THIRD-PARTY LIBRARY CONVENTIONS (ADDITIONAL CATEGORY)

- **Evaluate before installing** — Check bundle size, maintenance activity, open issues, and alternatives before adding a dependency.
- **Wrapper pattern** — Wrap third-party libraries behind your own interface so they can be swapped.
- **Version range strategy** — Use `^` for minor-safe deps; pin exact for critical deps; always use lockfile.
- **Bundle impact check** — Run `bundlephobia` or equivalent before adding; set size limits in CI.
- **Dual-package hazard** — Be aware of ESM/CJS interop issues; verify packages export the format your bundler expects.
- **Peer dep compliance** — Ensure installed versions satisfy all peer dependency requirements.
- **Polyfill strategy** — Use `core-js` or `browserslist`-based polyfilling for target browser support.

---

## 33. SEO CONVENTIONS (ADDITIONAL CATEGORY)

- **Semantic HTML for SEO** — Use proper heading hierarchy, `<article>`, `<nav>`, `<main>`, `<section>`.
- **Meta tags** — Set `<title>`, `<meta name="description">`, and Open Graph / Twitter Card tags per page.
- **Canonical URLs** — Set `<link rel="canonical">` to prevent duplicate content.
- **Structured data (JSON-LD)** — Add Schema.org markup for rich search results.
- **Dynamic meta tags in SPAs** — Use framework-level head management (Next.js `metadata`, `react-helmet`).
- **Sitemap generation** — Auto-generate `sitemap.xml` at build time or via server route.
- **Robots.txt** — Configure crawl directives and link to sitemap.
- **Prerendering for SPA SEO** — Use SSR/SSG or a prerender service for crawlable SPA content.
- **Image alt text for SEO** — Provide descriptive, keyword-relevant alt text on meaningful images.

---

## 34. ENVIRONMENT & PLATFORM DETECTION CONVENTIONS (ADDITIONAL CATEGORY)

- **Feature detection over UA sniffing** — Use `'serviceWorker' in navigator` not `navigator.userAgent` checks.
- **Progressive enhancement** — Build for the lowest common denominator; enhance where features exist.
- **Graceful degradation** — If a feature is unavailable, degrade to a functional alternative.
- **`@supports` CSS queries** — Use `@supports` to conditionally apply modern CSS.
- **SSR/CSR-safe checks** — Guard browser API access with `typeof window !== 'undefined'` in universal code.

---

## 35. LEGAL & COMPLIANCE CONVENTIONS (ADDITIONAL CATEGORY)

- **Cookie consent** — Implement a consent banner compliant with GDPR/ePrivacy before setting non-essential cookies.
- **Privacy-first analytics** — Use privacy-respecting analytics (Plausible, Fathom) or anonymize GA data.
- **Data minimization** — Collect only necessary user data on the frontend.
- **Accessibility compliance** — Target WCAG 2.2 Level AA as the minimum standard.
- **License headers** — Include license headers in source files for open-source projects.
- **Third-party script audit** — Audit all third-party scripts for data collection and compliance impact.

---

This catalog contains approximately **500+ distinct conventions** across 35 categories. Each is a named, actionable standard that can be expanded into detailed implementation guidance. The categories you originally specified are fully covered in items 1-16, with items 17-35 representing additional categories that a comprehensive frontend framework should address.