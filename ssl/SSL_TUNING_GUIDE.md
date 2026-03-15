# SSL Tuning Guide (Beyond Epochs)

This guide explains **why** each tuning knob helps, and uses your run (temperature 0.5, 100 epochs, loss 4.15 → 2.50) as a reference.

---

## Your Run: Temperature 0.5, 100 Epochs

| Phase        | Epochs   | SSL Loss   | What it suggests |
|-------------|----------|------------|-------------------|
| **Fast drop** | 1–20   | 4.15 → 2.81 | Model quickly learns to pull positive pairs together. |
| **Steady**    | 20–80  | ~2.8 → ~2.53 | Gradual improvement; learning rate may be a bit high late. |
| **Plateau**   | 80–100 | ~2.50      | Loss flattens; more epochs alone may not help much. |

So: **beyond more epochs**, you can try the options below to get lower loss and better representations.

---

## 1. Learning rate and scheduling (e.g. CosineAnnealing, warmup + decay)

**Why it helps**

- Early in training, a larger learning rate helps the loss drop quickly (as in your first ~20 epochs).
- Later, if the learning rate stays high, updates can overshoot and the loss may oscillate (e.g. small bumps like 2.81, 2.80, 2.81).
- **CosineAnnealing** gradually reduces the learning rate from the initial value to near zero over the run. Smaller steps at the end make optimization more stable and often allow the loss to decrease a bit more instead of stalling around 2.50.
- **Warmup** (small lr for the first few epochs) avoids unstable gradients at the very beginning.

**In short:** Your curve suggests the model is close to a minimum; a decaying lr can help it settle there instead of bouncing.

---

## 2. Temperature (e.g. 0.1–0.2 instead of 0.5)

**Why it matters**

In NT-Xent, logits are **divided by temperature** before softmax: `sim / temperature`.

- **Larger temperature (e.g. 0.5):**  
  Logits become smaller in magnitude → softmax is “softer” → the probability gap between the positive pair and negatives is smaller. The model gets a weaker signal to separate positives from negatives, so representations may be less discriminative.

- **Smaller temperature (e.g. 0.1–0.2):**  
  Logits become larger → softmax is “sharper” → the loss pushes the positive pair’s probability up and negatives down more strongly. Gradients focus more on pulling positives together and pushing negatives apart, so **positive vs negative separation is stronger** and representations often improve. Too small (e.g. 0.05) can make optimization harder or unstable.

**With your run:** At temperature 0.5 you already reached ~2.50. Trying 0.2 (or 0.1 carefully) can give a **sharper contrast** and sometimes a lower loss and better features for downstream tasks.

---

## 3. Weight decay (e.g. 1e-6 for Adam)

**Why it helps**

- With limited data (e.g. 291 images), the model can overfit: it memorizes the training set and generalizes poorly.
- **Weight decay** adds an L2 penalty on the weights, encouraging smaller weights and **simpler, smoother** solutions instead of fitting noise.
- So the encoder and projection head are regularized, which often improves generalization and can also help the loss curve behave more smoothly in later epochs.

---

## 4. Batch size and gradient accumulation (simulate larger batch)

**Why it helps**

- In NT-Xent, **all other samples in the same batch** act as negatives for each anchor. So:
  - Batch size 32 → 31 negatives per positive pair.
  - Effective batch 128 (e.g. via accumulation) → 127 negatives per pair.
- More negatives give a **richer contrastive signal** and usually better representations. With only 291 images, simulating a larger batch (e.g. 64 or 128) via gradient accumulation is a simple way to get this benefit without more data.

---

## 5. Projection head (e.g. 2048→2048→128)

**Why it helps**

- The projection head maps encoder features (e.g. 2048-d) into the space where contrastive loss is applied. If the head is too small (e.g. 2048→512→128), it can **bottleneck** the information and limit how well the encoder’s features are used.
- SimCLR uses 2048→2048→128: the hidden dimension matches the encoder, giving the head more capacity. Trying a larger hidden size (e.g. 2048) or a larger output dimension can improve representation quality without changing the core idea.

---

## 6. Pretrained encoder (e.g. ImageNet `weights="IMAGENET1K_V1"`)

**Why it helps**

- Training ResNet from scratch needs a lot of data to learn basic edges, textures, and shapes.
- Starting from **ImageNet pretrained weights**, the encoder already has good low-level and mid-level features. SSL then only needs to **adapt** these to your dataset (same image → closer; different images → farther). So:
  - Convergence is usually **faster** (you may see a steeper drop in the first epochs).
  - Final representations are often **better**, especially with limited data like 291 images.

---

## 7. Data augmentation (stronger or task-specific)

**Why it matters**

- **Stronger augmentation** (e.g. Gaussian blur, RandAugment): The two views of the same image differ more, so the model must rely on more **invariant** structure (shape, layout) rather than trivial cues. That typically gives more robust representations.
- **Weaker augmentation when needed:** If your downstream task relies on **color** (e.g. some defect detection), strong `RandomGrayscale` can remove useful signal. Reducing or removing it keeps color in the SSL objective and can improve performance on color-sensitive tasks.

---

## Quick reference

| Tuning knob        | Typical change              | Main effect                          |
|--------------------|-----------------------------|--------------------------------------|
| Learning rate      | Add CosineAnnealing / decay | Smoother, often lower late loss      |
| Temperature        | Try 0.2 or 0.1              | Sharper contrast, better separation |
| Weight decay       | e.g. 1e-6 for Adam          | Less overfitting, better generalization |
| Batch size         | Gradient accumulation       | More negatives, richer contrast      |
| Projection head    | 2048→2048→128 or larger     | Less bottleneck, better representations |
| Encoder init       | ImageNet pretrained         | Faster convergence, better features  |
| Augmentation       | Stronger or task-specific   | More robust or task-aligned features |

Your run (temperature 0.5, loss 4.15 → 2.50) is a good baseline; the options above are the main levers to improve **beyond more epochs**.
