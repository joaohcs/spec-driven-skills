# Design System

## Principles

[2-3 design principles. Defaults:]
- **Clarity over decoration** — every element should serve a purpose. Avoid ornamental UI.
- **Consistent density** — maintain uniform spacing and sizing across all views.
- **Accessible by default** — meet WCAG 2.1 AA. Proper contrast, focus states, semantic HTML.

## Color palette

### Brand colors
| Token | Light | Dark | Usage |
|-------|-------|------|-------|
| `--primary` | [e.g., #18181B] | [e.g., #FAFAFA] | Primary actions, links, key UI elements |
| `--primary-foreground` | [e.g., #FAFAFA] | [e.g., #18181B] | Text on primary backgrounds |

### Semantic colors
| Token | Light | Dark | Usage |
|-------|-------|------|-------|
| `--background` | [e.g., #FFFFFF] | [e.g., #09090B] | Page background |
| `--foreground` | [e.g., #09090B] | [e.g., #FAFAFA] | Default text |
| `--muted` | [e.g., #F4F4F5] | [e.g., #27272A] | Subtle backgrounds, disabled states |
| `--muted-foreground` | [e.g., #71717A] | [e.g., #A1A1AA] | Secondary text, placeholders |
| `--accent` | [e.g., #F4F4F5] | [e.g., #27272A] | Hover states, selected items |
| `--destructive` | [e.g., #EF4444] | [e.g., #EF4444] | Errors, delete actions, warnings |
| `--border` | [e.g., #E4E4E7] | [e.g., #27272A] | Borders, dividers |
| `--ring` | [e.g., #18181B] | [e.g., #D4D4D8] | Focus rings |

### Status colors
| Token | Color | Usage |
|-------|-------|-------|
| `--success` | [e.g., #22C55E] | Success states, confirmations |
| `--warning` | [e.g., #F59E0B] | Warnings, attention needed |
| `--info` | [e.g., #3B82F6] | Informational messages |

**Note:** These map to shadcn/ui's CSS custom property system. Update them in `globals.css` under `:root` and `.dark`.

## Typography

| Element | Font | Size | Weight | Line height |
|---------|------|------|--------|-------------|
| **Heading 1** | Inter | 2.25rem (36px) | 700 (bold) | 1.2 |
| **Heading 2** | Inter | 1.875rem (30px) | 600 (semibold) | 1.25 |
| **Heading 3** | Inter | 1.5rem (24px) | 600 (semibold) | 1.3 |
| **Heading 4** | Inter | 1.25rem (20px) | 600 (semibold) | 1.4 |
| **Body** | Inter | 0.875rem (14px) | 400 (regular) | 1.5 |
| **Body small** | Inter | 0.8125rem (13px) | 400 (regular) | 1.5 |
| **Label** | Inter | 0.875rem (14px) | 500 (medium) | 1.4 |
| **Code** | JetBrains Mono / monospace | 0.8125rem (13px) | 400 | 1.6 |

**Font loading:** Use `next/font` (Next.js) or `@fontsource/inter` (Vite) to self-host fonts.

## Spacing

Use Tailwind's default spacing scale (4px base). Key values:
- **4px** (`p-1`) — tight internal padding
- **8px** (`p-2`, `gap-2`) — standard internal spacing
- **12px** (`p-3`, `gap-3`) — comfortable padding
- **16px** (`p-4`, `gap-4`) — section padding, card padding
- **24px** (`p-6`, `gap-6`) — between sections
- **32px** (`p-8`, `gap-8`) — page-level spacing
- **48px** (`p-12`) — major section separation

## Border radius

Follow shadcn/ui's radius system via CSS custom property:
- `--radius`: `0.5rem` (8px) — default for cards, dialogs, dropdowns
- Buttons and inputs: `calc(var(--radius) - 2px)` (6px)
- Small badges/chips: `calc(var(--radius) - 4px)` (4px)
- Full round: `9999px` — avatars, circular buttons

## Shadows

Minimal shadow usage. Prefer borders for elevation:
- `shadow-sm` — dropdowns, popovers
- `shadow-md` — dialogs, modals
- Avoid `shadow-lg` and above in most cases

## Components

### Component library
Using **shadcn/ui** — components are copied into `src/components/ui/` and owned by the project. They are not a dependency — modify them freely to match the design system.

### Patterns
- **Forms:** Use shadcn/ui Form components with `react-hook-form` + `zod` validation.
- **Data display:** Use shadcn/ui Table for tabular data. Card for summary blocks.
- **Feedback:** Use `sonner` (toast) for success/error feedback. Inline alerts for persistent messages.
- **Navigation:** Consistent sidebar or top nav. Use shadcn/ui NavigationMenu or custom.
- **Empty states:** Always design for empty. Show a clear message and a primary action.
- **Loading states:** Use skeleton loaders (shadcn/ui Skeleton) over spinners where possible.
- **Error states:** Show what went wrong and what the user can do about it.

## Responsive behavior

- **Mobile-first** — design for mobile, then enhance for larger screens
- Breakpoints follow Tailwind defaults: `sm` (640px), `md` (768px), `lg` (1024px), `xl` (1280px)
- Navigation collapses to hamburger/sheet on mobile
- Tables scroll horizontally on small screens or switch to card layout
- Touch targets minimum 44x44px on mobile

## Dark mode

[Supported / Not supported / TBD]

If supported:
- Use CSS custom properties (shadcn/ui pattern) for all colors
- Toggle via `class` strategy on `<html>` element
- Store preference in `localStorage`, respect `prefers-color-scheme` as default
- Test all components in both modes
