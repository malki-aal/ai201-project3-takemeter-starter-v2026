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

TakeMeter reads comments from r/soccer, Reddit's general football community, and
sorts each one by how it argues. `analysis` backs a claim with something you
could check, such as a stat, a fee, a date or a named match. `hot_take` states a
confident opinion with nothing checkable behind it. `reaction` is aimed at another
commenter rather than at the football. The model is a DistilBERT fine-tuned on 200
comments I labelled myself. It's meant to separate the comments that bring
evidence from the ones that only bring confidence or an argument with another user.


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
pre-labelled by an AI tool ([NAME THE TOOL YOU USED]) given my definitions; I then read and corrected each
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

**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs, learning rate 2e-5, batch size 16, max length 128 tokens, seed 42.

**Device:** `cpu` (PyTorch 2.14.1+cpu, Windows 11). The laptop has an NVIDIA RTX 3050, but the default `pip install` gave a CPU-only PyTorch build, so training ran on the CPU. This is recorded in `results.json`.

**Anything I changed from the defaults, and why:** Nothing. All settings are the notebook defaults.

**Split sizes:** train 139 · val 31 · test 30

| Label | train | val | test |
|---|---|---|---|
| `analysis` | 41 | 9 | 9 |
| `hot_take` | 72 | 16 | 15 |
| `reaction` | 26 | 6 | 6 |

`reaction` has only 6 posts in the test split, under the ~8 the notebook warns about. One post more or less right moves its F1 a lot between seeds.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1: stress-testing the labels (Milestone 2)**

- *What I asked for:* I gave Claude (Claude Code, in VS Code) my three draft
  definitions and asked for eight posts written to sit on the boundaries between
  them.
- *What came back:* Eight r/soccer-style posts. Five of them couldn't be
  classified cleanly. For example, "Arteta's side can't win big games because
  there's no plan B" gives a reason, so it didn't fit `hot_take` = "no support
  offered". "You clearly didn't watch the match. Rice was everywhere" made
  "mainly aimed at a commenter" a judgement call.
- *What I changed:* I rewrote all three definitions around one test, whether
  the post has a checkable specific. `hot_take` became "no checkable specific,
  even if it gives reasons", and `reaction` became "aimed at a commenter or the
  thread, with no checkable specific". I also wrote the decision rule in the
  order fact → target → otherwise. The record is at the end of `reading_notes.md`.

**Moment 2: checking my labels against the rule (Milestone 4)**

- *What I asked for:* I asked Claude to check `labels.csv` against the new
  decision rule.
- *What came back:* A list of every non-`analysis` post containing a number or
  link, and every `analysis` post without one, with a call on each. Three
  pre-labelled posts broke the rule. One `hot_take` ended by calling the other
  commenter "purposely dense", so it was really a `reaction`. One `hot_take`
  named a checkable incident (CR7 leaving for Madrid). One `reaction` rested on
  a checkable fact (the highest PL wage bill).
- *What I changed:* I relabelled those three (marked `pre-labelled, corrected`)
  and used two of them as hard cases. I left the cold labels alone, because
  they're the honesty check for the agreement report.

**Other AI help, disclosed:** Claude also drafted `criteria.md`, wrote the
README write-ups (Label Taxonomy, The Dataset, The Training Run, this section),
set up the `.venv` and ran notebook sections 1–5. I read and kept what it wrote.

**Pre-labelling disclosure:** 32 posts were labelled cold by me with no tools
(`cold` in the note column). The other 168 were pre-labelled by
**[NAME THE TOOL YOU USED]** from my definitions, then read and corrected by me
(`pre-labelled`). Three of those were corrected again after the Milestone 2
rule was written (`pre-labelled, corrected`).

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.300 | 0.500 | +0.200 |
| Macro F1 | 0.297 | 0.222 | -0.075 |
| F1 — `analysis` | 0.300 | 0.000 | -0.300 |
| F1 — `hot_take` | 0.211 | 0.667 | +0.456 |
| F1 — `reaction` | 0.381 | 0.000 | -0.381 |

Baseline is `facebook/bart-large-mnli` zero-shot, given my definitions from
`label_definitions.txt`, scored on the same 30 held-out posts in
`test_split.csv` (9 analysis, 15 hot_take, 6 reaction) that the trained model
was scored on (`baseline_results.json`).

**What my definitions were worth:** I ran the baseline a second time with no
definitions file, so it saw only the bare label names
(`baseline_results_bare_names.json`). It did *better*: accuracy 0.533 vs.
0.300, macro F1 0.398 vs. 0.297 (bare names scored analysis 0.000, hot_take
0.650, reaction 0.545). So as zero-shot hypotheses my definitions cost 23
points of accuracy instead of adding any. My guess is that long hypotheses
like "This post is a claim about football backed by at least one checkable
specific…" are hard for an NLI model to score, because they are rules about
what a post contains, not descriptions of what it says. The table above
reports the definitions run, as the milestone asks.

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
I didn't write a prediction down before the trained numbers were in front of
me, because `results.json` came out of the previous unit. So what follows is a
reconstruction, not a real prediction. Going by the label design, I would have
expected the trained model to beat the baseline most clearly on `analysis`. Its
rule is about the post's surface (a stat, score, fee, date or link), which is
something fine-tuning on 200 examples can pick up and a zero-shot model reading
a definition can't. In fact the trained model scored 0.000 on `analysis` and
beat the baseline only on `hot_take`, the majority class.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->
The trained model's 0.500 accuracy is not a real win. It predicts `hot_take`
for all 30 posts, so 0.500 is just the share of hot_take posts in the test
set. Its macro F1 (0.222) is lower than the baseline's (0.297), and it never
gets one analysis or reaction post right. The baseline at least tries to
separate the three labels. As trained, my fine-tuning has learned only the
majority class.



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.65 | 0.500 | 0.500 | 0.500 | MISSED |
| 2. Macro F1 | ≥ 0.55 | 0.222 | 0.222 | 0.222 | MISSED |
| 3. Per-label F1 floors (`hot_take`/`analysis`/`reaction`) | ≥ 0.65 / 0.55 / 0.40 | 0.667 / 0.000 / 0.000 | 0.667 / 0.000 / 0.000 | 0.667 / 0.000 / 0.000 | MISSED (`hot_take` passes, the other two score 0) |
| 4. Agreement with staff labels | ≥ 24/30 | 30/30 | 30/30 | 30/30 | MET (the 30 labels were made by Claude, not me; see Agreement Report) |
| 5. Most-confident third minus least-confident third, accuracy | ≥ +0.20 | 0.70 − 0.20 = +0.50 | 0.70 − 0.40 = +0.30 | 0.50 − 0.10 = +0.40 | MET as written; criterion revised as broken (see `criteria.md`) |

**Device:** CPU (`torch 2.14.1+cpu`), `distilbert-base-uncased`, 3 epochs,
lr 2e-5, batch 16, max length 128. About 60 seconds of training per seed.
Every seed's test split held 30 posts: 9 analysis, 15 hot_take, 6 reaction.
Raw numbers are in `results_three_seeds_before.json`.

The notebook's spread table, as printed:

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.500 | 0.500 | 0.500 | 0.000 |
| Macro F1 | 0.222 | 0.222 | 0.222 | 0.000 |
| F1 — `analysis` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `hot_take` | 0.667 | 0.667 | 0.667 | 0.000 |
| F1 — `reaction` | 0.000 | 0.000 | 0.000 | 0.000 |

**About the spread:** the notebook calls a spread of 0.000 "stable enough to
trust", but that's misleading here. Every seed gives the same numbers because
every seed learned the same thing: always guess `hot_take`. A model that
ignores its input can't be affected by which posts land in the test split.
Stratified splitting keeps the label counts at 15/9/6 every time, so the score
is the same too. Zero spread here means the model collapsed, not that it's
reliable. The training logs show why. Validation loss was still falling at
epoch 3 on every seed (seed 42: 1.014 → 0.986 → 0.976), while validation
accuracy stayed at the majority-class rate (0.516) after every epoch. With
about 140 training posts, batch 16 and 3 epochs, that's only 27 gradient
steps at lr 2e-5. That isn't enough to move it off the majority-class guess,
so it's underfit, not unstable.

**About criterion 5:** it passes on every seed, but every prediction is
`hot_take`, so the confidence score is only saying which posts look *more*
`hot_take`-like. The "confident third" scores higher just because more of it
is real `hot_take`. Mean confidence was about 0.50 whether the model was
right or wrong (seed 42: 0.515 vs. 0.488). I'm counting it as met on paper,
not as evidence that the model's confidence means anything.

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

Seed 42 (seeds 7 and 2024 are identical):

| true \ predicted | analysis | hot_take | reaction |
|---|---|---|---|
| **analysis** | 0 | 9 | 0 |
| **hot_take** | 0 | 15 | 0 |
| **reaction** | 0 | 6 | 0 |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->
All 9 real `analysis` posts were called `hot_take` and none went the other
way. So the model never learned my main boundary, the checkable specific that
separates `analysis` from `hot_take`. It treats every confident football
opinion as a hot take, whether or not it has a stat, score, fee or link. The
same thing happens to all 6 `reaction` posts, so this isn't one hard boundary.
The model has learned no boundaries at all and just predicts the majority
label.


---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy, every seed | ≥ 0.65 | **MISSED** | 0.500 on all three seeds, 15 points short each time |
| 2 | Macro F1, every seed | ≥ 0.55 | **MISSED** | 0.222 on all three seeds |
| 3 | Per-label F1 floors, every seed | `hot_take` ≥ 0.65, `analysis` ≥ 0.55, `reaction` ≥ 0.40 | **MISSED** | `hot_take` clears it (0.667), but `analysis` and `reaction` are 0.000 on every seed. All three floors had to hold, so it's a miss |
| 4 | Agreement with staff labels | ≥ 24/30 | **MET** | 30/30 from `agreement.py`. Caveat: the 30 labels were made by Claude, not me (see Agreement Report) |
| 5 | Confident third ≥ 0.20 more accurate than unsure third, every seed | ≥ +0.20 | **MET as written**, but the criterion is broken (revised in `criteria.md`) | +0.50, +0.30, +0.40. But it passed for a model that predicts one label for everything, so it can't show that confidence means anything |

**Diagnoses**

Every miss (1, 2 and 3) has the same root: on every seed the model predicts
`hot_take` for all 30 posts. So the question is *why* it collapsed. Of the
four common causes, the evidence points mainly to **training, with label
inconsistency on `analysis` vs. `hot_take` as a second cause and too few
`reaction` examples as a third**.

**1. Mainly a model/training problem: it never left the majority guess.**
- *What I saw:* validation accuracy was 0.516 (the `hot_take` share of the
  validation set) after epoch 1, 2 and 3 on all three seeds. Validation loss
  was still falling at epoch 3 (seed 42: 1.014 → 0.986 → 0.976). A model that
  had finished learning would have stopped improving. This one was still
  moving and hadn't yet crossed the point where any non-`hot_take` guess wins.
- *Why:* about 140 training posts, batch 16 and 3 epochs is only 27 gradient
  steps at lr 2e-5. That's very little training for a 66M-parameter model.
- *How I know the labels aren't the main problem:* a two-line keyword rule I
  wrote (a digit, link or currency sign → `analysis`; "you", "people" or
  "lol" → `reaction`; otherwise `hot_take`) scores **macro F1 0.414 on the
  same 30 test posts** (0.581 across all 200), against the model's 0.222 and
  the zero-shot baseline's 0.297. If the labels were noise, a crude rule
  couldn't beat the model on them. The signal is in the data, and the
  fine-tuned model didn't train long enough to find it.
- *So it isn't "a task the model can't reach from 200 examples".* That
  can't be decided until the model has actually been trained enough to try.
  This is the thing to fix in Milestone 5.

**2. Second cause: I applied the `analysis`/`hot_take` boundary
inconsistently.**
- *My agreement report can't decide this one.* It came back 30/30, but
  Claude labelled those 30 and the staff rules are tighter than mine, so it
  says nothing about how consistently my own 200 were labelled. I had to
  check `labels.csv` directly instead.
- *What I found:* 13 of my 103 `hot_take` posts contain a number, and 25 of
  my 59 `analysis` posts contain none. Some of those are fine under my
  definition (a "named match or incident" counts as a specific with no
  digits). But some pairs are labelled opposite ways for the same kind of
  evidence:
  - *"Pretty sure Arsenal has the second highest wage bill in the league"* →
    `analysis`, yet *"People have complained about Chelsea for 20 years…"* →
    `hot_take`. Both give one vague, checkable-in-principle fact.
  - *"The fact Pep had to buy Guehi and Semenyo to keep up is embarrassing"*
    → `analysis` because it names signings. Under the staff set's rule 1,
    the names are decoration on a conclusion already reached, and my own
    definition has no rule for decoration at all.
- *Why it happened:* 165 of my 200 labels were AI-pre-labelled and only 3
  were changed when I reviewed them. My definition says *any* checkable
  specific makes a post `analysis`, but in football almost every post names
  a player or a club, so "specific" was stretched case by case.
- *What it costs:* this makes `analysis` harder to learn, but it doesn't
  explain an F1 of exactly 0. That's cause 1. Once the model trains properly,
  I expect this to be the boundary where errors pile up.

**3. Third cause: too few `reaction` examples.**
- `reaction` has 38 posts, which leaves about 27 for training and 6 per test
  split, so one post moves its F1 by roughly 0.15.
- Even the keyword rule only reaches 0.143 on `reaction` in the test split.
  That suggests the "aimed at another commenter" cue is genuinely harder than
  spotting a number, as I predicted in criterion 3. I can't separate "too
  few" from "genuinely hard" until the model is past cause 1.


---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Who labelled the 30:** With my mentor's OK, the 30 labels in
`my_staff_labels.csv` were made by Claude (an AI assistant), applying
`data/staff_taxonomy.md` as written. They were committed before
`data/staff_labels.csv` was opened. So this rate measures how an AI applies
the staff rules, not how I do, and it's weaker evidence about the
consistency of my own 200 labels.

Labelling notes, for the borderline posts:
- `sp_005`, `sp_007`: both open with a general claim ("Carbon plates aren't
  the whole story", "it's more than that"). Labelled `analysis` because the
  numbers (2% + 2% with the mileage change; 90 seconds in 4C vs. 24C) are
  the reason for the claim, not decoration (rule 1).
- `sp_002`, `sp_010`: one-runner or three-runner data. Labelled `analysis`
  because a small sample is still a checkable fact that carries the argument.
- `sp_014`: `hot_take`. It's almost exactly rule 1's own example: the 3% is a
  flourish on "obviously the shoes are the whole story".
- `sp_023`, `sp_027`: contain numbers (eight weeks, two miles, 20 degrees)
  but argue nothing; they report what just happened and how it felt, so
  `reaction` (rules 2 and 3).

**Agreement rate:** 30 / 30 = 100%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

None. `agreement.py` found 0 disagreements across the 30 posts
(`agreement_results.json`), so there's nothing to sort into piles (a), (b)
or (c).

**Were the hard posts actually hard?** Staff flagged 7 of the 30 as
borderline. Here is how each one was decided, and whether I'd flagged it
beforehand in the labelling notes above:

| Post | Label | The rule that decided it | Flagged before seeing the key? |
|---|---|---|---|
| `sp_005` | analysis | Rule 1: the opening clause sounds like a hot take, but the 2% + 2% with only mileage changing is the argument | Yes |
| `sp_010` | analysis | Rule 1: hedged and a tiny sample, but the observation is checkable and the claim rests on it | Yes |
| `sp_014` | hot_take | Rule 1: the 3% is decoration; "so obviously" does the work | Yes |
| `sp_019` | hot_take | Rule 2: reflective tone, but it makes a general claim about everyone, not a report of the writer's day | No |
| `sp_020` | hot_take | Rule 1: sounds observational, but no count is given | No |
| `sp_028` | reaction | Rule 3: "third this year" is countable, but nothing is argued from it | No |
| `sp_029` | reaction | Rule 2: a shoe observation, but it reports the writer's own realisation, now | No |

**What the pattern in my disagreements tells me:**

There's no pattern to read, because there were no disagreements. I don't
think 100% means the rules are airtight, for three reasons:

1. **The set is cleanly blocked.** Posts 1–10 are all `analysis`, 11–20 all
   `hot_take` and 21–30 all `reaction`, so 23 of the 30 aren't close calls.
   The real test was the 7 flagged posts. All 7 matched, but 4 of them
   (`sp_019`, `sp_020`, `sp_028`, `sp_029`) weren't flagged as borderline
   when labelling. They were decided by the rules without ever feeling
   difficult.
2. **The staff rules are tighter than mine.** Each boundary has a written
   test with a worked example (rule 1's 3% example is almost `sp_014` word for
   word). My own taxonomy has one test, "is there a checkable specific", and
   no rule for when a fact is decoration. The staff set has
   decoration-vs-load-bearing as rule 1. Under my definitions, `sp_014` and
   `sp_020`-style posts would be much harder to call.
3. **The labeller wasn't me.** As noted above, Claude made these 30 labels.
   So 100% shows that the staff rules can be applied consistently as
   written. It isn't direct evidence that I applied *my* rules consistently
   across my 200. For Milestone 4, the honest reading is that this check
   doesn't rule out inconsistency in `labels.csv`. Most of those 200 were
   AI-pre-labelled and then corrected by hand, and the model collapsing to
   `hot_take` means the labels need checking directly.


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
