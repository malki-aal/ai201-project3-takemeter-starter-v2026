# Reading notes: r/soccer

**Community:** r/soccer (public subreddit, all clubs and leagues, not tied to one
show or release). Comments pulled from 155 threads across eight dates, Nov 2025 to
Aug 2026, via the public Arctic Shift Reddit archive: 488 unique comments of 12+
words, from which 40 were read for this step (the #numbers below are positions in
that 40) and 200
were labelled into `labels.csv`.

## Candidate distinctions (things you can point at)

1. **Some comments name a checkable specific. Most don't.**
   A stat, a date, a transfer, a named incident, a link.
   - Has it: #23 (Messi's World Cup games/goals/assists), #28 (City signed
     Marmoush, Semenyo, Donnarumma "5-year deal for 80m"), #9 ("Gvardiol injured
     and the chequebook comes out"), #3 and #38 (links to the clip).
   - Doesn't: #1 ("Kompany >>>>> Arteta"), #11 ("the product is poor"), #13
     ("Argentina has always been a world beater team").

2. **Some comments give a reason ("because", "if… then", concedes a point). Others
   just assert.**
   - Reasons: #30 (league got more competitive → lower point tallies → Liverpool
     deserve credit), #39 (separates "same situation" from "same perspective"),
     #10 ("correct me if I'm wrong… his tactics have been figured out").
   - Asserts: #1, #18, #35, #22's first line.

3. **Some comments are aimed at another user. Others are about football.**
   "You", "you lot", "keep copin'", "true hypocrite".
   - Aimed at a person: #5, #15, #34, #40, #13's ending, #26.
   - About football: #27, #30, #17.
   - Hard case: #17 and #29 rebut a user *with* evidence. That's the boundary
     between distinctions 1 and 3.

4. **Some comments aren't really a take on football at all.**
   - #7 (a daily guessing game's share text), #24, #25, #31 (drug use), #16, #19.
   These need either their own label or to be dropped from the 200.

## Possible labels these suggest

- `analysis`: a claim backed by a checkable specific (1). Reasoning alone
  (distinction 2) was too fuzzy to apply consistently, so a well-argued comment
  with no checkable fact is `hot_take`.
- `hot_take`: a confident claim about football with nothing behind it
- `reaction` (first called `jab`): mainly aimed at another commenter, not at the football (3)

Hardest boundary: `analysis` vs `reaction` when a reply attacks someone *and* cites
facts (#17, #29, #40). One possible rule: if the comment names a checkable
specific about football, it's `analysis`, whatever the tone.

Off-topic comments (4) are a decision still to make: drop them from the 200, or add
an `other` label.
