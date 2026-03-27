# llms-txt-architect

Claude Code skill for generating production-ready llms.txt files following the llmstxt.org standard.

## Project Structure

```
llms-txt-architect/
├── README.md                          # Documentation for GitHub
├── LICENSE                            # MIT License
└── skills/
    └── llms-txt-architect/
        └── SKILL.md                   # The skill itself
```

## Key Facts

- **Type:** Claude Code skill (no runtime, no dependencies — just Markdown instructions)
- **Install:** `claude install alissonlinneker/llms-txt-architect-skill`
- **Invoke:** `/llms-txt-architect`
- **Output:** 4 files (llms.txt, llms-full.txt, llms-{lang}.txt, llms-full-{lang}.txt)
- **Standard:** llmstxt.org

## Editing Rules

- The SKILL.md is the core deliverable — changes there affect all users
- README.md must stay in sync with SKILL.md capabilities
- Keep the skill self-contained (no external dependencies or files)
- Frontmatter fields: only `name`, `description`, `argument-hint`
