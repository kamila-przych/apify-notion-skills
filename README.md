# Apify Notion skills

Two Claude skills from my Apify Notion knowledge base case study (clean-up pilot).

## Why

Many Notion searches for a basic company question return an old page, a copy or personal notes. These skills help to clean up existing pages and to write new pages to one standard, for people and AI agents.

## What is inside

| Skill | What it does | Use it when |
|---|---|---|
| `notion-cleanup-priority` | Sets a draft Priority (Reach x Risk) and draft Suggested action tags on Notion pages. | New or unscored pages appear in the wiki or in a Notion export. |
| `notion-page-builder` | Helps an Apifier create or review a page so it follows the Apify page standard. | Someone wants to write, update or review a company page in Notion. |

Both skills only draft. A person confirms tags, and the page owner publishes and verifies pages.

## Structure

```
skills/
  notion-cleanup-priority/
    SKILL.md
  notion-page-builder/
    SKILL.md
```

Each skill is one folder with one `SKILL.md` file. The file holds the full instructions.

## How to try a skill

- **Claude.ai:** ZIP one skill folder and upload it as a skill in Claude settings.
- **Claude Code:** copy the skill folder into `.claude/skills/` in your project.

## Notes

- The pilot thresholds and dates are assumptions for the case study.
- `notion-cleanup-priority` lists some mapping rules as "not yet confirmed". This is on purpose: the skill reports them back for a person to confirm.
