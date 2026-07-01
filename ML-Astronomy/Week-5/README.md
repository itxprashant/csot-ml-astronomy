# Week 5 — VLM Interpreter and the CNN vs CLIP vs VLM Capstone

> The final week, and the payoff. In Week 4 CLIP gave us a fast *score* — a number — but no explanation. This week we hand the same cutouts to a **generative vision–language model** and ask it to *talk*: "Is this a lens? What do you see? What could fool you?" Then we do the thing that makes this a real science project — put all three tools on trial. The **Week-3 morphology CNN**, the **Week-4 CLIP ranker**, and the **Week-5 VLM** face the same images, and you decide who to trust, where each breaks, and what a real survey would send to a human. You will also watch a VLM confidently hallucinate an Einstein ring that isn't there.

This week is split into **two parts**:

- **Part 1 — VLM as interpreter:** what a generative VLM adds over CLIP, how to write a fixed, reproducible prompt that returns a structured verdict, and how VLMs *hallucinate* — inventing arcs and rings — so that confidence never equals correctness.
- **Part 2 — The three-way capstone:** the physics of why arcs and spirals confuse everyone, then a head-to-head audit of CNN vs CLIP vs VLM on the same test cutouts, ending in a written, science-aware trust/reflection.

This is the **capstone of the whole track**. You are not learning a new model family so much as learning to *judge* models — the skill that outlives any one architecture.

---

## At a Glance

- **Time estimate:** 4–6 hours.
- **Prerequisites:** Finish [Week 1](../Week-1/) through [Week 4](../Week-4/) first. You specifically need your Week-3 `galaxy_model.pth` (or the ability to retrain it) and your Week-4 CLIP scoring code — the capstone reuses both.
- **Primary dataset:** the same [`mwalmsley/euclid_strong_lens_expert_judges`](https://huggingface.co/datasets/mwalmsley/euclid_strong_lens_expert_judges) test split as Week 4, down-sampled to a **stratified 30–50 cutouts** (lenses + hard negatives) so the VLM calls stay cheap.
- **Secondary dataset:** [`Arbie333/gravitational_lensing`](https://huggingface.co/datasets/Arbie333/gravitational_lensing) — 40 image+caption pairs, used as "good description" reference examples.
- **Primary model (default):** an **open VLM on Colab — `Qwen/Qwen2-VL-2B-Instruct`** via `transformers`. No API key, runs on a T4. A hosted-API path (Gemini free tier / GPT-4o) is documented as an optional fallback.
- **Deliverable:** One completed Colab notebook — `week5_lens_vlm_starter.ipynb` — that prompts a VLM on 30–50 cutouts, scores it against ground truth, reloads the Week-3 CNN and Week-4 CLIP on the *same* images, and produces a single three-way comparison figure plus a ~300-word capstone reflection.

---

## Learning Objectives

| ML / Computer Science | Astronomy / Physics |
|-----------------------|---------------------|
| Explain what a **generative VLM** adds over CLIP — natural-language *evidence*, not just a similarity score. | Explain why an arc and a spiral arm look alike, and the concentric-around-a-foreground-galaxy tell that separates them. |
| Write a **fixed prompt** that returns a structured `VERDICT / EVIDENCE / CAUTION` answer and parse it. | Name the non-lens decoys (spiral arms, merger tails, ring galaxies, dust lanes) and why each fools an observer. |
| Control **temperature** and understand **reproducibility** limits of generative models. | Connect automated lens finding to real survey workflows: candidate ranking then human confirmation. |
| Map `yes/no/uncertain` verdicts to binary and compute **precision/recall** against ground truth. | Explain why rare-object pipelines optimise **precision** at some recall cost, given Rubin/LSST scale. |
| Identify and document **hallucination** — a model describing features that are not present. | Argue why "confidently wrong" is more dangerous in science than "uncertain". |
| Run the **Week-3 CNN** on Euclid cutouts and interpret its mispredictions as a **wrong-task** lesson. | Read a false positive as a genuine morphological ambiguity, not merely a bug. |
| Build a **three-way comparison** (CNN vs CLIP vs VLM) on identical images and reason about tool–task fit. | Judge *when* to trust automation and *what* to escalate to human experts / citizen science. |

---

## Suggested Reading Order

Concept markdowns are numbered in reading order. Part 1 is files 01–02; Part 2 is files 03–04; the capstone task (05) ties everything together.

### Part 1 — VLM as Interpreter

1. [`01-vlm-prompting-for-science.md`](01-vlm-prompting-for-science.md) — generative VLMs vs CLIP, the fixed `VERDICT/EVIDENCE/CAUTION` prompt, temperature, and reproducibility.
2. [`02-hallucination-and-human-in-the-loop.md`](02-hallucination-and-human-in-the-loop.md) — how VLMs invent rings, why confidence isn't correctness, and where humans re-enter the loop.

### Part 2 — The Three-Way Capstone

3. [`03-arcs-vs-spirals-confusion-physics.md`](03-arcs-vs-spirals-confusion-physics.md) — the astrophysics of lookalikes: arcs, spiral arms, tidal tails, and ring galaxies.
4. [`04-comparing-cnn-clip-and-vlm.md`](04-comparing-cnn-clip-and-vlm.md) — task mismatch, prompt-sensitivity, and hallucination side by side: which tool for which job.

### Capstone

5. [`05-project-task.md`](05-project-task.md) — the final deliverable, with acceptance criteria, a presentation template, and stretch goals.

For deeper dives, see [`resources.md`](resources.md).

---

## Notebooks

Two notebooks live in [`notebooks/`](notebooks/), as one starter/solution pair. Both open directly in Colab.

| Part | Starter | Solution |
|------|---------|----------|
| VLM audit + capstone | [`week5_lens_vlm_starter.ipynb`](notebooks/week5_lens_vlm_starter.ipynb) | [`week5_lens_vlm_solution.ipynb`](notebooks/week5_lens_vlm_solution.ipynb) |

This notebook pulls three threads together, so have your **Week-3 `galaxy_model.pth`** and **Week-4 CLIP scoring cells** handy — the solution includes fallback setup for both. The default VLM (`Qwen/Qwen2-VL-2B-Instruct`) runs entirely on the Colab **T4 GPU** with no API key; a commented hosted-API cell is provided if you prefer Gemini/GPT-4o. Always attempt the **starter** before opening the **solution**. To open in Colab: push your fork to GitHub and use **File → Open notebook → GitHub**, or download the `.ipynb` and use **File → Upload notebook**.

---

## Definition of Done

You've completed Week 5 — and the track — when you can, without referring to the solution:

- [ ] Run a VLM on **≥30** test cutouts with a **documented, fixed** prompt and parse its verdicts.
- [ ] Give at least **two** concrete examples of VLM **hallucination or overconfidence**.
- [ ] Compute the VLM's **precision/recall** against ground truth.
- [ ] Compare **CNN, CLIP, and VLM** on the **same** images in one figure or table.
- [ ] Explain why the **morphology CNN is the wrong model** for lens finding (wrong task *and* domain shift).
- [ ] Write a short **capstone reflection** on precision, human review, and failure modes.

---

## Common Pitfalls

| Symptom | Cause | Fix |
|---|---|---|
| VLM output won't parse | Free-form prose instead of the fixed format. | Pin the `VERDICT / EVIDENCE / CAUTION` template and parse the `VERDICT:` line (see [`01`](01-vlm-prompting-for-science.md)). |
| Different answer every run | Non-zero temperature / sampling. | Set a low temperature (or greedy decoding) and fix seeds where possible; note residual nondeterminism. |
| VLM always says "yes" | Leading prompt or over-eager model. | Neutral wording; measure precision/recall, don't trust the vibe (see [`02`](02-hallucination-and-human-in-the-loop.md)). |
| CNN "does badly" on lenses | It was trained on Galaxy Zoo morphology, not lenses, on SDSS not Euclid. | **Expected** — that's the wrong-task/domain-shift lesson, not a bug ([`04`](04-comparing-cnn-clip-and-vlm.md)). |
| Qwen2-VL out-of-memory on T4 | Loading in full precision or too many images at once. | Load in half precision (`torch_dtype`), process one image per call, keep the sample at 30–50. |

---

## Where to Ask for Help

- Prompt won't return a clean verdict? See the template and parsing notes in [`01-vlm-prompting-for-science.md`](01-vlm-prompting-for-science.md).
- Not sure whether a VLM claim is a hallucination? Re-read [`02-hallucination-and-human-in-the-loop.md`](02-hallucination-and-human-in-the-loop.md) and check the image against the physics in [`03`](03-arcs-vs-spirals-confusion-physics.md).
- Confused why the CNN is even in the comparison? That's the point of [`04-comparing-cnn-clip-and-vlm.md`](04-comparing-cnn-clip-and-vlm.md).
- Model loading / OOM trouble on Colab? See the pitfalls table above and the notebook's setup cell.
- Still stuck? Drop a message in the CAIC mentor channel — that's exactly what mentors are for.

---

⬅️ Back to [track overview](../README.md) · Previous week: [Week 4](../Week-4/) · You've reached the end of the track — nice work.
