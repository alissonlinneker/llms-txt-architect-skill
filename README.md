<p align="center">
  <img src="logo.svg" alt="llms.txt Architect" width="400">
</p>

<p align="center">
  <em>Claude Code skill that generates production-ready llms.txt files following the <a href="https://llmstxt.org">llmstxt.org</a> standard — making your website discoverable by AI models and agents.</em>
</p>

---

While traditional SEO optimizes for search engine crawlers, **GEO (Generative Engine Optimization)** optimizes for LLMs like ChatGPT, Claude, Perplexity, and Gemini. The `llms.txt` file is to AI what `robots.txt` is to search engines — except instead of granting permission, it provides context.

This skill reads your entire codebase, understands your project, and generates structured files that help AI models recommend your product, cite your services, and understand your competitive advantages.

## What It Does

| Output | Description |
|--------|-------------|
| `/llms.txt` | Compact index in primary language with links to pages |
| `/llms-full.txt` | Expanded version with all content inline (packages, pricing, FAQ, specs) |
| `/llms-en.txt` | Compact English version for international reach |
| `/llms-full-en.txt` | Expanded English version with full details |
| HTML `<link>` tags | Discovery tags for AI crawlers in your document head |
| `robots.txt` validation | Ensures AI crawlers (GPTBot, ClaudeBot, etc.) are not blocked |

## Why This Skill?

Existing llms.txt generators either:
- Only produce a single file (no full version, no multilingual)
- Focus on open-source documentation (not commercial products/services)
- Require external APIs or scrapers

**llms-txt-architect** works differently:
- Reads your **actual codebase** — constants, pages, components, SEO config, pricing data
- Generates **4 files** covering compact + full × 2 languages
- Focuses on **commercial positioning** — differentiators, certifications, client logos, pricing
- Configures **HTML discovery** and validates **robots.txt**
- Runs entirely inside Claude Code — no external dependencies, no API keys

## Installation

### Option A: Install from GitHub (recommended)

```bash
claude install alissonlinneker/llms-txt-architect-skill
```

After installation, use it in any project:

```
/llms-txt-architect
```

Or with arguments:

```
/llms-txt-architect --output static/ --primary-lang pt --secondary-lang en
```

### Option B: Manual installation

1. Clone this repository:

```bash
git clone https://github.com/alissonlinneker/llms-txt-architect-skill.git
```

2. Copy the skill folder into your Claude Code skills directory:

```bash
# Global (available in all projects)
cp -r llms-txt-architect/skills/llms-txt-architect ~/.claude/skills/

# Or project-local (available only in current project)
mkdir -p .claude/skills
cp -r llms-txt-architect/skills/llms-txt-architect .claude/skills/
```

3. Restart Claude Code or start a new session.

### Option C: Direct SKILL.md copy

If you only want the skill without cloning:

1. Create the directory:

```bash
mkdir -p ~/.claude/skills/llms-txt-architect
```

2. Copy the `SKILL.md` file from `skills/llms-txt-architect/SKILL.md` into that directory.

## Usage

Once installed, invoke the skill in Claude Code:

```
/llms-txt-architect
```

The skill will:

1. **Scan your codebase** — pages, components, constants, SEO config, pricing, translations
2. **Ask targeted questions** — primary language, output directory, company details not found in code
3. **Generate 4 files** — llms.txt, llms-full.txt, llms-en.txt, llms-full-en.txt
4. **Configure HTML** — add `<link rel="alternate">` tags in your document head
5. **Validate robots.txt** — check that AI crawlers are allowed

### Arguments

| Argument | Default | Description |
|----------|---------|-------------|
| `--output <dir>` | `static/` or `public/` (auto-detected) | Output directory for generated files |
| `--primary-lang <code>` | Auto-detected from project | Primary language (e.g., `pt`, `en`, `es`, `fr`) |
| `--secondary-lang <code>` | `en` (or `pt` if primary is `en`) | Secondary language for international version |
| `--skip-html` | `false` | Skip HTML `<link>` tag configuration |
| `--skip-robots` | `false` | Skip robots.txt validation |

### Example Output

After running, your project will have:

```
static/
├── llms.txt            ← Primary language, compact
├── llms-full.txt       ← Primary language, full
├── llms-en.txt         ← English, compact
├── llms-full-en.txt    ← English, full
└── robots.txt          ← Validated (AI crawlers allowed)
```

And your HTML head will include:

```html
<link rel="alternate" type="text/markdown" title="LLM-friendly version" href="/llms.txt" />
<link rel="alternate" type="text/markdown" title="LLM-friendly version (English)" href="/llms-en.txt" hreflang="en" />
```

## llms.txt Format Reference

The [llmstxt.org](https://llmstxt.org) standard defines this structure:

```markdown
# Project Name

> One-paragraph summary with the most critical information about the project.

## Section Name

- [Link Title](https://example.com/page): Brief description of what this links to
- [Another Link](https://example.com/other): Another description

## Another Section

Details as plain text or more links.
```

**Rules:**
- **H1** (required): Project/site name — only one
- **Blockquote** (recommended): Short summary immediately after H1
- **Body text** (optional): Important context, caveats, or notes
- **H2 sections**: Categorized content with links in `[Title](url): description` format
- **No H3+**: The standard only uses H1 and H2 headers
- **Plain Markdown**: No HTML, no frontmatter, no special syntax

**llms-full.txt** follows the same structure but includes all content inline instead of linking to pages.

## Framework Support

The skill auto-detects your framework to find the right output directory and HTML head file:

| Framework | Output Dir | HTML Head |
|-----------|-----------|-----------|
| Gatsby | `static/` | `src/html.js` |
| Next.js | `public/` | `app/layout.tsx` or `pages/_document.tsx` |
| Nuxt | `public/` | `nuxt.config.ts` or `app.vue` |
| Astro | `public/` | `src/layouts/*.astro` |
| SvelteKit | `static/` | `src/app.html` |
| Vite/React | `public/` | `index.html` |
| Plain HTML | Root `/` | `index.html` |

## AI Crawlers Validated in robots.txt

The skill checks that these user-agents are not blocked:

- `GPTBot` (OpenAI)
- `ChatGPT-User` (OpenAI)
- `OAI-SearchBot` (OpenAI)
- `ClaudeBot` (Anthropic)
- `anthropic-ai` (Anthropic)
- `Claude-Web` (Anthropic)
- `PerplexityBot` (Perplexity)
- `Google-Extended` (Google AI)
- `Bytespider` (ByteDance)
- `CCBot` (Common Crawl)
- `Amazonbot` (Amazon)
- `Meta-ExternalAgent` (Meta)
- `FacebookBot` (Meta)
- `YouBot` (You.com)
- `Diffbot` (Diffbot)
- `cohere-ai` (Cohere)

## Post-Generation Checklist

After running the skill:

- [ ] Review generated files for accuracy
- [ ] Verify all URLs are correct and accessible
- [ ] Deploy to production
- [ ] Test: `curl https://yourdomain.com/llms.txt`
- [ ] Submit to [llmstxt.site/submit](https://llmstxt.site/submit) and [directory.llmstxt.cloud](https://directory.llmstxt.cloud)
- [ ] Verify HTML source has `<link>` tags
- [ ] Test robots.txt: `curl https://yourdomain.com/robots.txt`

## Contributing

Contributions welcome! Please open an issue first to discuss proposed changes.

## License

MIT
