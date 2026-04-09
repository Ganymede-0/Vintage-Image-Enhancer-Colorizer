# Adaptive Pipeline for Vintage Photo Restoration
> **A multi-stage image restoration and colorization pipeline combining classical computer vision and deep learning.**

## Project Summary
Vintage photographs frequently suffer from degradation like fading, scratches, and low resolution. This project aims to recover these lost details through a modular pipeline. Instead of relying purely on a neural network to do all the heavy lifting, this project introduces a **quality-aware adaptive preprocessing stage** to correct structure and contrast *before* passing the image to a Deep Learning colorization model (DeOldify). 

## Architecture & Approaches

The project investigates two distinct approaches to restoration, establishing a baseline and an optimized adaptive pipeline.

### Approach 1: The Baseline Pipeline
A straightforward approach testing the raw capabilities of the colorization model.
* **Colorization:** Direct application of the DeOldify model (U-Net Generator with NoGAN training) on raw grayscale inputs.
* **Post-processing:** Application of **Unsharp Masking** (subtracting a Gaussian-blurred version of the image from the original) to forcefully retrieve subtle details post-colorization.

### Approach 2: The Adaptive Quality-Aware Pipeline (Primary Contribution)
A robust, three-stage pipeline that categorizes images by their degradation level and applies customized computer vision techniques.

**1. Adaptive Pre-processing**
Images are manually categorized into High, Medium, and Low quality. Each category triggers a specific cv2 enhancement route:
* **High Quality:** Minimal intervention. Light Non-Local Means (NLM) denoising (`h=5`) to remove minor artifacting without destroying textures.
* **Medium Quality:** Moderate NLM denoising (`h=7`) followed by **CLAHE** (Contrast Limited Adaptive Histogram Equalization) applied to the luminance channel in LAB color space to dynamically enhance contrast.
* **Low Quality:** Aggressive NLM denoising (`h=6`), followed by a custom **Laplacian sharpening kernel** `[[0, -0.2, 0], [-0.2, 2, -0.2], [0, -0.2, 0]]` to rebuild lost edges, and finally CLAHE for structural contrast.

**2. Deep Learning Colorization**
* The pre-processed, structurally sound images are fed into the PyTorch/FastAI **DeOldify** model, rendering realistic and naturally blended colors.

**3. Post-processing**
* A light **Gaussian Blur (5x5 kernel)** is applied to smooth out any residual noise or artificial harshness introduced during the colorization of heavily degraded images.

## 📈 Evaluation & Metrics
The results were evaluated using standard perceptual and structural metrics to compare the colorized outputs against the structural integrity of the originals.

| Metric | Score    | Interpretation                               |
| ------ | -------- | -------------------------------------------- |
| PSNR   | 30.98 dB | Excellent luminance preservation             |
| SSIM   | 0.9838   | Very high structural similarity              |
| LPIPS  | 0.0918   | Low perceptual difference (visually natural) |

*Note: A human evaluation survey was also conducted to validate the visual appeal and realism of the GAN-generated results.*

## Dataset
The pipeline was tested on 29 grayscale vintage photos sourced from the "Old Photos Dataset" on Kaggle, which were manually segmented by visual degradation levels to test the adaptive architecture.

Link: https://www.kaggle.com/datasets/marcinrutecki/old-photos

## Key Takeaways
- **Pre-processing matters:** Feeding raw, noisy images into a GAN often results in hallucinated artifacts. The adaptive pipeline proved that classical CV pre-processing significantly stabilizes deep learning colorization.
- **Context is key:** Blanket applying CLAHE or sharpening to all images ruins high-quality inputs. The tier-based routing ensures the right mathematical operations are applied to the right images.

## Visual Examples

![old_photo_03](https://github.com/user-attachments/assets/e3f537a8-de7a-4ee6-b282-965da2909868)

![processed_colorized_Low_old_photo_03](https://github.com/user-attachments/assets/47cd66f4-94bc-48c2-aea3-69e780c8d5a6)

![old_photo_28](https://github.com/user-attachments/assets/3c014843-d515-4311-b605-6a9a7dee049f)

![processed_colorized_Low_old_photo_28](https://github.com/user-attachments/assets/abdf741d-7cec-45b3-b67b-9a0dcea26c61)



  
