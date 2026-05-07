# Frontend Standards

These standards apply to all frontend work across Condado engagements. We work across two stacks depending on the engagement: MVC with Bootstrap 5, and React with Next.js, TypeScript, and Tailwind CSS. Standards in sections 1–5 apply to both. Sections 6–7 are stack-specific.

When citing a rule in a pull request or code review, use the section code in brackets (e.g., `[FE-A11Y-02]`). Codes are stable.

---

## 1. Project Structure [FE-PROJ]

### 1.1 General Principles [FE-PROJ-01]

Group files by **feature, not by file type**. A feature folder contains its views or components, styles, tests, and models. Shared utilities and design tokens live in a top-level `shared/` or `common/` folder, never buried inside a feature.

Every frontend project has a `README.md` at root describing how to install, run the dev server, build for production, and run tests.

### 1.2 MVC Layout [FE-PROJ-02]

```
/Views
  /Shared           ← layouts, partials, view components
  /FeatureName/
/wwwroot
  /css
    site.css
    /features/
  /js
    site.js
    /features/
  /lib              ← vendored libraries
  /images/
```

### 1.3 React / Next.js Layout [FE-PROJ-03]

```
/src
  /app              ← Next.js App Router pages and layouts
  /components
    /ui             ← design-system primitives (Button, Input, Modal)
    /features       ← feature-specific composed components
    /layouts        ← page shells and navigation
  /hooks            ← custom React hooks
  /lib              ← utilities, API clients, constants
  /types            ← shared TypeScript types and interfaces
  /styles           ← global CSS, Tailwind overrides
/public             ← static assets
```

### 1.4 No Deep Nesting [FE-PROJ-04]

Directory nesting beyond four levels from root signals a hierarchy problem. Use path aliases (`@/lib/format`) — never relative imports like `../../../../shared/utils`.

---

## 2. Naming Conventions [FE-NAME]

### 2.1 Files and Folders [FE-NAME-01]

| Context | Convention | Example |
|---|---|---|
| React components | PascalCase `.tsx` | `RecordingCard.tsx` |
| React hooks | camelCase, `use` prefix | `useRecordings.ts` |
| Utility modules | camelCase `.ts` | `formatDate.ts` |
| MVC views | PascalCase `.cshtml` | `Details.cshtml` |
| MVC partials | PascalCase, `_` prefix | `_RecordingCard.cshtml` |
| CSS files | kebab-case | `recording-card.css` |
| Test files | match source + `.test` | `RecordingCard.test.tsx` |

### 2.2 Variables and Functions [FE-NAME-02]

- camelCase for local variables, functions, parameters, object properties
- PascalCase for React component names, TypeScript interfaces, type aliases, enums
- `UPPER_SNAKE_CASE` for compile-time constants and environment variable keys
- Boolean variables use `is`, `has`, `should`, or `can` prefixes: `isLoading`, `hasPermission`
- Event handlers: `handleSave` for the function, `onSave` for the prop

### 2.3 CSS Classes [FE-NAME-03]

**MVC / Bootstrap:** use Bootstrap utilities first. Custom classes use kebab-case with a feature prefix. Never use generic names like `container`, `wrapper`, or `content` without a prefix — they collide across the application.

**React / Tailwind:** use Tailwind utility classes directly in JSX. When extracting a class, use `@apply` in a CSS module with kebab-case naming. Avoid class names as JavaScript hooks — use `data-*` attributes or refs instead.

---

## 3. Accessibility [FE-A11Y]

Accessible interfaces are not optional. Every new page and every changed component must meet **WCAG 2.1 Level AA**.

### 3.1 Semantic HTML [FE-A11Y-01]

- Use native HTML elements for their intended purpose: `<button>` for actions, `<a>` for navigation
- Never attach click handlers to `<div>` or `<span>` — use `<button>`
- Structure pages with landmarks: `<header>`, `<nav>`, `<main>`, `<aside>`, `<footer>`
- Use heading levels (`<h1>` through `<h6>`) in document order — never skip a level for styling

### 3.2 Keyboard Navigation [FE-A11Y-02]

- Every interactive element is reachable by keyboard alone
- Focus order follows the visual reading order
- Custom components implement the appropriate ARIA pattern (e.g., `role="dialog"` with focus trapping for modals)
- Never suppress the browser's default focus ring without providing an equal or better alternative

### 3.3 Color and Contrast [FE-A11Y-03]

- Text: 4.5:1 contrast ratio against background (3:1 for large text)
- Color is never the sole means of conveying information — pair it with text, icons, or patterns
- Always verify brand colors against WCAG contrast requirements before using them in UI

### 3.4 Forms [FE-A11Y-04]

- Every form input has a visible `<label>` or an `aria-label`
- Error messages are associated with their field via `aria-describedby`
- Required fields are indicated visually and with `aria-required="true"`
- Form submission errors are announced to screen readers via `aria-live="polite"`

### 3.5 Images and Media [FE-A11Y-05]

- Every `<img>` has a meaningful `alt` attribute — or `alt=""` if purely decorative
- Icons used as buttons include `aria-label` on the button element
- Video and audio content include captions or transcripts

### 3.6 Testing [FE-A11Y-06]

Run an axe-core or Lighthouse accessibility audit on every page before PR. Test with keyboard-only navigation. Spot-check critical flows with a screen reader (NVDA on Windows, VoiceOver on macOS).

---

## 4. Responsive Design [FE-RESP]

Every page renders correctly from 320px to 2560px.

- Write styles **mobile-first** — base styles for small viewports, add complexity at larger breakpoints
- Use native breakpoints (Bootstrap `sm/md/lg/xl/xxl` or Tailwind `sm/md/lg/xl/2xl`) — don't override them with desktop-first media queries
- Test at 320px, 768px, 1024px, and 1440px before marking a story done
- Touch targets are at least 44x44px on mobile

---

## 5. Error Handling and User Messaging [FE-ERR]

### 5.1 Principles [FE-ERR-01]

- All errors are caught — uncaught errors that surface as blank screens or console dumps are bugs
- Error messages tell the user what happened and what to do next, not what went wrong internally
- Never show technical error messages, stack traces, or internal identifiers to end users

### 5.2 Error State Coverage [FE-ERR-02]

Every data-fetching component handles three states: loading, success, and error. Every form handles validation errors inline (field-level) and submission errors at the form level.

---

## 6. MVC and Bootstrap 5 [FE-MVC]

### 6.1 View Architecture [FE-MVC-01]

- Views are thin — no business logic in Razor templates
- Shared UI units go in `Views/Shared/Components/` as View Components, not in `Partial` files scattered across feature folders
- JavaScript in MVC projects is feature-scoped: one file per feature in `wwwroot/js/features/`
- No inline JavaScript in `.cshtml` files — keep scripts in `.js` files and initialize on DOMContentLoaded

### 6.2 Bootstrap Usage [FE-MVC-02]

- Use Bootstrap utility classes first — don't write custom CSS for what Bootstrap provides
- Don't override Bootstrap variables in `site.css` — use `_variables.scss` before compilation if customizing
- Document any intentional Bootstrap component overrides with a comment

---

## 7. React, Next.js, TypeScript, and Tailwind [FE-REACT]

### 7.1 Component Architecture [FE-REACT-01]

- Functional components only — no class components
- One component per file; file name matches the component name
- Keep components small: if a component renders more than one distinct section, it probably should be split
- Separate concerns: data fetching hooks live in `/hooks`, presentation components in `/components`

### 7.2 TypeScript [FE-REACT-02]

- TypeScript strict mode is on in every project
- No `any` — use `unknown` and narrow types, or define the shape correctly
- Shared domain types live in `/types` and are reused rather than redefined
- Prefer `interface` for object shapes; use `type` for unions, intersections, and mapped types

### 7.3 State Management [FE-REACT-03]

- Start with local state (`useState`, `useReducer`) — reach for global state only when local state genuinely doesn't work
- Server state (data from APIs) is managed with a data-fetching library (React Query or SWR) — not in Redux or Context
- Don't store derived values in state — compute them from existing state

### 7.4 Tailwind CSS [FE-REACT-04]

- Apply Tailwind utility classes directly in JSX
- Extract repeated utility class combinations into components, not into custom CSS classes
- Use design tokens (spacing, color, typography) from the Tailwind config — don't hardcode values
- The Tailwind config is the source of truth for design decisions; update it rather than writing one-off styles

### 7.5 Next.js Specifics [FE-REACT-05]

- Use the App Router (`/app`) for all new projects
- Prefer Server Components for data fetching — add `'use client'` only when interactivity requires it
- Route-level loading states use `loading.tsx`; error boundaries use `error.tsx`
- Metadata is set via the `Metadata` API, not manual `<head>` tags

### 7.6 Frontend Testing [FE-REACT-06]

- Unit tests cover utility functions and custom hooks
- Component tests (React Testing Library) cover user interactions and conditional rendering
- Avoid testing implementation details — test what the user sees and does
- Critical user flows (login, checkout, form submission) have end-to-end test coverage
