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
This system takes in 200 posts from the subreddit r/baseball, and sorts them into one of three categories:"critique","question", and "informative". The classifier is first trained to identify the patterns among the 3 labels, and then is given the task to label posts it has never seen before using the patterns it has learned. This project is meant to determine how consistently and accurate the classifier can be at sorting the different posts.


---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `label_one`
- Critique

**Definition:**
Evaluates baseball,a player,a game, or stats by expressing praise,critisism, or both.

**Example 1:**
>I didn't notice the Yankee bias live and don't really care. Having not watched ESPN in forever I was just horrified how bad it was overall. Two things that stuck out to me not mentioned in article: After Yoshida gets a PH single, Perez says "You are all wondering why he isn't being run for". No, I am not. It is the 7th inning, there is 1 out and a man on 1B only. He is a PH and you aren't wasting another bench spot. Who the hell would EVER pinch run there? After the Yankees used a challenge in the 9th, they say "....use your challenge there". They never mention there are 2 in the playoffs. You would only know it because the UMP said it. It completely changes the context of the challenge - it would have been insane to do with only 1 challenge.

**Example 2:**
>Casual fan, but is the umpiring worse than ever this season? Is it just getting more attention? It seems like every day something atrocious happens. Serious question, do professional umps have to take eye exams?

### `label_two`
- Informative

**Definition:**
Gives a statement backed by a checkable fact. Can include a stat or a date.

**Example 1:**
> With their 2-1 loss to the Pirates the Chicago Cubs have lost 9 games in a row and With their last win coming against the Chicago White Sox on May 15th beating them 10-5

**Example 2:**
>Kyle Schwarber has 63 HR in his last 162 games played. Source: baseball reference game logs

### `label_three`
- Question

**Definition:**
Asks for information or insight on what could be a player, a game, or more about baseball.

**Example 1:**
>Hello fellow baseball fans. I've been listening to Sleep Baseball and it works great. I am always knocked out by the third inning when i hear a game on a podcast. Does your team put their play-by-play broadcasts on podcasts? Or are there any services that just put up random games for people to listen to after the fact? Thanks much in advance for your input!

**Example 2:**
> What's the saddest story or moment in baseball history? Make me cry

### The hardest boundary

**Which two labels:**

1. Informative & Critique

2. Critique and Question

3. Informative and Question



**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->

- If the post primarily states or explains factual information, it's 'informative', even if it includes opinions.
- If the post primarily expresses an opinion or evaluates a subject, it's 'critique', even if it includes statistics or facts.
- If the post primarily asks the community for answers, explanations, or opinions, it's 'question', even if it includes facts or personal opinions.


---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**
The posts came from a subreddit made for baseball. You can find the posts by searching "r/baseball".


**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| informative | 77 | 38.5% |
| question | 67 | 33.5% |
| critique | 56 | 28% |
| **Total** | 200 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:*
>Max Muncy got his eyes fixed a month ago. Since then he has 9 HRs and 31 RBIs.In his past seven games, he is hitting better than .400, has an OBP of .500, and has an OPS of .1727. He is the angriest man in baseball. He is suffocating. He is Maximum Muncy.
> *Could have been:*
> Informative
> *I chose, because:*
I chose critique, because of how this posts then after gives admiration due to the stats that they pointed out. This makes it praise.


**2.**
> *The post:*
>What was the loudest crack of the bat you’ve heard in-person? EDIT: Here are the names coming up the most: Kyle Schwarber, Barry Bonds, Giancarlo Stanton, Shohei Ohtani, Bo Jackson
> *Could have been:*
> Critique
> *I chose, because:*
I chose question, because the main idea is a question on people's experience, although the names that come after can be seen as opinions.


**3.**
> *The post:*
>What Player do you have zero respect for? Mine is without a doubt Marcell Ozuna of the Atlanta Braves. How in the McHell did he only get a 20 game suspension for what he did

> *Could have been:*
> Critique
> *I chose, because:*
I chose question, because even though the person gives their own opinion, they are asking for more opinions to be shared.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**
- device: cpu (CPU) 
- torch: 2.14.1+cpu 


**Settings:** <!-- epochs, learning rate, batch size, seed -->
- seed 42, 3 epochs, lr 2e-05, batch 16

**Anything I changed from the defaults, and why:**
- I did not change anything before my run.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->

train 139  ·  val 31  ·  test 30

             train  val  test
label                        
critique        39    9     8
informative     53   12    12
question        47   10    10



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:*
I asked Claude to pre-label the remaining 180 posts based off the decision rules and labels I have made for labeling.
- *What came back:*
Claude labeled each post from one of the three categories, and noted "pre-labeled" on each one.
- *What I changed:*
4 of the posts were incorrectly labeled as I was reviewing, because they did not follow a decision rule I told it to keep in mind of. I labeled the posts the correct ones, and also made a note "changed" after each post to keep track of how many were updated.

**Moment 2**

- *What I asked for:*
I asked Claude to help me understand what the f1 really meant and what was being calculated to get that number.
- *What came back:*
it explained that the f1 calculates two kinds of mistakes: Missed posts, and Wrong Calls. It takes in both and folds it into 1 score. Having fewer of each means a higher f1. Which is better.
- *What I changed:*
I made criterion 2 based off each labels f1: "All 3 labels have an F1 of at least .60.". This helped me evulate not only the whole accuracy of the WHOLE run, but the accuracy of each label.


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
