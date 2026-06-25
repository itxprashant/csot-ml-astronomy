# Week 4 — Multimodal Lens Finding with CLIP

> A new project begins. For three weeks you taught a CNN to read galaxy **morphology**; now we hunt something far rarer — **strong gravitational lenses**, where a foreground mass bends the light of a background galaxy into arcs and rings. Instead of training a network from scratch, we borrow a giant pretrained **vision–language model (CLIP)** and ask it, in plain English, *"does this cutout look like a gravitational lens?"* — no training required. By the end you'll have a zero-shot lens ranker and the metrics to know when to trust it.

This week is split into **two parts**:

- **Part 1 — Lensing science:** what gravitational lensing is, the difference between **strong** and **weak** lensing (we only use strong), and the zoo of lens morphologies — Einstein rings, arcs, doubles, quads, and cluster lenses.
- **Part 2 — CLIP zero-shot:** what a vision–language model is, how CLIP maps images and text into one shared space, how to **score** cutouts with text prompts, and how to evaluate a **rare-object** search with precision, recall, and ROC — not accuracy.

This is a **second project**, not "tune the Galaxy Zoo CNN again." We swap the science (lensing, not Hubble morphology), the data (Euclid expert-graded cutouts, not SDSS Galaxy Zoo JPGs), and the ML paradigm (use a pretrained multimodal model, not train a CNN).

---

## At a Glance

- **Time estimate:** 4–6 hours.
- **Prerequisites:** Finish [Week 1](../Week-1/) through [Week 3](../Week-3/) first. You need a working Colab GPU workflow, comfort loading image data, and the confusion-matrix / precision-recall literacy from Week 3.
- **Primary dataset:** [`mwalmsley/euclid_strong_lens_expert_judges`](https://huggingface.co/datasets/mwalmsley/euclid_strong_lens_expert_judges) (the `classification` config) — 5,876 train + 1,476 test Euclid cutouts, each labelled `0` (not a grade-A/B lens) or `1` (grade-A/B lens).
- **Primary model:** CLIP (`openai/clip-vit-base-patch32`) via Hugging Face `transformers`. No fine-tuning — zero-shot only.
- **Deliverable:** One completed Colab notebook — `week4_lens_clip_starter.ipynb` — that loads the Euclid set, reports its class balance, scores every test cutout with CLIP text prompts, plots an ROC / precision-recall curve, and shows a true-positive / false-positive error gallery with an astrophysical reading of the failures.

---

## Learning Objectives

| ML / Computer Science | Astronomy / Physics |
|-----------------------|---------------------|
| Explain what a **vision–language model** is and how CLIP shares one embedding space for images and text. | Define **gravitational lensing** in plain language: mass curves spacetime, light follows the curve. |
| Describe **contrastive learning** intuitively — pull matching image–text pairs together, push mismatches apart. | Distinguish **strong** lensing (visible arcs/rings) from **weak** lensing (statistical; out of scope). |
| Load CLIP and embed **images** and **text prompts** on GPU or CPU. | Name the common strong-lens morphologies: Einstein rings, arcs, doubles, quads, cluster lenses. |
| Build a **zero-shot** classifier from prompts — no task-specific training. | Explain why lenses are scientifically valuable (dark-matter mass maps, magnified distant sources, `H0`). |
| Score a cutout as *(lens-prompt similarity) − (non-lens-prompt similarity)* using **cosine similarity**. | Disambiguate a gravitational **lens** from a Hubble **lenticular (S0)** galaxy — same prefix, different physics. |
| Choose a **threshold** and read an **ROC** / **precision-recall** curve. | Relate lens follow-up to real survey workflows (candidate ranking → human confirmation). |
| Evaluate a **rare** class with precision, recall, F1, and ROC-AUC — and explain why **accuracy alone lies**. | Argue why an arc and a spiral arm can look alike, and why that confusion is physically reasonable. |
| Build **TP / FP / FN** galleries and tie each failure back to the astrophysics. | Read a false positive as a real morphological ambiguity, not just a model bug. |

---

## Suggested Reading Order

Concept markdowns are numbered in reading order. Part 1 is files 01–02 (astronomy); Part 2 is files 03–04 (ML); the project task (05) ties everything together.

### Part 1 — Lensing Science

1. [`01-gravitational-lensing-101.md`](01-gravitational-lensing-101.md) — mass bends light; strong vs weak lensing; why lenses probe dark matter and the distant Universe.
2. [`02-strong-lens-morphologies.md`](02-strong-lens-morphologies.md) — Einstein rings, arcs, doubles/quads, and cluster lenses, with linked HST/Euclid imagery.

### Part 2 — CLIP Zero-Shot

3. [`03-vision-language-models-and-clip.md`](03-vision-language-models-and-clip.md) — what VLMs are, CLIP's shared image–text embedding space, and contrastive learning.
4. [`04-zero-shot-lens-finding.md`](04-zero-shot-lens-finding.md) — prompt design, cosine-similarity scoring, thresholds, and rare-class metrics (precision/recall/ROC).

### Project

5. [`05-project-task.md`](05-project-task.md) — the deliverable for this week, with acceptance criteria and stretch goals.

For deeper dives, see [`resources.md`](resources.md).

---

## Notebooks

Two notebooks live in [`notebooks/`](notebooks/), as one starter/solution pair. Both open directly in Colab.

| Part | Starter | Solution |
|------|---------|----------|
| CLIP lens hunter | [`week4_lens_clip_starter.ipynb`](notebooks/week4_lens_clip_starter.ipynb) | [`week4_lens_clip_solution.ipynb`](notebooks/week4_lens_clip_solution.ipynb) |

Unlike Weeks 1–3, this week does **not** reuse the Galaxy Zoo pipeline — it downloads the Euclid lens dataset straight from Hugging Face (`pip install datasets transformers` in the first cell). **Switch the runtime to GPU** before you start; embedding ~1,476 test images with CLIP is much faster on the T4. Always attempt the **starter** before opening the **solution**. To open in Colab: push your fork to GitHub and use **File → Open notebook → GitHub**, or download the `.ipynb` and use **File → Upload notebook**.

---

## Definition of Done

You've completed Week 4 when you can, without referring to the solution:

- [ ] Load the Euclid HF dataset and report the **label distribution** on both train and test.
- [ ] Explain **strong** vs **weak** lensing in two sentences.
- [ ] Run CLIP **zero-shot** scoring with at least two lens and two non-lens prompts.
- [ ] Plot an **ROC or precision-recall** curve and justify a threshold choice.
- [ ] Show at least one **false positive** where spiral or ring-like structure plausibly confused the model.
- [ ] State why **accuracy alone** is misleading for rare-object search, using your dataset's class balance as evidence.

---

## Common Pitfalls

| Symptom | Cause | Fix |
|---|---|---|
| `ModuleNotFoundError: datasets` | The HF libraries aren't preinstalled on Colab. | Run `pip install datasets transformers` in the first cell. |
| CUDA out-of-memory loading CLIP | Embedding all images at once on a T4. | Use `clip-vit-base-patch32` and embed in **chunks of 32–64**. |
| "99% accurate!" but the model is useless | Severe **class imbalance** — most cutouts are non-lenses. | Report **precision/recall/ROC-AUC**, never accuracy alone (see [`04`](04-zero-shot-lens-finding.md)). |
| CLIP fires on spiral galaxies | Curved spiral arms resemble lensing arcs. | This is a **feature, not a bug** — analyse it in the error gallery. |
| Calling it a "lenticular lens" | Mixing up gravitational **lens** with Hubble **lenticular (S0)**. | Different words, different physics — see [`01`](01-gravitational-lensing-101.md). |

---

## Where to Ask for Help

- Confused about strong vs weak lensing? Re-read [`01-gravitational-lensing-101.md`](01-gravitational-lensing-101.md).
- Not sure what CLIP is actually doing? See the shared-embedding diagram in [`03-vision-language-models-and-clip.md`](03-vision-language-models-and-clip.md).
- Scores look random / all the same? Check prompt design and the cosine-similarity normalisation in [`04-zero-shot-lens-finding.md`](04-zero-shot-lens-finding.md).
- Metrics confusing (high accuracy, low recall)? That's the class-imbalance lesson — re-read the rare-class section of [`04`](04-zero-shot-lens-finding.md).
- Still stuck? Drop a message in the CAIC mentor channel — that's exactly what mentors are for.

---

⬅️ Back to [track overview](../README.md) · Previous week: [Week 3](../Week-3/) · Next week: [Week 5](../Week-5/)
