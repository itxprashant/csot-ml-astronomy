# Week 5 — Curated Resources

> The links in each numbered concept markdown are the *minimum* set you need. This page gathers extras for the curious, organised by theme. Nothing here is required; everything here is good. This is the last week — a few of these are worth keeping for after the track.

---

## Vision–Language Models & Prompting (Part 1)

### Models
- 📘 [Qwen2-VL-2B-Instruct model card (default VLM)](https://huggingface.co/Qwen/Qwen2-VL-2B-Instruct) and the [Qwen2-VL blog](https://qwenlm.github.io/blog/qwen2-vl/).
- 📘 [LLaVA project page](https://llava-vl.github.io/) — a widely used open VLM.
- 📘 [Hugging Face — image-text-to-text task guide](https://huggingface.co/docs/transformers/en/tasks/image_text_to_text).
- 📘 [Google AI Studio — Gemini API (free tier)](https://aistudio.google.com/) — the hosted-API fallback.

### Prompting
- 📘 [Prompting Guide (multimodal basics)](https://www.promptingguide.ai/).
- 📺 [Temperature in language models — short explainer](https://www.youtube.com/watch?v=Vpr4mQMbFOo).
- 📘 [Hugging Face — text generation strategies (greedy vs sampling)](https://huggingface.co/docs/transformers/en/generation_strategies).

---

## Hallucination & Human-in-the-Loop (Part 1)

- 📘 [Google Cloud — what are AI hallucinations?](https://cloud.google.com/discover/what-are-ai-hallucinations).
- 🌌 [Space Warps (Zooniverse)](https://www.zooniverse.org/projects/aprajita/space-warps) — citizen-science lens vetting.
- 📄 [Marshall et al. 2016 — Space Warps: crowd-sourced discovery (arXiv)](https://arxiv.org/abs/1504.06148).
- 📄 [Metcalf et al. 2019 — Strong Gravitational Lens Finding Challenge (arXiv)](https://arxiv.org/abs/1802.03609).
- 📘 [Rubin Observatory / LSST — survey scale](https://www.lsst.org/about) and [ESA Euclid](https://www.esa.int/Science_Exploration/Space_Science/Euclid).

---

## Arcs vs Spirals — the Physics of Confusion (Part 2)

- 🖼️ [ESA/Hubble — gravitational lensing image archive](https://esahubble.org/images/archive/category/gravitationallensing/).
- 🖼️ [ESA/Hubble — the Antennae Galaxies (tidal tails)](https://esahubble.org/images/heic0812c/).
- 📘 [Wikipedia — Ring galaxy](https://en.wikipedia.org/wiki/Ring_galaxy) and [Hoag's Object](https://en.wikipedia.org/wiki/Hoag%27s_Object).
- 📘 [CASTLES survey of lenses](https://lweb.cfa.harvard.edu/castles/) — compare real arcs to spiral lookalikes.
- 📘 [Week 4 — strong-lens morphologies](../Week-4/02-strong-lens-morphologies.md).

---

## Comparing Models & the Bigger Picture (Part 2)

- 📄 [Huertas-Company & Lanusse 2023 — Deep Learning for Galaxy Surveys (arXiv)](https://arxiv.org/abs/2210.01813).
- 📄 [Radford et al. 2021 — CLIP (arXiv:2103.00020)](https://arxiv.org/abs/2103.00020).
- 📘 [Domain adaptation / distribution shift (overview)](https://en.wikipedia.org/wiki/Domain_adaptation).
- 📘 [Week 3 — building the CNN](../Week-3/02-building-a-cnn.md) and [Week 4 — zero-shot lens finding](../Week-4/04-zero-shot-lens-finding.md).

---

## Datasets

- 📘 [`mwalmsley/euclid_strong_lens_expert_judges`](https://huggingface.co/datasets/mwalmsley/euclid_strong_lens_expert_judges) — the test split reused this week.
- 📘 [`Arbie333/gravitational_lensing`](https://huggingface.co/datasets/Arbie333/gravitational_lensing) — 40 image+caption pairs, "good description" examples.
- 📘 [`juliensimon/gravitational-lenses`](https://huggingface.co/datasets/juliensimon/gravitational-lenses) — tabular lens catalog (reading only; census scale).

---

## Communities

- 💬 [Hugging Face forums](https://discuss.huggingface.co/) — `transformers` VLM help.
- 💬 [PyTorch discussion forums](https://discuss.pytorch.org/).
- 💬 [Astronomy Stack Exchange](https://astronomy.stackexchange.com/).
- 💬 The official **CAIC CSoT channels** — your first stop for track-specific questions and mentor help.

---

## How to Use This List

You will not read all of this. Suggested workflow for Week 5:

1. **Part 1:** skim the Qwen2-VL model card and one temperature/generation explainer, then read about AI hallucinations before you open the notebook.
2. **Build** the VLM audit from the starter before reading more.
3. **Part 2:** compare a few real arcs vs spirals in the ESA/Hubble archive so your failure analysis is grounded in physics.
4. **Capstone:** read (or skim) the Lens Finding Challenge and the Huertas-Company & Lanusse review to see how professionals frame exactly the problem you just tackled.
5. **After the track:** explore Space Warps and classify some real candidates — you now understand the pipeline you'd be part of.

---

## A Note on Finishing the Track

You started at Week 1 not knowing what a tensor was. You now can: build and train a CNN, evaluate it honestly, hunt rare objects zero-shot with a multimodal model, and *critically audit* a generative AI's scientific claims. That last skill — knowing when to trust a model and when to call a human — is the one that will outlast every specific tool here. Well done.

⬅️ Back to [Week 5 README](README.md)
