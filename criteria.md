# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. Overall accuracy is at least 0.65 on the held-out set, on every one of the three seeds

**Why this target:** `hot_take` is 103 of my 200 posts, so a model that always
guesses `hot_take` already scores about 0.52. That's the real floor, not the 0.33
coin flip. The test split is about 30 posts, so 0.65 means roughly 20 right
against the majority guesser's 16. Four extra posts per seed is a modest but
real margin, and I'd call it useful. Anything higher would be optimistic for
200 training examples when it has to hold on all three seeds.

---

## 2. Macro F1 is at least 0.55 on the held-out set, on every one of the three seeds

**Why this target:** Accuracy alone can be gamed by leaning on `hot_take`. An
always-`hot_take` model scores about 0.23 macro F1, because it gets 0 on the
other two labels. Macro F1 weights my three labels equally, so it only passes if
`analysis` and `reaction` are being found too. I set it 0.10 below the accuracy
target because the small labels will drag the average down.

---

## 3. Every label clears its own F1 floor on every seed: `hot_take` ≥ 0.65, `analysis` ≥ 0.55, `reaction` ≥ 0.40

**Why this target:** The floors follow how much data each label has and how
easy its signal is. `hot_take` has 103 examples (~16 in each test split).
`analysis` has 59 (~9 per split) and usually carries a visible cue: a number, a
fee, a link. `reaction` has only 38 examples, so about 6 land in each test
split and one wrong post moves its F1 by 0.10 or more. Its boundary also depends on
spotting a "you" or "this thread" target *and* the absence of a fact, which is
harder for a model than spotting a number. Below 0.40, `reaction` predictions
would be wrong more often than right, and the label wouldn't be worth keeping.

---

## 4. I agree with the staff labels on at least 80% of the 30 staff posts (24 of 30), measured by `agreement.py`

**Why this target:** The model can't be more consistent than the labels it learns
from, and this is my only outside check on my own labelling. The staff taxonomy
comes with written decision rules, so a careful labeller should match on most
posts. 80% allows about 6 disagreements, all on genuine boundary cases. There's
a specific trap for me: staff `reaction` means "an immediate feeling about
something that just happened". My `reaction` means "aimed at another commenter".
If I slip into my own definition, I'll lose points on exactly those posts, and
going below 80% would mean I'm not applying written rules as written.

---

## 5. The model's most-confident third of test predictions is at least 0.20 more accurate than its least-confident third, on every seed

**Why this target:** If confidence means anything, I should be able to trust the
sure predictions more and send the unsure ones to a human. Each third of a
30-post test set is about 10 posts, so 0.20 means at least 2 more right in the
confident third. Anything smaller could come from one lucky post. I expect
confident predictions to be the `analysis` posts with obvious numbers in them.
I expect unsure ones to be on the `hot_take`/`reaction` boundary, where a
"you" might or might not make the post about the person.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
