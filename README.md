Restoration and Colorization of Vintage Photographs using Image Processing and Deep Learning
--------

This project presents a full image restoration and colorization pipeline for degraded black-and-white vintage photographs. Through the use of classical image processing and the DeOldify deep learning model, our pipeline enhances old photos by removing noise, correcting contrast, sharpening details, and generating vivid, realistic colors.

--------

Project Summary
Vintage photos often suffer from fading, scratches, and poor resolution due to age. Our goal was to build a smart and modular image pipeline that:
- Restores clarity and structure.
- Prepares the image for high-quality deep learning colorization.
- Applies post-processing to improve the final output.

--------

Techniques Used:

1. Pre-processing (We used 2 approaches)
   
Photos were divided into Low, Medium, and High quality categories. Each group received customized enhancement using:
- CLAHE (contrast enhancement)
- Gaussian blur (denoising)
- Laplacian filter or custom sharpening kernel (sharpening)
- Non-Local Means (NLM) denoising for preserving details in old images

2. Colorization with DeOldify

We used the DeOldify model built with PyTorch and FastAI, leveraging a U-Net Generator with NoGAN training. It produced realistic and naturally blended colorized outputs from grayscale images.

3. Post-processing (We used 2 approaches) appliyng
- Unsharp Masking for subtle detail sharpening (First approach)
- Gaussian Blur for light denoising and smoothness (Second approach)

--------

📈 Evaluation
| Metric | Score    | Interpretation                               |
| ------ | -------- | -------------------------------------------- |
| PSNR   | 30.98 dB | Excellent luminance preservation             |
| SSIM   | 0.9838   | Very high structural similarity              |
| LPIPS  | 0.0918   | Low perceptual difference (visually natural) |

🧪 A human evaluation survey was also conducted to validate the visual appeal and realism of the results.

--------
🔍 Dataset

We used 29 grayscale vintage photos from the "Old Photos Dataset" on Kaggle and manually categorized them by quality.

--------

💡 Key Contributions
- Two adaptive preprocessing pipelines based on image condition
- Demonstrated the importance of image preparation before colorization
- Combined traditional enhancement methods with modern GAN-based colorization
- Delivered strong perceptual and structural restoration results
--------


A simple example of the work..

![old_photo_03](https://github.com/user-attachments/assets/e3f537a8-de7a-4ee6-b282-965da2909868)

![processed_colorized_Low_old_photo_03](https://github.com/user-attachments/assets/47cd66f4-94bc-48c2-aea3-69e780c8d5a6)

![old_photo_28](https://github.com/user-attachments/assets/3c014843-d515-4311-b605-6a9a7dee049f)

![processed_colorized_Low_old_photo_28](https://github.com/user-attachments/assets/abdf741d-7cec-45b3-b67b-9a0dcea26c61)



  
