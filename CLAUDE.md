# CLAUDE.md — Project Rules & Instructions

This file defines project-level rules for Claude Code. These are the "unwritten rules" experienced developers know and follow consistently. Read and apply these before making any change.

---

## General Principles

- **Never hardcode values** — use design tokens, environment variables, or constants.
- **Reuse before you create** — always check for an existing component, utility, or pattern before writing a new one.
- **Match the codebase style** — follow the conventions already in use; don't introduce new patterns without a clear reason.
- **Accessibility first** — meet WCAG 2.1 AA requirements for all UI work (contrast, focus states, ARIA labels, keyboard navigation).
- **No inline styles** unless absolutely necessary and temporary. Use design system tokens or class utilities instead.

---

## Project Structure

```
/
├── .github/
│   └── workflows/       # GitHub Actions — do not modify without understanding impact
├── src/
│   ├── components/      # Reusable UI components (atomic/shared)
│   ├── features/        # Feature-scoped modules (co-locate logic + UI)
│   ├── layouts/         # Page layout primitives
│   ├── styles/          # Global styles, design tokens, theme config
│   ├── assets/          # Images, fonts, static files
│   └── utils/           # Shared utility functions
```

### Rules
- UI components belong in `src/components/`. One component per file.
- Feature-specific code (hooks, local state, API calls) lives in `src/features/<feature-name>/`.
- Layout primitives (containers, grids, page wrappers) go in `src/layouts/`.
- Never co-locate unrelated features.

---

## Component Rules

- **Naming**: PascalCase for components (`UserCard.tsx`, not `user-card.tsx`).
- **Files**: One component per file. File name matches component name.
- **Props**: Define a typed `Props` interface at the top of each component file.
- **No magic numbers**: Extract all numeric values (sizes, durations, z-indices) into named constants or tokens.
- **Document non-obvious behavior** with a comment — don't document the obvious.

```tsx
// Good
interface Props {
  userId: string;
  isActive: boolean;
}

export function UserCard({ userId, isActive }: Props) { ... }
```

---

## Styling

- Use the project's design system tokens for **all** color, spacing, typography, and shadow values.
- Avoid Tailwind utility classes that duplicate design token values — prefer the token.
- Global styles live in `src/styles/global.css`. Do not scatter global resets or overrides elsewhere.
- Responsive breakpoints are defined in the theme config — don't invent new ones.

---

## Design Tokens

- Tokens are defined in `src/styles/tokens/`.
- Never hardcode hex colors, `px` sizes, or font families — always reference a token.
- When a Figma design specifies a value that doesn't map to an existing token, ask before creating a new one.

---

## Asset Management

- Static assets (images, fonts) go in `src/assets/`.
- Reference assets via import or a project-standard asset helper — no hardcoded paths.
- Optimize images before committing (compress, use correct format: WebP preferred for photos).
- **Do not add new icon packages** — all icons should come from the established icon system or Figma payload.

---

## MCP Servers

### Figma MCP Server Rules

These rules apply to all Figma-driven implementation work and must be followed in order.

#### Required flow (do not skip steps)

1. Run `get_design_context` first to fetch the structured representation for the exact node(s).
2. If the response is too large or truncated, run `get_metadata` to get the high-level node map, then re-fetch only the required node(s) with `get_design_context`.
3. Run `get_screenshot` for a visual reference of the node variant being implemented.
4. Only after you have both `get_design_context` and `get_screenshot`, download any needed assets and begin implementation.
5. Translate the output into this project's conventions, styles, and framework. Reuse existing color tokens, components, and typography.
6. Validate against the Figma screenshot for 1:1 look and behavior before marking complete.

#### Implementation rules

- Treat Figma MCP output (React + Tailwind) as a **design and behavior reference**, not as final production code.
- Replace Tailwind utility classes with the project's preferred utilities/design-system tokens when applicable.
- Reuse existing components (buttons, inputs, typography, icon wrappers) instead of duplicating functionality.
- Use the project's color system, typography scale, and spacing tokens consistently.
- Respect existing routing, state management, and data-fetch patterns already in the repo.
- Strive for 1:1 visual parity with the Figma design. When conflicts arise, prefer design-system tokens and adjust spacing or sizes minimally to match.
- Validate the final UI against the Figma screenshot for both visual appearance and behavior.

#### Asset handling from Figma MCP

- If the Figma MCP server returns a `localhost` source for an image or SVG, **use that source directly** — do not replace it with a placeholder or re-fetch it.
- **Do NOT import or add new icon packages** — all icons should come from the Figma payload.
- **Do NOT create placeholder assets** if a localhost source is provided.

---

## GitHub Workflows

- Workflows live in `.github/workflows/`. Changes here affect CI/CD for all contributors.
- `claude.yml` — handles interactive Claude Code responses to `@claude` mentions in issues/PRs.
- `claude-code-review.yml` — automatically reviews all incoming PRs.
- Do not disable or bypass these workflows without explicit team approval.

---

## What to Never Do

- Never hardcode API keys, secrets, tokens, or environment-specific URLs.
- Never commit `.env` files or credentials.
- Never skip accessibility attributes on interactive elements.
- Never add a new dependency without checking if the functionality already exists in the project.
- Never push directly to `main` — all changes go through pull requests.
- Never create a placeholder image or icon if the Figma MCP server has already provided a source.
