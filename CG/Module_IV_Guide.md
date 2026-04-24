# MODULE IV — Digital Image Processing Fundamentals
## CST304 Computer Graphics | Study Guide

> **Exam weight:** 3 marks (Part A Q7 or Q8) + 14 marks (Part B Q17 or Q18)
> **Appeared in:** ALL 4 question papers
>
> 📂 **Your PDF for this module:** `Module 4/M4-CST304-Ktunotes.in.pdf`

---

## TOPIC 1: Fundamental Steps in Digital Image Processing (8 marks)
> 📖 **PDF Reference:** `Module 4/M4-CST304-Ktunotes.in.pdf` → Section: **"Fundamental Steps in Digital Image Processing"**. Look for the vertical block diagram (flowchart) with all the steps — this is the most important figure in this module. The diagram alone = 4 marks in exam.
**[Appears in all papers — diagram alone = 4 marks]**

### What is Digital Image Processing? (Simple explanation)
Taking a digital image (like a photo) and doing operations on it using a computer — to improve it, analyse it, or extract useful information.

**Think of it like:** Photoshop, but done automatically by algorithms.

### The Fundamental Steps (Block Diagram):

```
Problem Domain
     ↓
Image Acquisition
     ↓
Image Enhancement       ←——————————————————————
     ↓                                          |
Image Restoration       ←— Knowledge Base ——→  |
     ↓                                          |
Morphological Processing                        |
     ↓                                          |
Segmentation                                    |
     ↓                                          |
Representation & Description                    |
     ↓                                          |
Object Recognition
     ↓
Results
```

### Explanation of Each Step:

| Step | What it does | Real-world example |
|---|---|---|
| **Image Acquisition** | Capturing the image digitally | Camera taking a photo |
| **Image Enhancement** | Making image look better for human viewing | Increasing brightness, contrast |
| **Image Restoration** | Removing degradation (blur, noise) | Deblurring a shaky photo |
| **Morphological Processing** | Shape-based operations | Removing noise by shape |
| **Segmentation** | Dividing image into meaningful parts | Separating background from objects |
| **Representation & Description** | Describing objects mathematically | Finding object edges, area |
| **Object Recognition** | Identifying what objects are in image | Face recognition, OCR |

### For 8-mark answer:
- Block diagram: 4 marks
- Brief explanation of each step (1-2 lines each): 4 marks

---

## TOPIC 2: Components of an Image Processing System (6 marks)
> 📖 **PDF Reference:** `Module 4/M4-CST304-Ktunotes.in.pdf` → Section: **"Components of an Image Processing System"**. Look for the diagram showing all hardware components connected — image sensors, computer, displays, storage, hardcopy, network.
**[Appears in papers as alternative question]**

### Block Diagram:
```
Problem Domain
      ↑
Image Sensors / Cameras
      ↓
Specialized Image Processing Hardware
      ↕
Computer (CPU + Memory)
      ↕          ↕          ↕
Image Displays   Mass Storage   Hardcopy
      ↕
     Network / Cloud
```

### Components:
1. **Image Sensors** — cameras, scanners that capture images
2. **Specialized Hardware** — fast processors for image operations (GPU)
3. **Computer** — runs image processing software
4. **Image Displays** — monitor to view results
5. **Mass Storage** — hard disk / cloud to store images
6. **Hardcopy** — printer to print results
7. **Network** — to share/transmit images

---

## TOPIC 3: Sampling and Quantization (3 marks — Part A)
**[Appears in every paper]**

### Simple explanation:
A real-world image is **continuous** (infinite detail). To store it in a computer (which is digital), we need to **sample** and **quantize** it.

**Analogy:** Think of a smooth curve. To store it digitally:
- **Sampling** = pick points on the curve at regular intervals (discrete x positions)
- **Quantization** = round the y value at each point to a fixed set of allowed values

### Sampling:
- Divides the image into a **grid of pixels**
- Each pixel represents one small area of the image
- More samples = higher **spatial resolution** = sharper image
- **Sampling rate** = number of pixels per unit area

### Quantization:
- Assigns a **discrete grey level** to each pixel
- Usually 2^n levels (e.g., 256 levels for 8-bit image)
- More levels = higher **grey level resolution** = smoother tones
- Too few levels → false contouring effect (visible bands)

### 3-mark answer:
**Sampling (1.5 marks):** Converting a continuous image into discrete spatial coordinates by dividing it into a grid. Determines spatial resolution. More samples = finer detail.

**Quantization (1.5 marks):** Converting the continuous intensity values at each sampled pixel into discrete integer values. A k-bit image has 2^k grey levels. More bits = smoother gradients.

---

## TOPIC 4: Spatial vs Grey Level Resolution (6 marks)

### Spatial Resolution:
- How many **pixels** are used to represent the image
- Measured in pixels per inch (PPI) or total pixel count (e.g., 1920×1080)
- **Higher spatial resolution** = more pixels = sharper, more detailed image
- Reducing spatial resolution → image looks blocky/pixelated

### Grey Level Resolution:
- How many **grey levels (shades)** are used for pixel intensity
- Measured in bits per pixel (bpp)
- 1-bit image: 2 levels (black & white only)
- 8-bit image: 256 levels (standard greyscale)
- **Higher grey level resolution** = smoother shading
- Reducing grey level resolution → false contouring appears (visible stepped bands)

| Feature | Spatial Resolution | Grey Level Resolution |
|---|---|---|
| What varies | Number of pixels | Number of grey shades |
| Effect of reduction | Blocky/pixelated | False contours |
| Common measure | PPI or megapixels | Bits per pixel (bpp) |
| Formula | M×N pixels | 2^k levels for k-bit |

---

## TOPIC 5: Pixel Connectivity and Paths (8 marks — NUMERICAL)
> 📖 **PDF Reference:** `Module 4/M4-CST304-Ktunotes.in.pdf` → Section: **"Pixel Relationships"** or **"Connectivity"** or **"Digital Paths"**. Look for the grid figures showing 4-neighbours and 8-neighbours diagrams, and any worked example with a pixel grid.
**[Appears in EVERY paper — must know 4, 8, m connectivity]**

### Basic Pixel Relationships:

**4-Neighbours** of pixel (x,y): the 4 pixels directly up, down, left, right
```
     (x, y-1)
(x-1, y)  P  (x+1, y)
     (x, y+1)
```

**8-Neighbours** of pixel (x,y): all 8 surrounding pixels (includes diagonals)
```
(x-1,y-1) (x,y-1) (x+1,y-1)
(x-1, y )   P    (x+1, y )
(x-1,y+1) (x,y+1) (x+1,y+1)
```

### Paths:
- **4-connected path**: Can only move up/down/left/right (no diagonals)
- **8-connected path**: Can move in all 8 directions (including diagonals)
- **m-connected path** (mixed): Use diagonal ONLY if no 4-connected path exists at that step. Avoids ambiguity.

### Connected Component:
A set S of pixels where every pair of pixels in S has a path between them through pixels in S.

### WORKED EXAMPLE (From May 2023 — 8 marks)
**Grid (p is bottom-left, q is top-right):**
```
3  1  2  1(q)
2  2  0  2
1  2  1  1
(p)1  0  1  2
```

**V = {0,1}** (only pixels with value 0 or 1 are considered for paths)

First, mark which pixels have values in V={0,1}:
```
Position: (row, col) from top=0
Row 0: 3(✗)  1(✓)  2(✗)  1(✓)  ← q is at row0,col3
Row 1: 2(✗)  2(✗)  0(✓)  2(✗)
Row 2: 1(✓)  2(✗)  1(✓)  1(✓)
Row 3: 1(✓)  0(✓)  1(✓)  2(✗)  ← p is at row3,col0
```

**4-connected path (V={0,1}):**
- From p(row3,col0), can only move to pixels with value in {0,1} using 4-connectivity
- p(r3,c0)=1✓ → right to (r3,c1)=0✓ → right to (r3,c2)=1✓ → up to (r2,c2)=1✓ → up to (r1,c2)=0✓ → can we reach q(r0,c3)?
- From (r1,c2)=0: up=(r0,c2)=2✗, right=(r1,c3)=2✗ — stuck!
- Try other routes... **4-connected path does NOT exist** for V={0,1}

**8-connected path (V={0,1}):**
- p(r3,c0) → (r3,c1)=0 → (r2,c2)=1 (diagonal) → (r1,c2)=0 → (r0,c3)=1=q (diagonal)
- Path: p → (r3,c1) → (r2,c2) → (r1,c2) → q
- **Length = 4** ✓

**m-connected path (V={0,1}):**
- Like 8-connected but diagonal allowed only if no 4-connected alternative at each step
- p(r3,c0) → (r3,c1) → (r2,c2)[diagonal, no 4-path to (r2,c2) without going through ✗] → (r1,c2) → (r0,c3)[diagonal, no 4-path]
- **Length = 5** (path may be slightly longer due to mixed connectivity rules)

**Repeat with V = {1,2}:** (pixels with values 1 or 2 are considered)
- More pixels available now
- 4-connected path length = 6
- 8-connected path length = 4
- m-connected path length = 6

---

## TOPIC 6: Defined Terms (3 marks — Part A)

### Connected Component:
Let S be a subset of pixels in an image. Two pixels p and q are **connected** in S if there exists a path between them using only pixels in S. The set of all pixels connected to p in S is called a **connected component** of S.

### Neighbours, Boundary, Path:
- **Neighbours**: Pixels adjacent to a given pixel (4-neighbours or 8-neighbours)
- **Boundary**: The set of pixels in a region that have at least one neighbour not in the region
- **Path**: A sequence of distinct pixels (p₁, p₂, ..., pₙ) where each consecutive pair are neighbours

---

## PART A QUICK ANSWERS FOR MODULE IV

### Q: Define connected component (3 marks)
For a subset S of pixels, two pixels p and q are connected in S if a path exists between them using only pixels in S. For any pixel p in S, the set of all pixels connected to it in S is called a **connected component** of S.

### Q: Write short notes on sampling and quantization (3 marks)
[See Topic 3 above — write 1.5 marks for each]

---

## EXAM STRATEGY FOR MODULE IV

**Q17 (Fundamental steps diagram + Spatial vs Grey resolution OR Illumination + Components diagram):** Most straightforward
**Q18 (Components diagram + Pixel path calculation OR Convolution + Medical applications):** Path calculation is scoring if you know it

**Recommended: Q17** — The fundamental steps diagram alone gives 4 marks just by drawing correctly

---

## NEW TOPICS ADDED (from 2023 papers)

### Convolution (8 marks — appeared in June 2023 and Dec 2019)
> 📖 **PDF Reference:** `Module 4/M4-CST304-Ktunotes.in.pdf` → Section: **"Convolution"** or **"Spatial Filtering"**. Look for the diagram showing a mask sliding over an image.

**Definition:** Convolution is a mathematical operation that slides a small filter mask (kernel) over every pixel in an image. At each position, it multiplies the mask values with the corresponding pixel values and sums them to produce an output pixel.

**Formula:** (f ★ g)(x,y) = Σ Σ f(s,t) · g(x-s, y-t)

**Properties:**
1. **Commutative:** f ★ g = g ★ f
2. **Associative:** (f ★ g) ★ h = f ★ (g ★ h)
3. **Distributive:** f ★ (g + h) = (f ★ g) + (f ★ h)
4. **Shift invariant:** shifting input shifts output by same amount

**Steps for convolving image with a mask:**
1. Place mask centered on a pixel
2. Multiply each mask value with corresponding image pixel
3. Sum all products
4. That sum = output pixel value at that position
5. Slide mask to next pixel, repeat

**Applications:**
- Blurring (smoothing) — using averaging mask
- Sharpening — using Laplacian mask
- Edge detection — using Sobel/Prewitt mask
- Noise removal — using Gaussian mask
