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
Critique

**Definition:**
Evaluates baseball,a player,a game, or stats by expressing praise,critisism, or both.

**Example 1:**
>I didn't notice the Yankee bias live and don't really care. Having not watched ESPN in forever I was just horrified how bad it was overall. Two things that stuck out to me not mentioned in article: After Yoshida gets a PH single, Perez says "You are all wondering why he isn't being run for". No, I am not. It is the 7th inning, there is 1 out and a man on 1B only. He is a PH and you aren't wasting another bench spot. Who the hell would EVER pinch run there? After the Yankees used a challenge in the 9th, they say "....use your challenge there". They never mention there are 2 in the playoffs. You would only know it because the UMP said it. It completely changes the context of the challenge - it would have been insane to do with only 1 challenge.

**Example 2:**
>Casual fan, but is the umpiring worse than ever this season? Is it just getting more attention? It seems like every day something atrocious happens. Serious question, do professional umps have to take eye exams?

### `label_two`
Informative

**Definition:**
Gives a statement backed by a checkable fact. Can include a stat or a date.

**Example 1:**
> With their 2-1 loss to the Pirates the Chicago Cubs have lost 9 games in a row and With their last win coming against the Chicago White Sox on May 15th beating them 10-5

**Example 2:**
>Kyle Schwarber has 63 HR in his last 162 games played. Source: baseball reference game logs

### `label_three`
Question

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

I first labeled the first 20 posts by myself using the rules on how to determine the label. After, I asked Claude to pre-label them, which I then after checked each and labeled them checked, with an extra note "changed" for the ones I corrected.

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
| Overall accuracy | 0.667 | 0.733 | +0.067 |
| Macro F1 | 0.577 | 0.567 | -0.011 |
| F1 — `critique` | 0.200 | 0.000 | -0.200 |
| F1 — `informative` | 0.818 | 0.960 | +0.142 |
| F1 — `question` | 0.714 | 0.741 | +0.026 |


**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
I expect my trained model to beat the baseline model on overall accuracy and Macro f1, since the trained model has learned from more than 100 baseball posts. The baseline model has only seen my three label definitions.


**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->
The model won on overall accuracy, but it was due to the 'informative' and 'question' labels. The trained model failed to recognize the 'critique' label, which meant that the f1 score for that label in the trained model was scored a 0, which affected the Macro f1. This meant that the baseline model got a higher Macro f1 score.


---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  | Accuracy | >=.70 |  |  |  |
| 2.  | Every label F1 | >=.60 |  |  |  |
| 3.  | Label share | <=.60 |  |  |  |
| 4.  | Classification Agreement | >=80% |  |  |  |
| 5.  | Top-confidence pile  | >=90% |  |  |  |



### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | critique | informative | question |
|---|---|---|---|
| **critique** | 0 | 1 | 7 |
| **informative** | 0 | 12 | 0 |
| **question** | 0 | 0 | 10 |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->
The model did not learn the decision boundary that separates the 'critique' label from the other two. Instead, it labeled 1 of the posts 'informative' and 7 of them 'question'.


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

**Agreement rate:** 26 / 30 = 87%

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
>"Checked the entry lists. 41% of this year's field have never run the distance before, against 26% five years ago. That's why the finish-line photos look different, not because standards dropped."

> *Staff said / I said:*
>'analysis'/'hot_take'

> *My call, and why:*
>I labeled it as hot_take, because I thought that the last sentence was an opinion being formed because of the stats used beforehand.

> *Which pile:*
Pile A

**2.**
> *The post:*
>"Ran the same loop in 4C and in 24C within a fortnight, same effort by heart rate. 90 seconds slower in the heat over six miles. People keep saying heat costs you 'a bit' and it's more than that."

> *Staff said / I said:*
>'analysis'/'hot_take'

> *My call, and why:*
> The post's last sentence was more of that person's take on the heat, which seemd to use the stats to mention that heat was a factor when running, which is why i labeled it as 'hot_take'

> *Which pile:*
Pile A


**3.**
> *The post:*
>"The 10K route was remeasured in 2021 and came out 60 metres long. Everyone comparing PBs from before and after that is comparing two different races, which is about 12 seconds at club pace."

> *Staff said / I said:*
>'analysis'/'hot_take'

> *My call, and why:*
> I labeled this post as 'hot_take', because it seemed like the post was talking more about a comparison of races.

> *Which pile:*
Pile A

**4.**
> *The post:*
>"Three of us ran the same session on the same day, different shoes, within 10 seconds of each other over 5 miles. I'm not saying shoes do nothing, but whatever they do was smaller than the noise between three people."

> *Staff said / I said:*
>'analysis'/'hot_take'

> *My call, and why:*
>I labeled this post as 'hot_take', because of how it described a fact or stat which was followed by an opinion about the shoes.
> *Which pile:*
Pile C. With this post having more of an opinion based sentence towards the end, one could argue that the statistic talked about was used more as a standing point on their take on the shoes.

**What the pattern in my disagreements tells me:**
- For the most part, 3 of the 4 incorrect labels were applied loosely, because they followed the taxonomy of each label, but they were just labeled incorrectly. This shows more of a consistency problem following cold labeling and the taxonomy rules of each label.



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
