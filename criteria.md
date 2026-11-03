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

## 1. Overall accuracy is at least 0.55

Accuracy on the held-out test split is **at least 0.55 on every one of the
three seeds** (42, 7, 2024).

**Why this target:** Three labels means guessing gets about 0.33, and `claim`
will probably be around half the data. 0.55 means the model learned more than
just "say claim" every time.

---

## 2. Every label has an F1 of at least 0.40

On the held-out test split, the F1 score of **each** label (`evidence`,
`claim` and `personal`) is **at least 0.40, on all three seeds**.

**Why this target:** `personal` is my smallest label and will only have about
8 posts in the test split, so a strict floor would mostly measure noise. 0.40
still proves the model isn't ignoring a label.

---

## 3. No label is more than 60% of labels.csv

Counting the `label` column of `labels.csv`, the largest label is **at most
60% of all rows**.

**Why this target:** Most answers on Fitness SE are advice with no source, so
`claim` will naturally be the biggest label. Above 60% the model can learn to
just guess `claim`. That's stricter than the course's 70% cap on purpose.

---

## 4. Confident predictions are at least 10 points more accurate

On the held-out test split, split the predictions by the model's top
probability: confident (above 0.7) and unsure (0.7 or below). Accuracy on the
confident ones is **at least 10 percentage points higher** than accuracy on
the unsure ones, on all three seeds.

**Why this target:** If confident and unsure predictions are right equally
often, the confidence number means nothing and the model is basically
guessing. A 10 point gap shows its confidence actually tells me something.

---

## 5. I agree with my own cold labels at least 16 out of 20 times

I re-label the 20 posts marked `cold` in `labels.csv`, at least a day later
and without looking at my first labels. **At least 16 of the 20 (80%) get the
same label** as the first time.

**Why this target:** The model can't be more consistent than the labels it
learned from. 16 of 20 is the level I'd trust as training data. Lower than
that and the problem is my labels, not the model.

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
