# 🤖 GAN vs VAE — Generative Model Comparison
### PyTorch Implementation on MNIST Handwritten Digits

---

## 📌 Project Overview
A hands-on comparison of two foundational Generative AI architectures —
Variational Autoencoder (VAE) and Generative Adversarial Network (GAN) —
implemented from scratch in PyTorch and trained on the MNIST dataset
(60,000 handwritten digit images).

Both models were built without using any pre-trained weights, allowing a
fair evaluation of how each architecture learns to generate new images
from scratch.

---

## 🧠 What I Built

### Variational Autoencoder (VAE)
- **Architecture:** Encoder (784 → 512 → 256 → 20) + Decoder (20 → 256 → 512 → 784)
- **Latent dimension:** 20
- **Loss function:** Reconstruction Loss (MSE) + KL Divergence
- **Epochs:** 20 | **Training time:** 256 seconds

### Generative Adversarial Network (GAN)
- **Generator:** Noise (100) → 256 → 512 → 1024 → 784
- **Discriminator:** 784 → 1024 → 512 → 256 → 1
- **Loss function:** Binary Cross Entropy (BCE)
- **Epochs:** 50 | **Training time:** 3,515 seconds

---

## 📊 Results

### Generated Images

| Model | Output Quality | Observation |
|---|---|---|
| VAE Reconstruction | ⭐⭐⭐⭐⭐ | Near-identical to originals |
| VAE Generation | ⭐⭐⭐ | Recognisable but noticeably blurry |
| GAN Generation | ⭐⭐⭐⭐ | Sharper edges, occasional distortion |

### Training Behaviour

| Metric | VAE | GAN |
|---|---|---|
| Final Total Loss | 64.2 | — |
| Final Reconstruction Loss | 43.0 | — |
| Final KL Loss | 21.2 | — |
| Final Generator Loss | — | ~0.93 |
| Final Discriminator Loss | — | ~1.30 |
| Total Parameters | ~1.08M | ~2.95M |
| Training Time | 256s | 3,515s |
| Training Stability | ✅ Smooth | ⚠️ Fluctuating |

---

## 🔍 Key Findings

### Finding 1 — VAE is significantly more stable to train
The VAE loss curves decreased smoothly and consistently across all
20 epochs with no instability. The GAN loss fluctuated — particularly
in the first 10 epochs — before settling into equilibrium around epoch
20. For production environments where training reproducibility matters,
VAE is the safer choice.

### Finding 2 — GAN produces sharper images but at significant cost
GAN generated images showed clearly crisper edges and more confident
digit strokes compared to VAE outputs. However, this came at the cost
of 3x more parameters and 14x longer training time. The GAN also
produced occasional distorted or unrecognisable outputs — a symptom
of mode instability that VAE does not exhibit.

### Finding 3 — VAE reconstruction is near-perfect; GAN cannot reconstruct
The VAE can take a real image, compress it to 20 numbers, and
reconstruct it with high fidelity. The GAN has no reconstruction
capability — it can only generate from noise. This makes VAE the
only viable choice for tasks requiring both generation AND reconstruction,
such as image denoising or anomaly detection.

### Finding 4 — GAN equilibrium was achieved, avoiding mode collapse
The Generator loss stabilised at ~0.93 and Discriminator at ~1.30 by
epoch 50. This balanced equilibrium indicates the training avoided
mode collapse — a common GAN failure where the Generator produces
identical outputs repeatedly. Label smoothing (real labels = 0.9)
and Adam optimiser with β=0.5 were key to achieving this stability.

### Finding 5 — Efficiency strongly favours VAE
VAE achieved strong results with 1.08M parameters in 256 seconds.
GAN required 2.95M parameters and 3,515 seconds for comparable (but
sharper) outputs. In resource-constrained environments, VAE offers
a far better quality-to-compute ratio.

---

## 🏆 Verdict — When to Use Each

| Use Case | Recommended Model | Reason |
|---|---|---|
| Photo-realistic image synthesis | GAN | Superior sharpness |
| Image reconstruction / denoising | VAE | Only model that reconstructs |
| Anomaly detection | VAE | Controllable latent space |
| Limited compute / fast training | VAE | 14x faster, 3x fewer params |
| Data augmentation for ML pipelines | Either | Both generate valid new samples |
| Production deployment | VAE | More stable and predictable |

---

## 💡 What I Would Do Differently

1. **Train GAN for more epochs** — the Generator loss was still
   slightly decreasing at epoch 50, suggesting more training could
   improve output quality further

2. **Try a Convolutional GAN (DCGAN)** — replacing Linear layers
   with Convolutional layers would likely produce significantly
   sharper images as CNNs better capture spatial structure in images

3. **Increase VAE latent dimension** — bumping from 20 to 64
   dimensions would give the VAE more expressive power and likely
   reduce the blurriness in generated outputs

4. **Add Frechet Inception Distance (FID) scoring** — a quantitative
   metric to measure image quality objectively rather than relying
   on visual inspection alone

---

## 🛠️ Tech Stack
- **Framework:** PyTorch
- **Dataset:** MNIST (60,000 training images, 28×28 pixels)
- **Environment:** Google Colab (T4 GPU)
- **Visualisation:** Matplotlib

---

## 📈 Course Concepts Applied

| Great Learning Course Concept | Implementation |
|---|---|
| Neural Networks — layers & neurons | All `nn.Linear` layers |
| Activation Functions (ReLU, Sigmoid, Tanh) | Used across both models |
| Backpropagation | `loss.backward()` in every training step |
| Gradient Descent | `optimizer.step()` — Adam optimiser |
| Overfitting prevention | `nn.Dropout(0.3)` in Discriminator |
| Generative vs Discriminative models | VAE (generative) vs GAN Discriminator |
| GANs — adversarial training | Full GAN implementation in Cells 10–13 |
| VAEs — latent space representation | Full VAE implementation in Cells 5–9 |
| Loss functions | MSE for VAE, BCE for GAN |

---

## 🔗 Related Projects
- [Walmart Data Chatbot](https://github.com/mohitramtekkar5-oss/Walmart-Data-Chatbot)
- [AI-Powered Data Insights](https://github.com/mohitramtekkar5-oss/AI-Powered-Data-Insights)
- [Prompt Engineering Experiments](https://github.com/mohitramtekkar5-oss/Prompt-Engineering-Experiments)

---

## 👤 Author
**Mohit Priya Ramtekkar**
Data Analyst | ML & Generative AI Enthusiast
[LinkedIn](https://linkedin.com/in/mohit-priya-ramtekkar) •
[GitHub](https://github.com/mohitramtekkar5-oss)

---
*Built as a hands-on implementation exercise following the
Generative AI for Beginners course by Great Learning.*
