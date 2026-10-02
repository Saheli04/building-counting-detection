# Building Detection and Counting Dataset

This project uses aerial imagery and corresponding building-label masks for building detection and counting.

The dataset consists of aerial images along with binary segmentation masks identifying building regions. The images are used to train a deep learning segmentation model that detects buildings and subsequently estimates the number of buildings in an image.

## Dataset Link: https://drive.google.com/drive/folders/11AD6MWt0qLFoTW2IbXGpDA1n77GpOpOr?usp=drive_link

---

## Dataset Structure

The dataset is organized into training, validation, and testing sets:

```text
dataset/
├── tiff/
│   ├── train/
│   ├── train_labels/
│   ├── val/
│   ├── val_labels/
│   ├── test/
│   └── test_labels/
````

### Folders

| Folder          | Description                                       |
| --------------- | ------------------------------------------------- |
| `train/`        | Training aerial images                            |
| `train_labels/` | Building masks corresponding to training images   |
| `val/`          | Validation aerial images                          |
| `val_labels/`   | Building masks corresponding to validation images |
| `test/`         | Test aerial images                                |
| `test_labels/`  | Ground-truth masks for test images                |

---

## Image Information

The aerial images used in the project are approximately:

* **Image size:** 1500 × 1500 pixels
* **Channels:** 3-channel RGB
* **Image format:** TIFF (`.tif` / `.tiff`)

For model training, images are processed into smaller **512 × 512 pixel crops**.

---

## Label Masks

The corresponding label images contain building annotations.

The masks are converted into binary segmentation masks:

| Pixel Value | Meaning    |
| ----------: | ---------- |
|         `0` | Background |
|       `255` | Building   |

During preprocessing, these masks are converted into binary values suitable for model training.

---

## Dataset Preparation

The preprocessing workflow includes:

1. Loading aerial images and corresponding building masks.
2. Matching each image with its corresponding label.
3. Converting images to RGB format.
4. Normalizing image pixel values.
5. Converting building labels into binary masks.
6. Creating training crops of size `512 × 512`.
7. Separating the data into training, validation, and test sets.

---

## Model Input

The segmentation model receives:

```text
Input Image
    ↓
3-channel RGB aerial image
    ↓
512 × 512 training crop
    ↓
U-Net + ResNet34
```

The model produces a pixel-level probability map indicating the likelihood that each pixel belongs to a building.

---

## Building Counting

After segmentation, the predicted building mask is processed to estimate the number of individual buildings.

The counting pipeline uses:

```text
Predicted Probability Map
        ↓
Probability Threshold
        ↓
Binary Building Mask
        ↓
Connected Component Analysis
        ↓
Building Count
```

The final configuration used in the project applies:

* **Probability threshold:** `0.60`
* **Minimum component area:** `5 pixels`

Connected components are used to identify individual detected building regions.

---

## Dataset Usage in This Project

The dataset is used for:

* Building segmentation
* Building detection
* Building counting
* Model evaluation
* Visualization of predicted building masks
* Comparison between predicted and ground-truth building counts

The project uses a **U-Net architecture with a ResNet34 encoder** for semantic segmentation.

---

## Dataset Size Used

The project training configuration used:

* **Training samples:** 1,050
* **Validation samples:** 20
* **Test images evaluated:** 10

Training was performed using image crops rather than processing the complete 1500 × 1500 images directly.

---

## Data Availability

The raw aerial-image dataset is **not included in this GitHub repository** because image datasets can be large.

The repository contains the notebook, documentation, and selected visualization results required to understand and reproduce the project workflow.

The dataset should be obtained from its original source and arranged according to the folder structure described above before running the notebook.

---

## Important Note

The building-counting results depend on image resolution, building density, segmentation quality, probability threshold, and connected-component parameters.

The counting approach is therefore intended as a machine learning research/portfolio project rather than a production-level building inventory system.

````

