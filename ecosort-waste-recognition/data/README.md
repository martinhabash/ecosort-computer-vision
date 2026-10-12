# Data — TrashNet

EcoSort plans to use the **TrashNet** dataset. No images are stored in this repository.

## Source

- Repository: <https://github.com/garythung/trashnet>
- Authors: Gary Thung and Mindy Yang, collected for Stanford's CS 229 (Machine Learning) final project, 2016.
- Paper: Thung, G., & Yang, M. (2016). *Classification of Trash for Recyclability Status.* <https://cs229.stanford.edu/proj2016/report/ThungYang-ClassificationOfTrashForRecyclabilityStatus-report.pdf>
- The authors ask users of the dataset to cite the repository.

## What the data is

- **2,527 JPEG images**, resized to **512 × 384 px**.
- Each photo shows one item placed on a white posterboard, lit by sunlight and/or room lighting, taken with iPhone 7 Plus, iPhone 5S, and iPhone SE (per the repository README).

| Label | Images |
|---|---|
| cardboard | 403 |
| glass | 501 |
| metal | 410 |
| paper | 594 |
| plastic | 482 |
| trash | 137 |
| **Total** | **2,527** |

These counts were verified on 2026-10-12 by extracting `data/dataset-resized.zip` from the repository and counting the `.jpg` files in each class folder (a macOS `__MACOSX` metadata file in the archive was excluded). They match the repository README.

## Labels

The label of each image is the name of the folder it sits in (`cardboard/`, `glass/`, `metal/`, `paper/`, `plastic/`, `trash/`). No additional labeling is planned.

## How to obtain it

1. Clone the repository: `git clone https://github.com/garythung/trashnet`
2. Unzip `trashnet/data/dataset-resized.zip` (about 43 MB).
3. Place the extracted `dataset-resized/` folder in `data/raw/` locally or in Colab. `data/raw/` is not committed.

The repository README also points to a Hugging Face copy of the dataset (`huggingface.co/datasets/garythung/trashnet`). That copy was not checked for this proposal.

## Why the data is needed

The pretrained MobileNetV2 model knows ImageNet's 1,000 categories, not waste materials. TrashNet's labeled photos are needed to train a new six-class output layer (and optionally fine-tune top layers) and to measure accuracy on images the model has not seen.

## Planned train / validation / test split

- **Stratified 70 / 15 / 15** split by class, with a fixed random seed so the split can be reproduced.
- **Train (70%)**: fit the new classification head (and later, fine-tuned layers).
- **Validation (15%)**: choose settings such as learning rate, augmentation, and number of epochs.
- **Test (15%)**: held out and used **once**, at the end, to report final accuracy and timing.

Held-out testing is necessary because accuracy on images used for training or tuning overstates how well the model handles new photos. The split will be saved as file lists so the same test images are used in every report.

## Limitations

- All photos use a plain white background and similar lighting; real user photos with clutter, shadows, or different lighting may be classified less accurately.
- The **trash** class has only 137 images (about 5% of the data), so it may be predicted less reliably than the others.
- 2,527 images is small for deep learning, which is one reason for using transfer learning from a pretrained model.
- Category labels describe material, not whether a specific local program accepts an item.

## Licensing and usage terms to check

- The TrashNet GitHub repository is published under the **MIT License** (`LICENSE` file, copyright 2017 Gary Thung).
- Before publishing results, confirm the dataset terms on the Hugging Face dataset card as well, and cite the repository as the authors request.
- The full dataset does **not** need to be committed to this repository; add `data/raw/` to `.gitignore`.
