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

TakeMeter sorts posts from **Fitness Stack Exchange**, the public Q&A site
for lifting, running and nutrition, by what they offer to back up what they
say. A post is `evidence` if it gives a fact you could check (a study, a
number, a named rule), `personal` if the support is the poster's own
training, and `claim` if it's only confident say-so. Fitness advice is
everywhere and most of it is unsourced. A model like this could flag which
answers are worth trusting and which you should check before you change your
training. It's a fine-tuned DistilBERT trained on 219 posts I labelled.


---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

Three labels, about *what a post offers to back up what it says*. That's the
thing Fitness SE itself cares about. One comment I read just says "Please
consider editing your answer to include the reference."

### `evidence`

**Definition:** The post backs its point with a specific fact a reader could
check: a study, a number, a date, a named rule or a named source.

**Example 1:**
> According to this study: Long-Term Effects of HIIT on Cardiac Function and Mortality in Heart Failure, (emphasize mine): Long-term HIIT is highly effective in improving cardiac performance, decreasing the severity of HF (heart failure) symptoms, and improving the quality of life among patients. [...] Early HIIT, tailored to the individual needs of each patient, has the potential to improve clinical outcomes and increase survival rates in heart failure populations. Since you're mostly concern with the heart rate, there you go. Yes, it is even beneficial for the heart.

**Example 2:**
> The rule you're talking about is the IPF's 2023 change: at the bottom of the rep, the elbows have to drop below the top of the shoulder joint. It was aimed squarely at the extreme-arch, ultra-wide-grip benches where the bar barely moved. And like the comment says, judging it is genuinely fuzzy — it's a visual call from the side of the platform, and when it first came in even experienced judges weren't calling it consistently. Since you can't control the judging, control your margin: Set up for your stiff days, not your best days. [...the rest is setup advice, and it ends with "If you compete outside the IPF, read your own federation's rulebook for the division you're in."]

### `claim`

**Definition:** The post gives advice or an opinion with confidence but
offers nothing checkable and no personal experience behind it. It's only the
poster's say-so.

**Example 1:**
> It won't affect your gains if you run daily for 30 minutes. You don't have to think about it. If you want to be safe with your progress then remember to do strength training before running when you do it in a same day. Right after running you will have worse results in lifts but if running is in another day then it's gonna be okay. Answering your question - lower intensivity activities will be better if you want to affect less your lifts but they will be worse in building your conditioning.

**Example 2:**
> Second-guessing pain mechanisms with biomechanical models is so 90's...

### `personal`

**Definition:** The post's support, or the whole post, is the poster's own
training, body or experience: "I did X and Y happened."

**Example 1:**
> I used to have one of these and am considering getting another. I used one as a kid and got a 6 pack within weeks. You use it while doing crunches with yoir feet elevated for 3 minute sessions 3 times a week. Having this to pull against your abs just kind of forced you to brace your core more. I don't know if it really helps that much.

**Example 2:**
> Thanks, I figured out the flexibility by now :) I've been running the blacknoir version of the program for about two months now, and am quite happy with it. It's much lighter than full body no split workouts (which made me dread training days recently)

### The hardest boundary

**Which two labels:** `evidence` and `claim`, when a post states specific
facts with confidence but names no source. The post that showed me this:

> In the hip flexion arc of squats, glute activation is virtually just as high towards 150 degrees as it is close to (over-)extension. The quads aren't even able to generate a lot of force beyond 135 degrees knee flexion due to their length-strength profile.

Nothing is cited, so it reads like a claim. But the 150° and 135° are things
you could look up and prove wrong.

**The decision rule I used every time:** A checkable fact is enough. If the
post states a specific fact about the world that a reader could verify (a
number, a date, an angle, a named rule, a study), it's `evidence`, even if no
source is named and even if the tone is confident. The squat comment above is
`evidence`.

Two things don't count as a checkable fact (both came out of the stress test
in How I Used AI):

- **A prescription with a number in it.** "Rest 2–3 minutes between heavy
  sets" tells you what to do; it isn't a fact about the world. On its own
  that's `claim`.
- **Numbers about the poster's own body or training.** "I cut from 90 to 78 kg"
  can't be checked by anyone else. That's `personal`.

For posts that mix them, the order is evidence first, then personal, then
claim. One checkable fact anywhere makes it `evidence`. Otherwise, if the
poster's own experience is what's backing it up, it's `personal`. Otherwise
it's `claim`.

**Left out on purpose:** thanks, site moderation ("This borders on spam"),
and back-and-forth about the thread itself. They're a big share of the
comments, but they aren't about fitness and none of the three labels fit.
I didn't collect them rather than invent a fourth label for them.


---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Fitness Stack Exchange, through its public
API. I pulled the 300 newest answers and the 300 newest comments (early
November 2026), stripped the HTML, and kept every post whole. I dropped exact
duplicates, posts under 60 characters, posts over 1,600 characters, and
chatter: thanks, moderation, and short questions back to the asker. That left
449 candidates. 219 of them are in `labels.csv`.

**How I labelled them:**

1. **20 cold, by me.** No tools, only the definitions and the rule. They're
   marked `cold` in the note column and I haven't changed any of them since.
2. **The rest were pre-labelled by Claude.** Claude labelled the 449
   candidates in three batches using my definitions and rule, word for word.
   73 were marked skip (chatter that got past the filter). From the rest I
   took a balanced 200: 60 evidence, 60 personal, 80 claim.
3. **A second, blind pass.** A separate Claude run labelled those 200 again
   without seeing the first labels. The two passes agreed on 175 of 200
   (87.5%).
4. **I decided all 25 disagreements myself.** I read each post with both
   labels and picked one. I dropped one of them (a comment that was only a
   link) as not really a post. The note column says which rows went through
   this.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `claim` | 85 | 38.8% |
| `personal` | 70 | 32.0% |
| `evidence` | 64 | 29.2% |
| **Total** | 219 | 100% |

`claim` is the biggest, as I expected. On the real site it's even bigger:
the first pre-label pass put it near 59% of one batch. I sampled down to keep
it under the 60% in criterion 3.

**Three hard cases**

**1.**
> *The post:* "Note that calculators and apps for working these things out can be at least as wrong, I just tried this one niddk.nih.gov/bwp (which being NIH ought to be good) and it said I would need to eat 3,300 calories a day to maintain my current weight. I would be huge if I ate anything close to that, 2,500 would be more realistic. At the end of the day (IMHO) there is so much individual variability that it is all just a "rule of thumb"..."
>
> *Could have been:* `evidence` (it names the NIH planner and a number) or `personal`.
>
> *I chose, because:* `personal`. The NIH source isn't backing the point. It's what the poster is arguing *against*, and the support is their own body ("I would be huge"). The 3,300 and 2,500 are numbers about them.

**2.**
> *The post:* "Of course you can, you may take a little longer to build up levels of testosterone ... You can follow a guide like this https://homefitnesscorp.com/blogs/workout-gym-tips/free-weights-workout-lower-body-leg-routine which will help with lower body workouts. Apart from that advice, just rest, enough sleep and a healthy diet will help you a lot ... I tend to do, Arms one day, Chest the next..."
>
> *Could have been:* `evidence` (it links a source) or `claim` (the link is just a routine, and the hormone talk has nothing behind it).
>
> *I chose, because:* `evidence`. My rule counts a named source and a reader can follow the link and check it. This is the case where my rule is most generous, and I'll watch for it in unit 6.

**3.**
> *The post:* "I suffered from shin splints and a stress fracture when I got back to running after 15 years of less than healthy habits. My physical therapist noted that I had bad running form ... my average cadence went from 160 to 175 over the next year. I am generally between 175 and 180 now. I have been running for many years now, and was even able to qualify for Boston..."
>
> *Could have been:* `evidence` (one pre-label pass found a general 170–180 cadence figure in it) or `personal`.
>
> *I chose, because:* `personal`. Almost every number is the poster's own (160 to 175, qualifying for Boston), and my rule says numbers about yourself are personal. The story is the support.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs, learning rate 2e-5, batch size 16, max length 128
tokens, seed 42.

**Anything I changed from the defaults, and why:** Nothing. The only edit to
the notebook was `LABELS = ["evidence", "claim", "personal"]` and my name.

One thing to know about max length: many Fitness SE answers are longer than
128 tokens, so the model only reads the start of them. In some long answers
the checkable fact comes near the end, which could matter for `evidence`.
I've left it at the default for now and noted it for unit 6.

**Where it ran:** `mps` (Apple Silicon GPU), torch 2.14.1. `results.json`
records the same. Training took about 14 seconds.

**Split sizes:** 153 train / 33 val / 33 test (70/15/15, stratified).

| Label | Train | Val | Test |
|---|---|---|---|
| `evidence` | 45 | 9 | 10 |
| `claim` | 59 | 13 | 13 |
| `personal` | 49 | 11 | 10 |

Every label has at least 10 in test, so none falls below the ~8 the brief
warns about. Ten is still small, though: one post changes a label's recall by
10 points.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1: stress-testing the labels (Milestone 2)**

- *What I asked for:* I gave Claude my three definitions and the "a checkable
  fact is enough" rule, and asked for eight made-up posts that sit right on
  the boundaries.
- *What came back:* Eight posts. Six I could label cleanly. Two I couldn't:
  "Rest 2-3 minutes between heavy sets. Anything less and your bar speed
  drops." has a number but is advice, and "I cut from 90 to 78 kg in six
  months" is all numbers but only about the poster. The full table is in
  `community_notes.md`.
- *What I changed:* I added two lines under my rule: a prescription with a
  number isn't a checkable fact (so it's `claim`), and numbers about yourself
  are `personal`. I didn't change the labels themselves. I also put the same
  exceptions into `label_definitions.txt` so the baseline gets them too.

**Moment 2: pre-labelling with two blind passes (Milestone 4)**

- *What I asked for:* Claude pre-labelled the 449 candidate posts using my
  definitions word for word. Then a separate Claude run re-labelled the 200
  I picked without seeing the first labels, so I could see where it was
  unsure instead of trusting one pass.
- *What came back:* The passes agreed on 175 of 200 (87.5%). Nearly all 25
  splits were on my hardest boundaries: posts that only link a resource or
  name a program (evidence or claim?), and running or weight stories with
  one general fact inside (evidence or personal?).
- *What I changed:* I decided all 25 myself. Mostly I went with personal when
  the poster's own body was the real support, even if a source was named (the
  NIH calorie planner post is hard case 1). I dropped one comment that was
  only a link. Doing this showed me my rule is generous about links. I count
  a bare link as a named source, and the second pass didn't.

**Pre-labelling disclosure:** 199 of the 219 rows were pre-labelled by Claude
(the note column says `pre-labelled (Claude)` on every one of them). The 20
`cold` rows are mine alone, labelled with no tools before I saw any AI labels.
For the 25 posts where the two Claude passes disagreed, I read each post and
chose the label (24 kept, one dropped); the note says `passes split A/B, I
decided X`. For the 175 rows where both passes agreed, I kept the label and haven't yet re-read each
one individually. I'm reviewing those before unit 6 and will note any I
change.

**Other AI use, for honesty:** for `criteria.md`, I asked Claude for
suggestions. It wouldn't write the criteria, but it gave me options for each
number and each reason, and I picked one of each. It then gave one "too easy"
and one "too hard" reason for every target and pointed out that #4 needed a
confidence cutoff someone else could check, so I added 0.7. When I asked it
to suggest labels for the 20 cold posts, it refused, because that would make
them not cold.

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
