---
name: "notion-cleanup-priority"
description: "Pre-set Priority (Reach x Risk) and Suggested action tags on new or unscored Notion pages, using the Apify clean-up pilot rules. Draft tags only; a person confirms."
---

# Notion clean-up: Priority and Suggested action

Use this skill when new pages appear in the inventory or the "Welcome to Apify" wiki, or when a full Notion export arrives. The skill sets a draft Priority and draft Suggested action tags. A person confirms them.

Source of the rules: Diagnostics summary, Decision log and Notion clean-up pilot pages (Notion wellness project). If those pages change, update this skill.

## Write style
- ASD-STE100: short sentences, active voice, one idea per sentence.
- No em dashes.
- Tag your own claims [Certain], [Probable] or [Conjecture] when you report back.

## Where to write
| Database | Priority property | Tags property | Notes property |
|---|---|---|---|
| notion_page_inventory.csv | Priority | Suggested action | Comments |
| Welcome to Apify (wiki) | Cleanup: priority | Suggested action | none (report in chat) |

Priority options: High, Can wait, Low.
Tag options: Archive review, Possible duplicate, Merge then archive, Update content, Assign owner, Move to general space, Add text summary for agent, No action, Apply page template.

## Step 1: Collect signals per page
Read metadata first. State which source you used: metadata or page content.
- **Reach:** views in the last 90 days, backlinks.
- **Duplicate:** same or near-same topic in the title as another page. Includes OLD, NEW, DRAFT, copy, v1, v2, WIP, a year in the title.
- **Owner left:** creator or last editor marked "(left YYYY)" or not a current workspace member.
- **Personal space:** path starts with "Personal /".
- **Very short:** two lines of text or less (about 30 words or less), or mostly images or a link to a sheet.
- **Very long:** more than 2,000 words.
- **Old edit:** no edit for 18+ months.
- **Working doc:** plan, retro, 1:1, meeting notes, brainstorm, proposal.
- **Hub page:** a landing or section page that mainly links to other pages.

## Step 2: Score Reach
- High: 200+ views in 90 days, or 10+ backlinks.
- Medium: 50 to 199 views.
- Low: under 50 views.

## Step 3: Score Risk
- Strong: part of a duplicate set, last editor left, policy or reference page in a personal space, very short, very long.
- Weak: old edit (18+ months) only, or only the creator left.

Very short and very long are strong risk, not a direct High. A page is High only when it also has high reach.

## Step 4: Set Priority
- High = High reach + strong risk.
- Can wait = High reach + weak risk only, OR Medium or Low reach + strong risk.
- Low = everything else.

Special rules:
- **Duplicate sets share one reach score:** use the highest reach in the set for every page in it. Why: a stale copy hurts the readers of the official page too.
- **Working docs are always Low** with Archive review, even when very long. Why: they are noise to clean, not pages to fix.

The user chose to keep these thresholds for the pilot. Review them after pilot week 6.

## Step 5: Set Suggested action tags
A page can have more than one tag. Add a short Comment that gives the reason.

| Signal | Tag | Comment pattern |
|---|---|---|
| Working doc | Archive review | Working doc, not reference; archive review (keep only if a live decision is inside) |
| Empty page | Archive review | Empty page; archive |
| Overlaps another page on the same topic | Possible duplicate | Overlaps with other pages on same topic; decide one canonical page |
| Probable official page in a duplicate set | Possible duplicate | Probable canonical page in a duplicate set; confirm, then point other versions here |
| Outdated version (OLD, superseded) | Possible duplicate | Outdated version; archive after backlinks point to the current page |
| Unofficial draft or copy | Merge then archive | Unofficial version (draft or copy); merge into canonical page, then archive |
| Reference page in a personal space, no official copy | Move to general space | Reference content in a personal space; move to General or merge |
| Creator and last editor left | Assign owner | Creator and last editor left; assign a new owner |
| Creator left, last editor still here | Assign owner | Creator left; last editor <name> is a likely owner |
| Old edit (18+ months) | Update content | Not edited since <YYYY-MM>; owner to confirm content is valid (date may be unreliable) |
| Very short | Add text summary for agent | Little or no text; agents cannot read it; add a short text summary |
| Very long | Update content | Very long (over 2,000 words); owner to check if it can be shorter or split |
| Hub page, no issue | No action | Hub page; no issue in metadata; dates likely reset by a restructure, do not use them for staleness |
| No issue | No action | No issue in metadata; check content |

Mapping rules NOT yet confirmed by the user (apply them, but list them in your report):
- Working docs where the last editor left: Archive review only, no Assign owner.
- Unofficial copies in a personal space: Merge then archive only, no Move to general space.
- Empty page: Archive review.
- Outdated version and probable canonical page: both Possible duplicate.
- Very long page: Update content.

Do NOT assign "Apply page template" from metadata. It needs a content review against the page standard. Suggest it only after you read the page content, and only as a question to the user.

## Data warnings (always check)
- Moving or duplicating a page changes its creator and dates. Dates are one signal only. Never use a date as the single reason for Archive review.
- Flag these and add "data check" to the Comment: edit date before created date; page created or edited after the person left; a plan or retro created before the period in its title; many hub pages with the same recent created date.
- A sample is not the workspace. Never present counts from a slice as workspace numbers.

## What this skill does NOT do
- It does not archive, move, delete, verify or unverify pages. Owners decide. People archives approved pages.
- It does not change Owner. Owners are assigned by the project owner of each team.
- It does not overwrite a Priority or tag that a person already set. Fill empty values only, unless the user asks.
- Note: verifying or unverifying a wiki page resets Owner to the person who runs the action. If a later step changes verification, set Owner again afterwards.

## Report back to the user
- Counts per Priority and per tag for the pages you scored (label as the scored set, not the workspace).
- The pages where a data warning applied.
- The unconfirmed mapping rules you used, so the user can confirm or change them.
- One question or one next step.