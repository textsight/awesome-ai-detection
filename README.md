# Awesome AI Detection [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Tools, datasets, benchmarks and research for detecting AI-generated text — and for
> understanding where detection breaks down.

Most lists in this space are directories of commercial detectors. This one starts from the
**evidence**: the datasets you can measure a detector against, the papers that define what
"working" means, and the documented failure modes that decide whether a detector output is
safe to act on.

If you only take one thing from this page, take this: **every production AI detector has a
non-trivial false-positive rate on genuine human writing, and none of them produce proof of
misconduct.** The [failure modes](#known-failure-modes) section has the numbers.

**Contents**

- [Benchmarks and datasets](#benchmarks-and-datasets)
- [Research papers](#research-papers)
- [Open-source detectors](#open-source-detectors)
- [Commercial detectors](#commercial-detectors)
- [Adversarial tooling (humanizers)](#adversarial-tooling-humanizers)
- [Known failure modes](#known-failure-modes)
- [Related lists](#related-lists)
- [Contributing](#contributing)

---

## Benchmarks and datasets

The part of this field that actually settles arguments. If a detector claim is not measured
against something in this section, it is marketing.

- **[RAID](https://github.com/liamdugan/raid)** — the largest shared benchmark for robust
  evaluation of machine-generated text detectors. 6M+ generations across models, domains,
  decoding strategies and adversarial attacks. Start here.
  ([paper](https://arxiv.org/abs/2405.07940))
- **[M4 / M4GT-Bench](https://github.com/mbzuai-nlp/M4)** — multi-generator, multi-domain,
  multi-lingual machine-generated text detection. The basis of SemEval-2024 Task 8.
  ([paper](https://arxiv.org/abs/2402.11175))
- **[MGTBench](https://github.com/xinleihe/MGTBench)** — a benchmarking framework that runs
  many detection methods over a common set of tasks.
  ([paper](https://arxiv.org/abs/2303.14822))
- **[HC3](https://huggingface.co/datasets/Hello-SimpleAI/HC3)** — Human ChatGPT Comparison
  Corpus. One of the earliest paired human/model answer sets, and still a common baseline.
  ([paper](https://arxiv.org/abs/2301.07597))
- **[AI Detector False-Positive Benchmark](https://github.com/textsight/ai-detector-benchmarks)** —
  1,180 CC BY academic abstracts **published in 2018**, before GPT-3 existed, so every AI
  flag is a false positive by construction. Per-document scores, checksums and a
  zero-dependency reproduction script. Measures one detector (TextSight), not a field
  comparison. *Disclosure: maintained by the same people as this list — see
  [Contributing](#contributing).*

**Why pre-model corpora matter.** Most detection benchmarks need someone to label what is
machine-written. A corpus that predates the models needs no labels at all: if the text was
published in 2018, a flag is wrong, full stop. It is the cheapest honest measurement of a
false-positive rate available.

## Research papers

Verified links, first author, and year.

- **[GPT detectors are biased against non-native English writers](https://arxiv.org/abs/2304.02819)**
  — Liang et al., 2023. Found that GPT detectors misclassified TOEFL essays by non-native
  writers at a far higher rate than native writing. The single most important paper for
  anyone deploying detection in education.
- **[DetectGPT](https://arxiv.org/abs/2301.11305)** — Mitchell et al., 2023. Zero-shot
  detection using probability curvature; the paper that started the zero-shot line of work.
- **[Fast-DetectGPT](https://arxiv.org/abs/2310.05130)** — Bao et al., 2023. Conditional
  probability curvature; two orders of magnitude faster than DetectGPT.
- **[Ghostbuster](https://arxiv.org/abs/2305.15047)** — Verma et al., 2023. Structured search
  over features from weaker language models. UC Berkeley.
- **[Binoculars](https://arxiv.org/abs/2401.12070)** — Hans et al., 2024. Zero-shot detection
  by contrasting two closely related models. Strong accuracy without training data.
- **[RAID](https://arxiv.org/abs/2405.07940)** — Dugan et al., 2024. Shows how sharply
  detector accuracy degrades under adversarial perturbation and domain shift.
- **[M4GT-Bench](https://arxiv.org/abs/2402.11175)** — Wang et al., 2024. Black-box detection
  across generators, domains and languages.
- **[HC3 / ChatGPT comparison corpus](https://arxiv.org/abs/2301.07597)** — Guo et al., 2023.

## Open-source detectors

Run these yourself; no vendor in the loop.

- **[Binoculars](https://github.com/ahans30/Binoculars)** — zero-shot, no training data
  required, needs two open-weight models at inference.
- **[Fast-DetectGPT](https://github.com/baoguangsheng/fast-detect-gpt)** — the efficient
  successor to DetectGPT.
- **[DetectGPT](https://github.com/eric-mitchell/detect-gpt)** — reference implementation of
  the perturbation-curvature method.
- **[Ghostbuster](https://github.com/vivek3141/ghostbuster)** — code for the Berkeley paper;
  a hosted demo is at [ghostbuster.app](https://ghostbuster.app/).
- **[roberta-base-openai-detector](https://huggingface.co/openai-community/roberta-base-openai-detector)**
  — OpenAI's GPT-2 output detector. Historically important, and now a good demonstration of
  why detectors trained on one model generation do not transfer to the next.

> **Retired:** OpenAI withdrew its own *AI Text Classifier* on 2023-07-20, citing a "low rate
> of accuracy". By OpenAI's own published figures it identified only **26%** of AI-written text
> as likely AI-written, while flagging **9%** of human writing as AI. When the lab that trained
> the models cannot reliably detect their output, treat any vendor claiming near-perfect
> accuracy with corresponding suspicion.

## Commercial detectors

Descriptions state what each product is and who it targets. They are **not** accuracy
rankings — with the exception of the corpus linked above, none of these have been
independently tested by this list, and vendor-published accuracy figures are generally not
reproducible. Alphabetical.

| Tool | What it is |
|---|---|
| [Compilatio](https://www.compilatio.net/) | Plagiarism and AI detection for European education institutions |
| [Copyleaks](https://copyleaks.com/ai-content-detector) | Enterprise AI detection and plagiarism, with LMS integrations |
| [GPTKit](https://gptkit.ai/) | Multi-model detection reporting a combined score |
| [GPTZero](https://gptzero.me/) | The best-known standalone detector; education focus, per-sentence highlighting |
| [Grammarly](https://www.grammarly.com/ai-detector) | AI detection bundled into the writing assistant |
| [Hive Moderation](https://hivemoderation.com/ai-generated-content-detection) | Content-moderation API covering AI text and synthetic media |
| [Isgen](https://isgen.ai/) | Multilingual detection across a wide language set |
| [Originality.ai](https://originality.ai/ai-checker) | Aimed at publishers, SEO teams and content agencies |
| [Pangram Labs](https://www.pangramlabs.com/) | Enterprise detection; publishes its own evaluation work |
| [PlagiarismCheck / TraceGPT](https://plagiarismcheck.org/ai-detector/) | Detection with document reports for institutions |
| [QuillBot](https://quillbot.com/ai-content-detector) | Free detector attached to the paraphrasing suite |
| [Quetext](https://www.quetext.com/ai-detector) | Originality checking for academic and professional use |
| [Reality Defender](https://www.realitydefender.com/) | Deepfake and synthetic-media detection; enterprise and public sector |
| [isthisaigenerated.app](https://isthisaigenerated.app/site/ai-image-detector/) | Free warning-only detectors for images, sampled video, text and documents; multilingual (EN/NL/ID), publishes [measured slices and limits](https://isthisaigenerated.app/site/accuracy/). *Affiliated: the submitter maintains this tool* |
| [Sapling](https://sapling.ai/ai-content-detector) | Detector with an API, aimed at developers |
| [Scribbr](https://www.scribbr.com/ai-detector/) | Student-facing checker from the academic writing service |
| [Smodin](https://smodin.io/ai-content-detector) | Multilingual detection in a broader writing toolkit |
| [TextSight](https://textsight.ai/ai-detector/) | English-only transformer-based detector; publishes its [false-positive corpus](https://github.com/textsight/ai-detector-benchmarks). *Maintainer of this list — see [Contributing](#contributing)* |
| [Trinka](https://www.trinka.ai/ai-content-detector/) | Academic writing platform with AI detection |
| [Turnitin](https://www.turnitin.com/solutions/topics/ai-writing/ai-detector/) | The institutional incumbent; available to institutions, not individuals |
| [Undetectable AI](https://undetectable.ai/) | Detector paired with a rewriting product (see below) |
| [Winston AI](https://gowinston.ai/) | Multilingual detection with OCR and report generation |
| [Writer](https://writer.com/ai-content-detector/) | Free detector attached to the enterprise writing platform |
| [ZeroGPT](https://www.zerogpt.com/) | High-traffic free detector |

## Adversarial tooling (humanizers)

Rewriting tools that alter AI text so detectors score it as human. Most detector lists omit
this category. That is a mistake: **you cannot evaluate a detector without knowing what it is
being attacked with**, and [RAID](https://arxiv.org/abs/2405.07940) shows adversarial
perturbation is where accuracy claims collapse.

- **[Undetectable AI](https://undetectable.ai/)** — rewriting plus a bundled detector.
- **[TextSight Humanizer](https://textsight.ai/ai-humanizer/)** — rewriting with the
  detector score shown before and after. *Maintainer of this list.*

**A note on the word "undetectable".** No rewriting tool can guarantee a given detector's
verdict, because detectors change without notice and none of these tools have access to the
target model. Treat any guarantee of undetectability as unsupportable. Note also that
evading detection in an academic submission is misconduct at most institutions regardless of
which tool produced the text.

## Known failure modes

The section that matters if a detector output is about to affect someone's grade, job or
publication.

**1. False positives on genuine human writing are common.**
Measured on 1,180 academic abstracts published in 2018 — before GPT-3 existed, so every flag
is provably wrong — one detector produced a **5.85% false-positive rate** (95% CI 4.65–7.34),
with a further **20% landing in "uncertain"**.
[Data and method.](https://github.com/textsight/ai-detector-benchmarks)

**2. Bias against non-native English writers is documented.**
[Liang et al. (2023)](https://arxiv.org/abs/2304.02819) found GPT detectors misclassified
non-native TOEFL essays far more often than native writing. This is the single most-cited
risk in educational deployment. It is detector-specific and must be measured per detector,
not assumed either way — the corpus above tested for it on one detector and returned a
**null result**, which is not the same as showing the bias is absent.

**3. Short text is unreliable.**
Detectors need signal. Under roughly 200–300 words, confidence intervals widen sharply and
most vendors say so in their own documentation.

**4. Accuracy collapses under adversarial pressure.**
[RAID](https://arxiv.org/abs/2405.07940) demonstrates large accuracy drops from paraphrasing,
homoglyph substitution and other cheap perturbations.

**5. Detectors do not transfer across model generations.**
A detector trained on GPT-2 output does not reliably catch GPT-4-class output. Every
published accuracy figure has an implicit expiry date.

**6. A detector score is evidence, not proof.**
No output here establishes misconduct. Institutional guidance — including
[Turnitin's own](https://www.turnitin.com/solutions/topics/ai-writing/ai-detector/) — is that
scores open a conversation with the writer; they do not close one.

## Related lists

- **[awesome-ai-spotter](https://github.com/oskar-j/awesome-ai-spotter)** — a curated list of
  AI countermeasures and detection tools.
- **[ai-detected/ai-content-detectors](https://github.com/ai-detected/ai-content-detectors)** —
  a large A–Z alphabetical directory of commercial text detectors.

## Contributing

Pull requests welcome. Please:

1. **One tool or paper per PR**, with a link that resolves.
2. **Describe what it is, not how good it is.** Accuracy claims need a citation to a
   reproducible evaluation, not a vendor page.
3. **Papers need a stable identifier** — arXiv, DOI or ACL Anthology.
4. **Disclose any affiliation** with what you are adding. It will still be accepted; it just
   gets labelled.

**Maintainer disclosure.** This list is maintained by the team behind
[TextSight](https://textsight.ai), which builds an AI detector and a humanizer. Both are
listed above and both are labelled. Competing products are listed on the same terms and none
of them have been ranked or scored. The one product claim made anywhere on this page —
the false-positive rate — is about our own detector, and the corpus, per-document scores and
analysis code are [published in full](https://github.com/textsight/ai-detector-benchmarks) so
that it can be checked, including the results that do not flatter us.

## Licence

[CC0 1.0](LICENSE) — public domain. Take it, fork it, no attribution required.
