# Card Sort Tool

A moderated card-sorting tool for working out how customers actually group the doTERRA storefront's products and content. Built for in-person research sessions: a moderator and one participant pass a single device back and forth while the participant sorts a deck of cards into groups.

It is the generative half of a pair. The [Tree Testing Tool](../Tree%20Testing%20Tool/) validates a proposed navigation structure; this one tells you whether that structure matches how people group things in the first place.

## Hosted version

The live, working copy of this tool is a published Claude Artifact:

**[Open the Card Sort Tool](https://claude.ai/artifact/Pokm3iGD1Zc9UGQXfBCscq)**

Private to the owner's account until shared from the page's share menu.

That is the version to run sessions with. It has a real shared database behind it, so:

- Every sort saves to a shared results ledger as the session runs, not just at the end.
- The ledger, the deck, and the categories all sync live across every device that opens the link.
- Results export as CSV, a co-occurrence matrix, JSON, or Markdown from the Ledger tab.

## This repo

`grouping-study.html` is the same tool as a single, self-contained HTML file: the source of truth for the code, and a way to run or inspect the tool outside of claude.ai.

Opened directly it still runs the full moderator/participant flow, but without the Artifact's shared database:

- Results save to that browser's own local storage instead of a shared ledger. Not synced across devices, and cleared if browsing data is cleared.
- Export works the same either way, and is the reliable way to get a copy out.

### Running locally

No build step or dependencies, just one HTML file with inline CSS and JS.

```bash
open grouping-study.html
```

Or serve it, which some browser features need:

```bash
python3 -m http.server 8080
```

Then visit `http://localhost:8080/grouping-study.html`.

## How the tool works

1. **Setup**. The moderator names a participant, optionally flagging a practice run or an internal (doTERRA staff) participant. Use the same participant ID as the Tree Testing Tool so the two exports can be joined later.
2. **Sort**. The participant reads a short prompt, then puts each card into a group. Cards can be tapped (tap a card, tap a group) or dragged. They can add their own groups, move anything at any time, and park anything that fits nowhere in the **Other / doesn't fit** bay with a line on why.
3. **Name**. Once they are done sorting they name any group they created themselves, after the fact rather than up front so they are not committed to a label early.
4. **Capture**. The moderator sees what was built, records confidence (1 to 5) and notes, and can retake the sort or end the session early.
5. **Done**. Results save to the ledger and the moderator starts the next participant.

An interrupted session stays in the ledger as in-progress and can be resumed or discarded from the Setup screen.

## Sort modes

Set per sort in the Content tab.

| Mode | What the participant sees | Use when |
|---|---|---|
| **Hybrid** (default) | Your categories, and they can add their own | You have a candidate structure and want to know both how well it holds and what it is missing |
| **Closed** | Your categories only | You want to validate a structure you have already committed to |
| **Open** | No categories, they build every group | You want the richest picture of how people group things, and have the participants to support it |

The **Other / doesn't fit** bay is available in every mode and is worth keeping on. At a small sample, a card parked there with a consistent reason is often the most actionable finding in the study.

## The categories are derived from the tree test IA

The default categories are not hand-written. They are exactly what the Content tab's **Derive categories from a tree test IA** box produces from the Tree Testing Tool's current navigation structure: its filing level, the branches its destination pages actually sit under, with each sub-branch qualified by its parent unless it already carries the parent's name.

That is what makes the two studies comparable. When the proposed IA changes, paste the updated structure into that box and re-derive rather than editing the category list by hand, so the two tools stay in step.

The default deck is 30 destination pages from the same structure, deduplicated and trimmed. It is a representative sample, not the catalogue.

## Reading the results

The Ledger tab shows, per sort:

- **Card agreement**, weakest first. What share of participants put each card in the same group, banded Strong / Mixed / Contested against thresholds you set in the Content tab.
- **Cards nobody could place**: everything parked in Other, with the reasons given.
- **Groups participants added**: what they invented when your categories did not fit. This is where the gaps in the proposed structure show.
- **Cards that travel together**: how often two cards ended up in the same group, out of the participants who placed both. The full matrix is in the export.

### What the numbers are worth

At 6 to 10 participants, treat everything here as directional. The commonly cited baseline for card-sort clusters settling down is around 15 participants, and below that a co-occurrence matrix is noisy enough to mislead if read as a figure rather than a pattern. The tool says so on the Ledger whenever the sample is under 15.

In a moderated session the thinking aloud is the primary data. The percentages point you at which cards to go back and read the notes about.

Two further caveats worth holding on to:

- **Hybrid inflates agreement.** Categories on screen anchor people, so scores read higher than the same cards sorted openly would.
- **Priming between the two studies is real.** Someone who card sorts first has seen the categories. Run the two on different participants where you can, and where you cannot, run the tree test first.

## What this pair does not cover

Tree testing strips the visual design on purpose and card sorting strips context on purpose. Neither will tell you whether a menu is scannable, whether labels work in situ, or whether search absorbs the navigation load. A first-click test on a real design is the natural third surface.

## Project context

This tool lives under the broader **Storefront** project folder, which covers other WQA/doTERRA storefront UX work.
