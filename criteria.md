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

## 1.
Accuracy is at least .70
<!-- Your criterion. It must name a number. -->



**Why this target:**
This can help with determining whether the model actually learned the difference from each of the 3 labels. Some of my posts will be harder to label since they can sit between 2 different labels. Using .70 is about double the chance of getting it right, since a random guess out of the 3 labels is about .33.

---

## 2.
All 3 labels have an F1 of at least .60.
<!-- Your criterion. -->



**Why this target:**
Since each label can said to be distinct since one judges, another informs, and the third asks, it makes them easier to be separated. Since some labels may have less posts than others, it can be computed on a small handful, which is why it is also lower than the overall accuracy.

---

## 3.
No label can make up more than 60% of the dataset
<!-- Your criterion. -->

**Why this target:**
If one label dominates, the model will mostly learn that label and barely see the other two, so this will make the model not predict those 2 well. The baseball subreddit is mostly made of criticism and stats, so this will give the labels that can dominate more of a limit. With 200 posts, the limit to a label will be no more than 120, which leaves 80 for the other 2.


---

## 4.
If someone else labeled the same posts, they should agree at least on 80% of the classifications.
<!-- Your criterion. -->



**Why this target:**
This checks whether the logic in my labeling is actually consistent. Since the model is only working with the ideas that im feeding it, if a person cannot agree with at least 80% of the posts in the system, the results that the model gives won't mean anything later on. There are hard cases I expect to be missed, since a person can label a post differently than another.


---

## 5.
Original: The models top confidence pile is right 90% of the time.

Revised: The models TOP THIRD confidence pile is right 90% of the time.

Why: This makes the criterion measurable, since the pile size or threshold size was not mentioned. This helps me produce a verdict.
<!-- Your criterion. -->



**Why this target:**
The top confidence pile should be the most distinguishable posts, which are those that can just ask a question, state facts, or talk criticism clearly. These are the posts that I want to accept without reviewing. 90% would mean that these types of posts should have barely any misses.
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
