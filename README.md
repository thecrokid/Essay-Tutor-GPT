# Essay Tutor GPT

Essay Tutor GPT is a simple Next.js web app that lets a user paste an essay, send it to an OpenAI-powered reviewer, and receive structured feedback in clear sections.

## Features

- Paste an essay into a clean review interface
- Submit the essay to an AI reviewer using a dedicated system prompt
- View structured feedback for:
  - Thesis and Argument
  - Structure and Organization
  - Evidence and Support
  - Critical Thinking
  - Clarity and Style
  - Strengths
  - Top 5 Revisions

## Tech Stack

- Next.js (App Router)
- React
- TypeScript
- OpenAI Node SDK

## Local Setup

Use Node.js 18.17 or newer.

1. Install dependencies:

   ```bash
   npm install
   ```

2. Create a local environment file:

   ```bash
   cp .env.example .env.local
   ```

3. Add your OpenAI API key to `.env.local`:

   ```bash
   OPENAI_API_KEY=your_openai_api_key_here
   OPENAI_MODEL=gpt-4o-mini
   ```

4. Start the development server:

   ```bash
   npm run dev
   ```

5. Open [http://localhost:3000](http://localhost:3000)

## Notes

- The original request referenced a "prompt below", but no separate prompt text was provided. This project includes a built-in reviewer system prompt in `lib/review.ts` that matches the required feedback structure.
- The API route is implemented at `app/api/review/route.ts`.
- The frontend is implemented in `app/page.tsx`.

## Deploy to Vercel

1. Push this repository to GitHub.
2. In Vercel, create a new project and import the repository.
3. Add environment variables in Vercel Project Settings:
   - `OPENAI_API_KEY` (required)
   - `OPENAI_MODEL` (optional, defaults to `gpt-4o-mini`)
4. Deploy.

Before deploying, verify locally:

```bash
npm run lint
npm run build
```
