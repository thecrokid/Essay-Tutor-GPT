# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev      # Start dev server (also auto-opens browser via scripts/open-browser.js)
npm run build    # Production build — run this after every change to confirm no errors
npm run lint     # ESLint via next lint
npm run start    # Start production server (requires build first)
npx vercel --prod  # Deploy to Vercel (no GitHub auth needed — Vercel CLI is configured)
```

Always run `npm run lint && npm run build` before deploying.

## Environment

Requires `.env.local` (copy from `.env.example`):
- `OPENAI_API_KEY` — required, server-side only, never exposed to the client
- `OPENAI_MODEL` — optional, defaults to `gpt-4o-mini`

## Architecture

**Next.js 14 App Router** with TypeScript and Tailwind CSS.

### Data flow
1. User fills in essay + options on the home page (`app/page.tsx` → `components/sections/EssayInput.tsx`)
2. Submit calls `POST /api/review` (`app/api/review/route.ts`) which calls OpenAI and returns a `ReviewResponse`
3. Result is stored in `sessionStorage` under the key from `mockResultStorageKey` and the user is redirected to `/results`
4. `/results` (`app/results/page.tsx`) reads sessionStorage and renders the scored feedback
5. `/api/chat` (`app/api/chat/route.ts`) handles the follow-up interactive tutoring chat

### Key files
- `lib/analysis/review.ts` — `ReviewResponse` type, `reviewerSystemPrompt`, all scoring section definitions. **The `/api/review` response shape must match this type exactly.**
- `lib/analysis/scoringEngine.ts` — `generateMockAnalysis`, `AnalysisResult`, `ScoreKey` types used by the results page
- `lib/analysis/textParser.ts` — `estimateWordCount`, `firstSentence`, `splitIntoParagraphs` utilities
- `lib/utils.ts` — `cn()` helper (clsx + tailwind-merge)

### API contract (`/api/review`)
Response must always include:
```json
{
  "review": {
    "thesisAndArgument": "string",
    "structureAndOrganization": "string",
    "evidenceAndSupport": "string",
    "criticalThinking": "string",
    "clarityAndStyle": "string",
    "strengths": ["2–5 strings"],
    "top5Revisions": ["exactly 5 strings"],
    "scores": { ... },
    "highlights": [ ... ]
  },
  "warning": "string | null"
}
```

### Component structure
- `components/sections/` — full-page section components (Hero, Navbar, Footer, EssayInput, etc.)
- `components/shared/` — reusable utility components (`AnimateIn` scroll-reveal wrapper)
- `components/ui/` — primitive UI components (`DropdownMenu`, `Button`)

### Styling
Global CSS variables are defined in `app/globals.css` (light cream theme: `--background`, `--foreground`, `--accent`, `--border`, etc.). Components use these via `var(--token)` or Tailwind arbitrary values like `text-[var(--foreground)]`. The `.panel`, `.eyebrow`, `.btn-primary`, `.btn-secondary` utility classes are defined globally. Scroll-reveal animations use the `AnimateIn` wrapper component or the `.animate-on-scroll` / `.in-view` CSS classes.

### Path alias
`@/` maps to the repo root — use it for all internal imports.
