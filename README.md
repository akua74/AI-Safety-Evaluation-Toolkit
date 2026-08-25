# AI-Safety-Evaluation-Toolkit


An open-source, deployment-aware toolkit that automates robustness, distribution-shift, uncertainty, and failure-mode testing for AI models before they're deployed in high-stakes settings.

## Motivation

Strong performance on a benchmark or held-out test set doesn't guarantee that an AI system will behave reliably once it meets real-world conditions. This project grew out of research on an AI model that detects surgical-site infections (SSIs) from post-caesarean wound images collected in Rwanda — specifically, how realistic changes in image quality (blur, brightness, contrast) affect model performance, and where an otherwise strong model becomes unreliable.

The goal of this repository is to generalize that project-specific pipeline into a reusable toolkit: given a trained model and an evaluation dataset, run a structured suite of safety tests and produce a standardized deployment-readiness report.

## Status

Early-stage, actively developed. The underlying robustness-evaluation pipeline (PyTorch, ResNet50-based) is built and has already surfaced a concrete failure mode:

- Under increasing blur, AUROC drops from ~0.85 to ~0.51 (indistinguishable from chance), while specificity collapses from ~0.71 to ~0.21 even though sensitivity holds close to baseline — the model doesn't start missing infections under blur, it starts flagging far more false positives.
- Brightness changes are comparatively well tolerated; contrast has a moderate but real effect.

This repository is where that pipeline is being generalized into a documented, reusable tool, using the SSI-detection model as the first test case.

## Roadmap

- [x] Reproduce and extend the SSI-detection pipeline (PyTorch, ResNet50); address class imbalance (SMOTE, class weighting, threshold selection)
- [x] Build the blur/brightness/contrast degradation-analysis pipeline and identify the blur failure mode above
- [ ] Generalize the pipeline into a documented, reusable module
- [ ] Add distribution-shift analysis
- [ ] Add uncertainty and calibration assessment
- [ ] Add automated failure-mode identification and performance-threshold analysis
- [ ] Generate standardized deployment-readiness reports
- [ ] Documentation, tutorials, and example workflows
- [ ] Case study applying the toolkit to a second real-world AI system

## Structure

```
.
├── notebooks/
│   └── POD11_Degradation_Analysis.ipynb   # reproduction + degradation-robustness pipeline
├── src/          # reusable toolkit modules, as they're generalized out of the notebook above
├── requirements.txt
└── LICENSE
```

## Setup

```bash
git clone <this-repo-url>
cd ai-safety-eval-toolkit
pip install -r requirements.txt
```

## License

MIT — see [LICENSE](LICENSE).
