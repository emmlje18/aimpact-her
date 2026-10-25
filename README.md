# Aiventura

AIventura is an educational app on AI safety & AI literacy for educators and students around the world.

It is a whole-class adventure for children aged 10–12 and their teachers: students learn to **write great prompts**, **spot AI mistakes** and **keep private information safe**, through short lessons and classroom games run from a projector, phones or paper.

## The journey

The journey map (`/journey`) has three phases of five steps each. Each phase ends with a challenge. Progress is stored in the browser on the classroom device.

| Phase | Theme | Example steps |
| --- | --- | --- |
| 1 | Write great prompts | Fix the prompt, Pick the right style, Prompt master |
| 2 | Spot the mistakes | True or not true?, Find a source, Mistake hunt |
| 3 | Stay safe | What is private?, Spot the secret, Safety badge |

## Games

- **Fix the Prompt** (`/game/fix-the-prompt`): learn the four parts of a good prompt (who it is for, what you want, how to explain, how much) by improving *"Tell me about climate change."* Pieces that share personal details or ask for too much are flagged.
- **Prompt It Clearly** (`/game/say-it-clearly`): students write step-by-step instructions for Bolt the robot chef to make a peanut butter and jelly sandwich. Bolt follows them *exactly*, so vague steps lead to funny mess.
- **Play in class** (`/play/host`, `/play/join`): the teacher shows a room QR code on the big screen. Students join on their phones with a random nickname (no real names) and pick a team. The classroom demo is a standalone page in `public/classroom/`.
- **Offline print pack** (`/print/fix-the-prompt`): 12 printable A4 pages (teacher guide, worksheet, cut-out cards, extra practice, answer key) for running Fix the Prompt without screens.

### Safety by design

Children's free-text prompts pass through `src/lib/safety.ts` before anything is sent to an AI. It blocks personal details (names, ages, addresses, schools, numbers, emails), real names of teachers, unkind descriptions, off-topic content and over-long prompts. The moderator is a swappable function, so a hosted moderation API can replace the keyword list later.

## Tech stack

- [TanStack Start](https://tanstack.com/start) (React 19, file-based routing in `src/routes/`, server functions)
- Tailwind CSS 4 and shadcn/ui (Radix) components
- Vite, built for Cloudflare via Nitro (through `@lovable.dev/vite-tanstack-config`)
- Vitest and Testing Library
- Supabase (Postgres) for class play rooms, teams and players; schema and migrations managed with Drizzle in `drizzle/`

## Getting started

Requires Node.js 20.19+ or 22.12+ (for Vite 8) and npm (or [Bun](https://bun.sh); a `bun.lock` is included).

```sh
git clone https://github.com/emmlje18/aiventura.git
cd aiventura
npm install
npm run dev        # http://localhost:3000
```

Brand images are served from Lovable's asset host. To see them on localhost, run `npm run dev:local`, which proxies them from the project's Lovable preview.

### Environment variables

Set these in `.env`. The committed `.env` holds only the Supabase project's public (publishable) settings; keep secret keys such as AI keys out of git, for example in `.env.local`.

| Variable | Purpose |
| --- | --- |
| `ANTHROPIC_API_KEY` | Powers Bolt in *Prompt It Clearly* with Claude (preferred). Billed to your Anthropic account. |
| `LOVABLE_API_KEY` | Alternative: uses the Lovable AI gateway when no Anthropic key is set. Uses the Lovable workspace's AI credits. |
| `BOLT_MODEL` | Optional model override for either provider. |
| `SUPABASE_URL`, `SUPABASE_PUBLISHABLE_KEY`, `SUPABASE_PROJECT_ID` (and `VITE_` copies) | Supabase project connection. |

Bolt tries Claude first, then Lovable AI. With neither key set, or if the AI call fails, *Prompt It Clearly* still works: it falls back to a free, rule-based practice robot (no AI) that recognises common kitchen words such as "open the bag", "knife" and "spread".

### Scripts

| Command | What it does |
| --- | --- |
| `npm run dev` | Start the dev server |
| `npm run dev:local` | Dev server with Lovable brand-asset proxy |
| `npm run build` | Production build |
| `npm run preview` | Preview the production build |
| `npm test` | Run the test suite once |
| `npm run lint` / `npm run format` | ESLint / Prettier |

## Project structure

```
src/
  routes/          Pages (cover, journey map, games, class play, print pack)
  components/
    brand/         Logo, mascot, icons, back navigation
    play/          Game scenes (Bolt's kitchen, prompt checks)
    ui/            shadcn/ui components
  lib/             Game logic, journey data, safety checks, AI server function
  test/            Routing tests and test setup
public/classroom/  Standalone classroom QR demo
drizzle/           Database schema and migrations
supabase/          Supabase config
```

`AGENTS.md` holds navigation and design conventions for contributors and coding agents; `roadmap.md` tracks completed work.

## Credits

Co-built by [emmlje18](https://github.com/emmlje18) and [Aisyah](https://github.com/aisyahlyana). Development started in [aisyahlyana/aiventura](https://github.com/aisyahlyana/aiventura), where both of us committed from that account, so its history is shared work.
