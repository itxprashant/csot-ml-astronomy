# 05 — Project Task: Hunt Lenses with CLIP

> Your deliverable for the week: a **zero-shot strong-lens detector** built on a frozen CLIP, evaluated the way a real survey would evaluate it. You'll load the Euclid expert-graded cutouts, report their class balance, score every test image with a bank of text prompts, plot ROC / precision-recall, and assemble an error gallery whose failures you can explain *astrophysically*. No training loop, no saved weights — the whole point is how far a pretrained multimodal model gets, and where it breaks.

---

## The Task

> **Build and evaluate a CLIP zero-shot lens ranker.** Load [`mwalmsley/euclid_strong_lens_expert_judges`](https://huggingface.co/datasets/mwalmsley/euclid_strong_lens_expert_judges) (`classification` config), print the train/test label distribution, and visualise example lenses and non-lenses. Load `openai/clip-vit-base-patch32`, embed every test cutout, and score each one as *(mean similarity to lens prompts) − (mean similarity to non-lens prompts)*. Sweep the threshold to plot an ROC and a precision-recall curve, report ROC-AUC, and justify a threshold choice. Finally, build a TP / FP / FN error gallery and write a short interpretation of which failures are physically understandable.

The science question behind it: *can a general-purpose vision–language model find rare cosmological needles, and where does it hallucinate arcs that aren't there?*

---

## Starter and Solution Notebooks

There is **one notebook pair** this week. Attempt the starter before opening the solution.

| Part | Starter | Solution |
|------|---------|----------|
| CLIP lens hunter | 🟢 [`notebooks/week4_lens_clip_starter.ipynb`](notebooks/week4_lens_clip_starter.ipynb) | 🟡 [`notebooks/week4_lens_clip_solution.ipynb`](notebooks/week4_lens_clip_solution.ipynb) |

The notebook opens in Colab directly and downloads the dataset from Hugging Face — no Galaxy Zoo pipeline needed this week. **Switch the runtime to GPU** (`Runtime → Change runtime type → GPU`) before you start; embedding ~1,476 images with CLIP is far faster on the T4.

---

## Step-by-Step Checklist

Each step maps to a cell (or small group) in `week4_lens_clip_starter.ipynb`.

- [ ] **Step 0.** Open the starter in Colab with a **GPU runtime** and run `pip install datasets transformers`. Confirm `device` prints `cuda`.
- [ ] **Step 1.** Load the dataset's `classification` config, `train` and `test` splits. (See [`02-strong-lens-morphologies.md`](02-strong-lens-morphologies.md) for what the labels mean.)
- [ ] **Step 2.** **First analysis cell:** print train/test sizes and the **label counts** (`0` = not a grade-A/B lens, `1` = grade-A/B lens). Note the imbalance out loud — it drives every metric choice. (See [`04-zero-shot-lens-finding.md`](04-zero-shot-lens-finding.md).)
- [ ] **Step 3.** Plot a matplotlib grid of a few example **positives** and **negatives** so you know what you're hunting.
- [ ] **Step 4.** Load CLIP (`openai/clip-vit-base-patch32`) and its `CLIPProcessor`; move the model to `device` and set `model.eval()`.
- [ ] **Step 5.** Define your **prompt bank** — at least two lens prompts and two non-lens prompts (start from the bank in [`04`](04-zero-shot-lens-finding.md)). Embed the prompts once.
- [ ] **Step 6.** Embed all test images **in batches of 32–64** under `torch.no_grad()`; normalise embeddings.
- [ ] **Step 7.** Compute each image's **score** = mean(lens-prompt sims) − mean(non-lens-prompt sims).
- [ ] **Step 8.** Compute **ROC-AUC**, then plot the **ROC** and **precision-recall** curves. Pick a threshold and justify it (survey-style: favour precision).
- [ ] **Step 9.** Build the **error gallery**: top true positives, worst false positives, worst false negatives. Title each with its score and true label.
- [ ] **Step 10.** Write the **reflection** (Markdown): which failures are physically understandable, and why is accuracy the wrong headline metric here?

---

## Acceptance Criteria

Your submission counts as complete when, on a fresh Colab session:

1. The notebook runs top-to-bottom (`Runtime → Restart and run all`) **without errors** on a GPU runtime.
2. You print the **label distribution** for both train and test and comment on the imbalance.
3. You run **CLIP zero-shot** scoring with **at least two lens and two non-lens prompts**.
4. You plot an **ROC or precision-recall** curve, report **ROC-AUC**, and justify a **threshold** choice.
5. You display an **error gallery** with at least one **false positive** where spiral/ring structure plausibly confused the model, and explain it astrophysically.
6. You state, in writing, **why accuracy alone is misleading** for this rare-object search, citing your dataset's class balance.
7. The notebook is saved to your Drive *or* pushed to your fork so a mentor can review it. (Don't commit the dataset or any images to the repo — see Submission.)

---

## Stretch Goals (Optional but Recommended)

### 1. Prompt engineering sweep

Try three different prompt banks (terse vs descriptive vs decoy-aware) and compare ROC-AUC. How much does wording alone move the score? This is the most direct way to *feel* CLIP's prompt-sensitivity.

### 2. Linear probe on frozen embeddings

Take CLIP's frozen image embeddings for the **train** split and fit a simple `LogisticRegression` (scikit-learn) on them using the real labels, then evaluate on test. Does a tiny supervised head beat pure zero-shot? (This is the bridge between zero-shot and full fine-tuning — cheap because CLIP stays frozen.)

### 3. AstroCLIP comparison

If Colab memory allows, compare generic CLIP against [AstroCLIP](https://github.com/PolymathicAI/AstroCLIP), a CLIP-style model trained on astronomical images. Does domain-specific pretraining help on Euclid cutouts?

### 4. Cross-domain sanity check

Run your **lens** prompts on a handful of **Galaxy Zoo 2** spirals from earlier weeks. How often does CLIP call a spiral a lens? Tie the result back to the spiral-arm/arc confusion in [`02`](02-strong-lens-morphologies.md).

### 5. Export candidates for "follow-up"

Save your top-20 highest-scoring cutouts as a CSV (id + score) — the artefact a real pipeline would hand to a human expert. You'll reuse this idea in [Week 5](../Week-5/).

---

## Opening the Notebook in Colab

### Option A — From a GitHub fork (recommended)

1. Fork the [CSoT repo](../../../README.md) on GitHub.
2. In Colab: **File → Open notebook → GitHub** tab.
3. Open `ML-Astronomy/Week-4/notebooks/week4_lens_clip_starter.ipynb`.
4. Edits autosave to Drive; push back with **File → Save a copy in GitHub**.

### Option B — Direct GitHub link

Paste a URL like this into Colab's "GitHub" tab (swap in your username):

```
https://github.com/YOUR_USER/csot-ml-astronomy/blob/main/ML-Astronomy/Week-4/notebooks/week4_lens_clip_starter.ipynb
```

### Option C — Upload

Download the `.ipynb` from this folder, then in Colab use **File → Upload notebook**.

---

## Submission

Submission logistics are confirmed via the official CAIC channels. Default expectation:

- Push your completed notebook into a `submissions/<your-name>/week4/` folder in your fork, **or**
- Share a Colab "share link" (with comment access) with your mentor.

> **Do not commit the dataset or any image files to the repo.** This is a teaching repository with no binary assets — the notebook re-downloads the dataset from Hugging Face on demand. If you export a candidate CSV, that's fine to attach to your submission, but keep images out of the repo.

Ask in the mentor channel **before** the week ends if anything is unclear.

---

## Reflection Questions

Write 2–3 sentences each in a Markdown cell at the bottom of the notebook:

1. What ROC-AUC did your zero-shot CLIP reach? Given that no labelled lenses were used to build the scorer, is that better or worse than you expected — and why?
2. Look at your worst false positives. Pick one and explain, using [`02-strong-lens-morphologies.md`](02-strong-lens-morphologies.md), why a human (or a model) could reasonably mistake it for a lens.
3. Your dataset is imbalanced. What accuracy would a "say no to everything" model get, and what does that tell you about why you reported precision/recall instead?
4. If you were running a real Euclid lens search, where would you set your threshold, and how many candidates would you be willing to send to a human expert? (This sets up Week 5.)

These won't be graded — they exist so you can look back at the end of the track and see how your thinking about AI and astronomy matured.

---

⬅️ Previous: [`04-zero-shot-lens-finding.md`](04-zero-shot-lens-finding.md) | 📚 See also: [`resources.md`](resources.md)
