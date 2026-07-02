# 05 — Capstone Task: Put CNN, CLIP, and VLM on Trial

> The final deliverable of the track. You'll prompt a **vision–language model** to interpret Euclid cutouts, catch it **hallucinating**, then stage a three-way audit — the **Week-3 CNN**, the **Week-4 CLIP** ranker, and the **Week-5 VLM** — on the *same* images. The output is a single comparison figure and a science-aware capstone reflection on when to trust automation and what to send to a human. No new model to train; the skill on trial is your *judgement*.

---

## The Task

> **Audit a VLM, then compare three tools.** Take a stratified 30–50 image sample from the Euclid test split (lenses + hard negatives). Prompt an open VLM (`Qwen/Qwen2-VL-2B-Instruct`) with a **fixed** `VERDICT/EVIDENCE/CAUTION` prompt, parse the verdicts, and score precision/recall against ground truth. Find and document at least two **hallucinations**. Then reload your **Week-3 CNN** and your **Week-4 CLIP** scores, run all three on the *same* cutouts, and build one comparison figure: `image | true label | CNN pred | CLIP score | VLM verdict + quote`. Finish with a ~300-word reflection on trust, precision, and human review.

---

## Starter and Solution Notebooks

There is **one notebook pair** this week. Attempt the starter before opening the solution.

| Part | Starter | Solution |
|------|---------|----------|
| VLM audit + capstone | 🟢 [`notebooks/week5_lens_vlm_starter.ipynb`](notebooks/week5_lens_vlm_starter.ipynb) | 🟡 [`notebooks/week5_lens_vlm_solution.ipynb`](notebooks/week5_lens_vlm_solution.ipynb) |

The notebook opens in Colab directly. **Switch the runtime to GPU** (`Runtime → Change runtime type → GPU`) — the default VLM (`Qwen/Qwen2-VL-2B-Instruct`) runs on the T4 with no API key. Have your **Week-3 `galaxy_model.pth`** and your **Week-4 CLIP scoring cells** ready; the solution includes fallback setup for both. A commented hosted-API cell (Gemini / GPT-4o) is provided if you'd rather use one.

---

## Step-by-Step Checklist

Each step maps to a cell (or small group) in `week5_lens_vlm_starter.ipynb`.

- [ ] **Step 0.** Open the starter in Colab with a **GPU runtime** and run the installs (`transformers`, `datasets`, `qwen-vl-utils`, `accelerate`, `scikit-learn`).
- [ ] **Step 1.** Reload the Euclid test split and take a **stratified 30–50 sample** — include real lenses (label 1) *and* hard negatives (label 0). Keep the images, labels, and ids together. (See [`Week-4/04`](../Week-4/04-zero-shot-lens-finding.md) for the dataset.)
- [ ] **Step 2.** *(Optional)* Show 2–3 examples from [`Arbie333/gravitational_lensing`](https://huggingface.co/datasets/Arbie333/gravitational_lensing) with their reference captions, as a "good description" benchmark.
- [ ] **Step 3.** Load `Qwen/Qwen2-VL-2B-Instruct` (half precision) and define the **fixed prompt** from [`01`](01-vlm-prompting-for-science.md).
- [ ] **Step 4.** Loop over the sample: send image + prompt, store the raw text, `VERDICT`, `EVIDENCE`, and `CAUTION`. Use low temperature / greedy decoding.
- [ ] **Step 5.** Parse verdicts and map `yes/no/uncertain` to binary; compute **precision, recall, F1** vs ground truth. (Decide and document how you treat `uncertain`.)
- [ ] **Step 6.** **Hallucination hunt:** surface ≥2 cutouts where the VLM confidently claims a lens that the label and your eyes reject; save the image + its `EVIDENCE` quote. (See [`02`](02-hallucination-and-human-in-the-loop.md).)
- [ ] **Step 7.** Reload the **Week-3 `GalaxyCNN`** from `galaxy_model.pth` and run it on the *same* cutouts. Record its morphology prediction. Expect poor relevance — that's the wrong-task lesson ([`04`](04-comparing-cnn-clip-and-vlm.md)).
- [ ] **Step 8.** Compute **Week-4 CLIP** lens scores on the same cutouts (reuse your prompt-bank scoring).
- [ ] **Step 9.** Build the **comparison figure/table**: `image | true label | CNN pred | CLIP score | VLM verdict + quote`.
- [ ] **Step 10.** Write the **~300-word capstone reflection** (see prompts below).

---

## Acceptance Criteria

Your submission counts as complete when, on a fresh Colab session:

1. The notebook runs top-to-bottom (`Runtime → Restart and run all`) **without errors** on a GPU runtime.
2. You run the VLM on **≥30** cutouts with a **documented, fixed** prompt and parse its verdicts.
3. You report the VLM's **precision/recall** against ground truth and state how you handled `uncertain`.
4. You show at least **two** concrete **hallucination / overconfidence** examples, each with the image and the model's own quoted evidence.
5. You reload the **Week-3 CNN** and run it on the same images, and you **explain** why it's the wrong tool (wrong task + domain shift).
6. You present **CNN vs CLIP vs VLM** on the **same** images in one figure or table.
7. You write a **~300-word capstone reflection** on trust, precision, and human review.
8. The notebook is saved to your Drive *or* pushed to your fork. (Don't commit the dataset, images, or `.pth` to the repo — see Submission.)

---

## Presentation Template

Structure your capstone writeup (in the notebook, or slides if your cohort presents) like this:

1. **The question.** One line: can multimodal AI find rare strong lenses, and where does it fail?
2. **Setup.** Dataset, sample size + class balance, the three systems, the fixed VLM prompt.
3. **Results.** VLM precision/recall; the three-way comparison figure.
4. **Failure gallery.** Your ≥2 hallucinations + at least one shared CLIP/VLM confusion, each explained astrophysically ([`03`](03-arcs-vs-spirals-confusion-physics.md)).
5. **Judgement.** Which tool for which job; where you'd set a threshold; what you'd send to a human.
6. **Honest limits.** Sample size, label uncertainty, model nondeterminism, domain shift.

---

## Stretch Goals (Optional but Recommended)

### 1. Prompt-paraphrase Monte Carlo

Rewrite the VLM prompt 5 different ways (same meaning, different words) and re-run on a handful of images. How much do the verdicts move? Quantifying this instability is a direct measurement of how much to trust any single answer. (See [`01`](01-vlm-prompting-for-science.md).)

### 2. CLIP + VLM ensemble (simple voting)

Combine the Week-4 CLIP score (thresholded) with the VLM verdict by simple voting (e.g. flag only if both agree). Does agreement improve precision over either alone? Tie back to "correlated failures" in [`04`](04-comparing-cnn-clip-and-vlm.md).

### 3. Export a candidate CSV

Save your top-ranked candidates (id, CLIP score, VLM verdict, VLM evidence) as a CSV — the artefact a real pipeline hands to a human expert or a Space Warps queue. This is the concrete output of the human-in-the-loop workflow from [`02`](02-hallucination-and-human-in-the-loop.md).

### 4. Try the API path

Swap the open model for the commented Gemini/GPT-4o cell and compare verdicts on the same 30–50 images. Does the stronger model hallucinate less, or just more convincingly?

---

## Reusing Your Week-3 and Week-4 Work

- **Week-3 CNN:** you need the `GalaxyCNN` class definition and the saved `galaxy_model.pth` (instantiate `GalaxyCNN(num_classes=...)`, then `load_state_dict`). If you didn't keep the weights, retrain quickly from `week3_cnn_solution.ipynb`, or note in your writeup that you're using a freshly trained copy. Remember its inputs are `64x64` — resize the Euclid cutouts to match.
- **Week-4 CLIP:** reuse your prompt bank and the `score = mean(lens sims) - mean(non-lens sims)` scorer from `week4_lens_clip_solution.ipynb`, applied to this week's sample.

---

## Opening the Notebook in Colab

### Option A — From a GitHub fork (recommended)

1. Fork the [CSoT repo](../../../README.md) on GitHub.
2. In Colab: **File → Open notebook → GitHub** tab.
3. Open `ML-Astronomy/Week-5/notebooks/week5_lens_vlm_starter.ipynb`.
4. Edits autosave to Drive; push back with **File → Save a copy in GitHub**.

### Option B — Direct GitHub link

Paste a URL like this into Colab's "GitHub" tab (swap in your username):

```
https://github.com/YOUR_USER/csot-ml-astronomy/blob/main/ML-Astronomy/Week-5/notebooks/week5_lens_vlm_starter.ipynb
```

### Option C — Upload

Download the `.ipynb` from this folder, then in Colab use **File → Upload notebook**.

---

## Submission

Submission logistics are confirmed via the official CAIC channels. Default expectation:

- Push your completed notebook into a `submissions/<your-name>/week5/` folder in your fork, **or**
- Share a Colab "share link" (with comment access) with your mentor.

> **Do not commit the dataset, image files, or `galaxy_model.pth` to the repo.** This is a teaching repository with no binary assets — the notebook re-downloads data and models on demand. A candidate CSV is fine to attach to your submission; keep images and weights out of the repo.

Ask in the mentor channel **before** the week ends if anything is unclear.

---

## Reflection Questions (this is the ~300-word capstone)

Write a cohesive ~300-word reflection covering:

1. **Trust.** Given your VLM precision/recall and hallucination examples, would you trust it to flag lenses unsupervised? Where exactly does it earn or lose your trust?
2. **The wrong tool.** What did running the Week-3 CNN on Euclid cutouts teach you about the limits of a trained model outside its task and domain?
3. **The pipeline.** If you ran a real Euclid/Rubin lens search, how would you combine CNN/CLIP/VLM and humans? Where would you set your threshold, and roughly how many candidates would you send to a human expert — and why? (Ties to [`02`](02-hallucination-and-human-in-the-loop.md).)
4. **Looking back.** From Week 1's first tensor to here — how has your sense of what AI can and can't do for science changed?

These won't be graded — they're the capstone of your own learning. Keep them; re-read them in a year.

---

⬅️ Previous: [`04-comparing-cnn-clip-and-vlm.md`](04-comparing-cnn-clip-and-vlm.md) | 📚 See also: [`resources.md`](resources.md)
