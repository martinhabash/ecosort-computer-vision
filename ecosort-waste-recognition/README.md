# EcoSort — Waste Recognition and Sorting Assistant

**Author:** Martin Habash
**Course:** ITAI 1378 — Computer Vision and AI
**Checkpoint:** Midterm Project Proposal — The Blueprint (individual)
**Status:** Proposal / planning stage. No code has been written, no model has been trained, and no results exist yet.

Proposal slides: [`docs/proposal.pdf`](docs/proposal.pdf)

## Tier

**Tier 1.** The project adapts one well-documented pretrained model (MobileNetV2) to a small public dataset with free tools, which is achievable for one student in one term.

## Problem

People often have to guess which bin a single item belongs in, because many everyday items look recyclable but are not, or belong in a different stream than expected. Items placed in the wrong bin can contaminate a recycling stream and make recycling less effective. The target users are households, students, and anyone who wants basic guidance when sorting common waste items.

## Solution

EcoSort is a small app that classifies a photo of a single waste item into one of six material categories and shows a basic sorting suggestion. The user uploads a photo; the model predicts a category and a confidence score; the app shows a fixed hint for that category and reminds the user that local recycling rules differ.

```
Waste-item photo  →  Classification model (MobileNetV2)  →  Predicted category + confidence  →  Sorting suggestion
```

EcoSort is for educational use and preliminary sorting only. It does not claim that every item in a predicted category is accepted by every local program. Photos should be taken safely, away from traffic and other hazards.

**Out of scope:** real-time video, smart-bin hardware or robotics, municipal or production deployment.

## Technical approach

| Item | Choice |
|---|---|
| Technique | Image classification (one label per photo; each photo shows one item) |
| Model | MobileNetV2 with transfer learning: ImageNet weights, frozen base, new head of global average pooling + dense softmax layer with 6 outputs; top layers may be fine-tuned later |
| Input size | 224 × 224 × 3 |
| Language / framework | Python, TensorFlow/Keras (MobileNetV2 from Keras Applications) |
| Environment | Google Colab (free tier, subject to usage limits) |
| Interface | Streamlit image-upload page, if feasible; a notebook demo is the fallback |

Reference: Sandler, M., Howard, A., Zhu, M., Zhmoginov, A., & Chen, L.-C. (2018). *MobileNetV2: Inverted Residuals and Linear Bottlenecks.* CVPR 2018.

## Data plan

- **Source:** TrashNet, <https://github.com/garythung/trashnet> (Thung & Yang, Stanford CS 229, 2016; repository under the MIT License).
- **Size:** 2,527 JPEG images at 512 × 384 px (verified by counting the files in `data/dataset-resized.zip` from the repository on 2026-10-12; matches the repository README).
- **Labels (6):** cardboard (403), glass (501), metal (410), paper (594), plastic (482), trash (137).
- **Split:** stratified 70 / 15 / 15 into train, validation, and held-out test sets with a fixed random seed. The test set is used once, at the end.
- **Limitations:** every photo shows one item on a white posterboard under sunlight or room lighting, so real-world photos with cluttered backgrounds may perform worse. The trash class is small (137 images, about 5%).

Details: [`data/README.md`](data/README.md)

## Success metrics (proposed targets, not results)

| Metric | Target | How it will be measured |
|---|---|---|
| Primary: accuracy | ≥ 85% on the held-out test set | Correct predictions ÷ total test images on the 15% split never used for training or tuning; a confusion matrix will show which classes are confused |
| Secondary: prediction time | < 2 seconds per image | Average time from loaded image to displayed prediction over the test images, in the chosen environment (Colab runtime or Streamlit app), with the hardware recorded |

For context, the TrashNet authors report about 75% test accuracy with their own CNN trained from scratch (repository README). The 85% target assumes transfer learning improves on that; it is a goal, not a result.

## Milestone plan

My class follows the 16-week term.

| # | Phase | Week | Goal | Checkpoint |
|---|---|---|---|---|
| 1 | Blueprint | Week 10 | Define the problem, data, metrics, and plan | Proposal PDF; repo with README, data README, and AI usage log |
| 2 | First Working Demo | Week 11 | Run a pretrained MobileNetV2 end-to-end on a few sample photos, before any dataset work or training | A prediction for each sample photo |
| 3 | Make It Yours | Weeks 12–13 | Load TrashNet, split it, and train the six-class head | Model predicts the six classes; validation accuracy logged |
| 4 | Improve and Measure | Week 14 | Add augmentation and fine-tuning; evaluate once on the held-out test set | Accuracy, timing, confusion matrix, and failure cases recorded against targets |
| 5 | Package and Present | Week 15 | Build the upload app and finish documentation | Streamlit demo, demo video, final repo, and slides |

## Risks and Plan B

| Risk | Plan B |
|---|---|
| 1. Weak accuracy on real photos: blurry images, unusual angles, or backgrounds unlike TrashNet's white posterboard | Stay with the six supported classes, add augmentation (rotation, brightness, cropping), and document wrong predictions honestly |
| 2. Setup, training time, or Colab compute limits slow development | Use smaller images (e.g., 160 × 160), a frozen base, and a smaller verified subset; fall back to a notebook demo if the app runs late |

## Resources and cost

Python, TensorFlow/Keras, Google Colab, the TrashNet dataset, and Streamlit (if feasible). **Estimated cost: $0**, assuming free tools and free Colab compute within its usage limits.

## Repository structure

```
ecosort-waste-recognition/
├── README.md            ← this file
├── data/
│   └── README.md        ← dataset source, labels, split plan (images are not committed)
└── docs/
    ├── proposal.pdf     ← Midterm Blueprint slides
    └── AI_usage_log.md  ← AI assistance log (started at the Blueprint stage)
```

`notebooks/`, `src/`, and `results/` will be added when implementation begins. No results are included at this stage.

## AI use

AI assistance is logged in [`docs/AI_usage_log.md`](docs/AI_usage_log.md).
