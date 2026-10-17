# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->



---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `analysis`

**Definition:** A claim about football backed by at least one checkable specific (a stat, score, fee, date, named match or incident, or a link), whatever the tone.

**Example 1:**
> "Barcelona created 154 BCC in 38 games and conceded 97. That's an average of 4.05 BCC created per 90, while conceding 2.55 BCC per 90."

**Example 2:**
> I really don't think going from a 6 point lead to a 2 point lead is particularly crazy. Especially when Arsenal just had two away games against top 5 teams in the league

### `hot_take`

**Definition:** A confident opinion about football with no checkable specific behind it, even if it gives reasons.

**Example 1:**
> He's good in a one off game / cup competition but left a lot to be desired in the league.

**Example 2:**
> They are a small club to be fair. Next to no pressure from the media

### `reaction`

**Definition:** A reply aimed at another commenter or the thread (addressing, mocking or arguing with "you" or "people") that offers no checkable specific.

**Example 1:**
> Insulting me won't change the fact that you foul on corners and refs dont have the balls to do something.

**Example 2:**
> If you honestly believe it was handled well and they extended with 'plenty of time' then all the power to you. If my three best players all left it until the summer to sign a new deal and one of them left I would be livid.

### The hardest boundary

**Which two labels:** `analysis` vs `reaction`: a reply that attacks another commenter *and* cites a fact.

> 150m for the lowest team in la liga that is alot.. 99.9% of leagues and teams would kill for this revenue per season. you have no concept of money in football

**The decision rule I used every time:** If the post names a checkable specific about football, it's `analysis`, even if it insults or addresses another commenter (the post above is `analysis` because of the 150m figure). Only with no checkable specific do I choose between the other two: aimed at a commenter or the thread → `reaction`; otherwise → `hot_take`. Reasons, vague appeals ("all season", "every time", "just watch him") and an "I think" don't make a checkable specific. A specific counts whether or not it's correct and whether or not it really proves the claim.

Posts that aren't a take on football at all (game share text, off-topic chat, pure emotion like "crying at this final") were left out of the 200 rather than given a fourth label.



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Public comments from r/soccer, pulled from 155
threads across eight dates (Nov 2025 to Aug 2026) through the public Arctic Shift
Reddit archive. Only comments of 12+ words were kept, which left 488 unique
comments. 200 of them are in `labels.csv`, whole and untrimmed. Off-topic
comments that aren't a take on football (game share text, off-topic chat, pure
emotion) were left out rather than given a fourth label.

**How I labelled them:** I labelled 32 posts cold first, with no tools and only
my definitions, and marked them `cold` in the note column. The other 168 were
pre-labelled by an AI tool given my definitions; I then read and corrected each
one, and they're marked `pre-labelled`. After the Milestone 2 decision rule was
written, I re-checked every non-`analysis` post that contained a number or
link, and every `analysis` post that didn't, against the rule. Three pre-labelled
posts changed and are marked `pre-labelled, corrected`. No cold labels were
changed.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `hot_take` | 103 | 51.5% |
| `analysis` | 59 | 29.5% |
| `reaction` | 38 | 19.0% |
| **Total** | 200 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "Oh no, a manager reform the club's squad to fit his style of play. Who had the highest wage bill last season in the PL btw? Inb4 it's because of "bonuses", the excuse that was never brought up when it was City topping it."
>
> *Could have been:* `reaction` or `analysis`
>
> *I chose, because:* `analysis`. It's sarcastic and aimed at the other commenter, so it was first labelled `reaction`. But the whole point rests on a checkable fact (which club had the highest PL wage bill last season), and my rule says a checkable specific wins whatever the tone.

**2.**
> *The post:* "I used to grapple in Sunday league as a youth player. It's a contact sport. We all know this. There's a difference between the regular physicality of the sport and teams adopting a specific approach to crowd and target GKs and also wrestle to this level in the 6 yard box. If you don't know the difference you're being purposely dense."
>
> *Could have been:* `hot_take` or `reaction`
>
> *I chose, because:* `reaction`. Most of the post argues about football, which is why it was first labelled `hot_take`. But it has no checkable fact (personal experience doesn't count), and it ends by calling the other commenter "purposely dense". With no fact, a post aimed at a commenter is `reaction`.

**3.**
> *The post:* "Let me clarify what I mean by quality. If this Arsenal team was up against City in 18/19 for example they are getting blown out of the water. Top to bottom the league has higher quality but up at the top the teams are the worst they've been in years. [...]"
>
> *Could have been:* `analysis` or `hot_take`
>
> *I chose, because:* `hot_take`. "City in 18/19" sounds like evidence, but it names a team and not a fact. "They'd get blown out" is a hypothetical nobody can check. A named season or team only counts as a checkable specific when the post states something about it that could be looked up.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**

**Settings:** <!-- epochs, learning rate, batch size, seed -->

**Anything I changed from the defaults, and why:**

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Moment 2**

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Pre-labelling disclosure:**

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
