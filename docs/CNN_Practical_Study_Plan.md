# CNN Practical Study Plan — Small Steps

You have completed the **CNN Theory** and **CNN Math** sections.

The next goal is to start the **practical CNN section** in very small checkpoints, learning only one idea at a time.

---

## 1. CIFAR-10 Basics — NEXT

Learn:

- What CIFAR-10 is
- Why it has 10 classes
- Image size: `32 × 32`
- Why the input shape is:

```text
3 × 32 × 32
```

---

## 2. Understand the First Convolution Layer

Example:

```python
nn.Conv2d(
    in_channels=3,
    out_channels=32,
    kernel_size=3
)
```

Understand:

- What `3` means
- What `32` means
- What `kernel_size=3` means

---

## 3. Calculate the First Output Shape Manually

Start with:

```text
3 × 32 × 32
```

Then:

- Apply convolution
- Calculate the new height
- Calculate the new width
- Determine the new number of channels

---

## 4. Understand ReLU in the Real Network

Learn:

- What comes out of convolution
- What ReLU changes
- What ReLU does **not** change
- Why the tensor shape stays the same

---

## 5. Understand Max Pooling

Learn:

- What `2 × 2` max pooling does
- Why the spatial size becomes smaller
- Why the number of channels remains unchanged

---

## 6. Track the Complete First CNN Block

Follow:

```text
Input
  ↓
Conv2d
  ↓
ReLU
  ↓
MaxPool
```

Manually track the tensor dimensions through every step.

---

## 7. Add the Second Convolution Block

Study:

```text
Conv2d
  ↓
ReLU
  ↓
MaxPool
```

Understand why a CNN might increase the number of channels:

```text
32 channels → 64 channels
```

---

## 8. Track Shapes Through Both Convolution Blocks

Example structure:

```text
3 × 32 × 32
    ↓
32 × ? × ?
    ↓
32 × ? × ?
    ↓
64 × ? × ?
    ↓
64 × ? × ?
```

Calculate every missing dimension manually.

---

## 9. Understand Flatten

Learn how a tensor such as:

```text
64 × 6 × 6
```

becomes:

```text
2304
```

Understand why:

```text
64 × 6 × 6 = 2304
```

---

## 10. Understand the Linear Layer

Learn:

- Why convolutional processing stops here
- Why the network now needs a classifier

Example:

```python
nn.Linear(2304, 128)
```

Understand what `2304` and `128` represent.

---

## 11. Understand the Final Output Layer

CIFAR-10 contains 10 classes.

Therefore, the final layer can be:

```python
nn.Linear(128, 10)
```

Understand what the **10 output values** represent.

---

## 12. Put the Complete CNN Together

Only after understanding the previous steps, study the complete architecture:

```text
Image
  ↓
Conv
  ↓
ReLU
  ↓
Pool
  ↓
Conv
  ↓
ReLU
  ↓
Pool
  ↓
Flatten
  ↓
Linear
  ↓
Linear
  ↓
10 outputs
```

---

## 13. Understand One Forward Pass

Follow **one CIFAR-10 image** through the complete network.

Track:

```text
Input image
→ Conv1
→ ReLU
→ Pool
→ Conv2
→ ReLU
→ Pool
→ Flatten
→ Linear1
→ Linear2
→ 10 output scores
```

The goal is to understand what happens to the image and tensor shape at every stage.

---

## 14. Study CNN Training

After the architecture is clear, connect the training concepts to CNNs.

Study:

- Loss
- `optimizer.zero_grad()`
- Forward pass
- Backward pass
- `optimizer.step()`

Training flow:

```text
Input batch
   ↓
Forward pass
   ↓
Predictions
   ↓
Calculate loss
   ↓
zero_grad()
   ↓
Backward pass
   ↓
Optimizer step
```

---

## 15. Compare CNN vs MLP

Use the same CIFAR-10 task and compare:

- Parameter count
- Spatial information
- Local connectivity
- Parameter sharing
- Feature extraction
- Performance differences

The goal is to understand **why CNNs are better suited to image data than a basic fully connected MLP**.

---

# Final Elite Task — Manual Parameter Calculation

Calculate the trainable parameters for:

```text
Conv1
Conv2
Linear1
Linear2
```

Then calculate:

```text
Total CNN parameters
```

This should combine your understanding of:

- Input channels
- Output channels
- Kernel size
- Bias
- Flattening
- Linear-layer parameters

---

# Progress Tracker

```text
CNN Theory ✅
CNN Math ✅

CNN Practical
├── 1. CIFAR-10 basics       ← NEXT
├── 2. First Conv2d
├── 3. Output shape
├── 4. ReLU
├── 5. Pooling
├── 6. First CNN block
├── 7. Second CNN block
├── 8. Shape tracking
├── 9. Flatten
├── 10. Linear layer
├── 11. 10-class output
├── 12. Complete architecture
├── 13. Forward pass
├── 14. Training
└── 15. CNN vs MLP

Elite Task
└── Manual parameter calculation
```

---

# Current Next Lesson

Do **not** jump to the complete CIFAR-10 implementation yet.

Your next lesson is only:

## Step 1 — What is CIFAR-10 and why is its input `3 × 32 × 32`?

Once this is fully understood, move to **Step 2: the first `Conv2d` layer**.
