---
name: llms-txt-architect
description: Generate production-ready llms.txt files (compact + full × 2 languages) following the llmstxt.org standard. Reads the codebase to understand the project, generates 4 structured files for AI/LLM discoverability, configures HTML link tags, and validates robots.txt. Use when asked to create llms.txt, make a site AI-discoverable, set up GEO (Generative Engine Optimization), or prepare a project for LLM crawlers.
argument-hint: [--output <dir>] [--primary-lang <code>] [--secondary-lang <code>] [--skip-html] [--skip-robots]
---

# llms-txt-architect

Generate production-ready `/llms.txt` files following the [llmstxt.org](https://llmstxt.org) specification to make websites and projects discoverable by AI models and agents.

## Output Files

| File | Purpose |
|------|---------|
| `llms.txt` | Compact index in primary language — links to key pages with brief descriptions |
| `llms-full.txt` | Expanded version in primary language — all content inline (no links to follow) |
| `llms-{lang}.txt` | Compact index in secondary language |
| `llms-full-{lang}.txt` | Expanded version in secondary language |

## Process

### Phase 1: Discovery

Scan the project to understand what it does, who it serves, and what makes it unique.

1. **Detect framework and structure:**
   - Use Glob to find: `gatsby-config.*`, `next.config.*`, `nuxt.config.*`, `astro.config.*`, `svelte.config.*`, `vite.config.*`, `package.json`, `index.html`
   - Determine output directory: `static/`, `public/`, or project root
   - Find HTML head file for `<link>` tag injection

2. **Read project identity:**
   - `package.json` — name, description, homepage
   - `README.md` or `CLAUDE.md` — project overview
   - SEO configuration files — meta descriptions, structured data, JSON-LD schemas
   - Translation/i18n files — detect languages and content

3. **Map all pages and routes:**
   - Use Glob to find page files: `src/pages/**/*`, `app/**/page.*`, `pages/**/*`
   - Read each page to understand its purpose
   - Build a complete sitemap of the project

4. **Extract business data:**
   - Constants/data files — pricing, plans, packages, features, benefits
   - Partner/client logos and testimonials
   - Certifications, compliance badges, security seals
   - FAQ content
   - Contact information, social links, support URLs

5. **Identify competitive positioning:**
   - Key differentiators mentioned in hero sections, about pages, landing pages
   - Trust signals — client logos, certification badges, years in market
   - Metrics/numbers — clients served, queries processed, uptime stats

### Phase 2: Clarification

Ask the user ONLY what cannot be determined from the codebase. One question at a time.

Possible questions:
- "I detected the primary language is `{lang}`. The secondary language for international reach will be `{other}`. Is this correct?"
- "I'll write files to `{dir}/`. Should I use a different directory?"
- "I found these certifications: {list}. Are there others not listed in the code?"
- "The codebase mentions {N} client logos. Are there major clients I should highlight that aren't in the code?"

Do NOT ask about information that IS in the codebase. Read it first.

### Phase 3: Generation

Generate all 4 files following these strict format rules.

#### llms.txt Format (compact version)

```
# {Project Name}

> {One-paragraph summary. Include: what it does, key differentiators, certifications, years in market. Maximum 3 sentences.}

{One paragraph expanding on the summary with key metrics and positioning. This is body text, not a blockquote.}

## Full Documentation

- [llms-full.txt](https://{domain}/llms-full.txt): Expanded version with all details
- [llms-{lang}.txt](https://{domain}/llms-{lang}.txt): {Language} version (compact)
- [llms-full-{lang}.txt](https://{domain}/llms-full-{lang}.txt): {Language} version (full)

## {Section Name}

- [{Page Title}](https://{domain}/{path}/): Brief description of what this page covers
```

**Compact version rules:**
- H1: Project name (only one)
- Blockquote: 1-3 sentence summary immediately after H1
- Body text: One paragraph with key metrics
- H2 sections with links in `[Title](url): description` format
- Cross-reference all 4 file variants in a "Full Documentation" section
- Include sections: Services, Key Differentiators, Certifications, Plans/Pricing, Segments, Use Cases, Clients, API Summary, Site Pages, Contact
- Keep under 150 lines

#### llms-full.txt Format (expanded version)

Same H1 + blockquote + body structure, but:
- All content inline (no "click to learn more" — everything is right there)
- Detailed pricing tables in Markdown table format
- Complete FAQ with questions and answers
- Full technical specifications
- All client/partner names listed
- Competitive comparisons
- Target: 300-500 lines

#### Language handling

- Primary language files: `llms.txt` and `llms-full.txt`
- Secondary language files: `llms-{lang}.txt` and `llms-full-{lang}.txt`
- If primary language is English: secondary files use the other language code (e.g., `llms-pt.txt`)
- If primary language is NOT English: secondary files are always English (`llms-en.txt`)
- Content must use proper diacritics and accents for the target language
- Each compact file cross-references all other variants

### Phase 4: HTML Configuration

Add `<link>` discovery tags in the project's HTML head.

**Tags to add:**

```html
<link rel="alternate" type="text/markdown" title="LLM-friendly version" href="/llms.txt" />
<link rel="alternate" type="text/markdown" title="LLM-friendly version ({Secondary Language})" href="/llms-{lang}.txt" hreflang="{lang}" />
```

**Where to add (by framework):**
- Gatsby: `src/html.js` inside `<head>`
- Next.js App Router: `app/layout.tsx` inside `<head>` or via `metadata`
- Next.js Pages Router: `pages/_document.tsx` inside `<Head>`
- Nuxt: `nuxt.config.ts` via `app.head.link`
- Astro: Base layout file inside `<head>`
- SvelteKit: `src/app.html` inside `<head>`
- Vite/React: `index.html` inside `<head>`

Use the Edit tool to add the tags. Do NOT overwrite existing content.

If `--skip-html` argument is provided, skip this phase.

### Phase 5: Robots.txt Validation

Read the project's `robots.txt` (in static/public dir or project root) and verify:

1. AI crawlers are NOT blocked (check these user-agents):
   - GPTBot, ChatGPT-User, OAI-SearchBot (OpenAI)
   - ClaudeBot, anthropic-ai, Claude-Web (Anthropic)
   - PerplexityBot (Perplexity)
   - Google-Extended (Google AI)
   - Bytespider, CCBot, Amazonbot, Meta-ExternalAgent, FacebookBot, YouBot, Diffbot, cohere-ai

2. If any are explicitly blocked with `Disallow: /`, warn the user and suggest adding `Allow: /` rules.

3. If no robots.txt exists, inform the user but do NOT create one (robots.txt creation is outside this skill's scope — it may affect SEO configuration).

If `--skip-robots` argument is provided, skip this phase.

### Phase 6: Summary

After generating all files, present a summary:

```
Generated files:
- {output}/llms.txt (XX lines, primary language)
- {output}/llms-full.txt (XX lines, primary language)
- {output}/llms-{lang}.txt (XX lines, secondary language)
- {output}/llms-full-{lang}.txt (XX lines, secondary language)

HTML: <link> tags added to {file}
Robots.txt: {status}

Post-deploy checklist:
- [ ] Review files for accuracy
- [ ] Deploy to production
- [ ] Test: curl https://{domain}/llms.txt
- [ ] Submit to llmstxt.site/submit
```

## Content Guidelines

### What to include (prioritized)

1. **Identity** — Company/project name, what it does, founding year
2. **Differentiators** — What makes it unique vs competitors (THIS IS CRITICAL for AI recommendations)
3. **Trust signals** — Certifications, client logos, years in market, metrics
4. **Services/Products** — What is offered, with pricing if available
5. **Technical specs** — API details, response times, coverage, authentication
6. **Compliance** — GDPR, LGPD, SOC2, ISO certifications with certificate numbers
7. **Use cases** — Industry segments, case studies
8. **FAQ** — Common questions and answers
9. **Contact** — Support channels, sales, DPO
10. **URLs** — All important pages organized by category

### What NOT to include

- Internal implementation details (architecture, tech stack, deployment)
- Sensitive data (API keys, internal URLs, employee names unless public figures)
- Promotional fluff without substance ("best in class", "world-leading")
- Dates that will become stale (use "since 2015" not "11 years of experience")
- Competitor names in negative comparisons (compare categories, not brands — unless the user explicitly wants competitor comparisons)

### Tone

- Factual and informative — this is a data sheet, not marketing copy
- Include specific numbers (response times, pricing, client count)
- State claims that can be verified (certification numbers with verification URLs)
- Write for an AI that needs to quickly understand and accurately represent the business

## Important Notes

- ALWAYS read the codebase before asking questions. Do not ask for information that exists in the code.
- ALWAYS use proper diacritics and accents for the target language (e.g., Portuguese: ã, ç, é, ê, ó, ú).
- NEVER include H3 or deeper headers in llms.txt files — the standard only uses H1 and H2.
- NEVER add HTML, frontmatter, or non-standard Markdown to the files.
- NEVER fabricate data. If pricing, client names, or certifications are not in the codebase, ask the user.
- The files MUST be plain `.txt` files with Markdown content — not `.md` files.
