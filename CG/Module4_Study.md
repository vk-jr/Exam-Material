# MODULE 4 — Digital Image Processing Fundamentals
## CST304 Computer Graphics | All Topics + Exam Answers

---

## TOPIC 1 — Fundamental Steps in Digital Image Processing
**[8 marks | Diagram = 4 marks alone | Appears in ALL papers]**

### What is Digital Image Processing? (Simple explanation)
Taking a digital image (like a photo) and performing operations on it using a computer — to improve it, analyse it, or extract useful information.

**Think of it like:** Automatic Photoshop done by algorithms.

### Block Diagram — Draw this in exam (4 marks):

```
         Problem Domain
               ↓
       Image Acquisition
               ↓
       Image Enhancement    ←——————————————————
               ↓                               |
       Image Restoration    ←— Knowledge Base —→
               ↓                               |
   Morphological Processing ←——————————————————
               ↓
         Segmentation
               ↓
  Representation & Description
               ↓
      Object Recognition
               ↓
            Results
```

*(The Knowledge Base box connects bidirectionally to the middle steps — show it as a side box with arrows)*

### Explanation of Each Step:

| Step | What it does | Real-world example |
|---|---|---|
| **Image Acquisition** | Digitally capturing the image from a sensor | Camera taking a photo |
| **Image Enhancement** | Making image look better for human viewing | Increasing brightness, contrast, sharpness |
| **Image Restoration** | Removing known degradation (blur, noise) | Deblurring a shaky photo |
| **Morphological Processing** | Operations based on shape of objects in image | Removing small noise blobs by shape |
| **Segmentation** | Dividing image into meaningful regions/objects | Separating background from foreground |
| **Representation & Description** | Describing objects mathematically | Finding edges, area, perimeter of objects |
| **Object Recognition** | Identifying and labelling what objects are present | Face recognition, text OCR |
| **Knowledge Base** | Domain knowledge that guides each step | e.g., knowing medical images need noise removal |

### For 8-mark answer:
- Block diagram (with Knowledge Base): 4 marks
- Brief explanation of each step (1-2 lines each): 4 marks

---

## TOPIC 2 — Components of an Image Processing System
**[6 marks | Alternative Part B question]**

### Block Diagram:

```
           Image Sensors / Cameras
                     ↓
      Specialized Image Processing Hardware
                     ↕
              Computer (CPU + Memory)
             /         |          \
      Image Displays  Mass Storage  Hardcopy
             ↕
         Network / Cloud
```

### Components:

| Component | What it does |
|---|---|
| **Image Sensors** | Cameras, scanners that capture the original image |
| **Specialized Hardware** | Fast processors (GPUs) for image operations |
| **Computer** | Runs image processing software; core of the system |
| **Image Displays** | Monitor to view original and processed results |
| **Mass Storage** | Hard disk, cloud storage to store large image files |
| **Hardcopy** | Printer to produce physical output |
| **Network** | To transmit or share images |

### For 6-mark answer:
Draw diagram + 1-2 lines per component = full marks.

---

## TOPIC 3 — Sampling and Quantization
**[3 marks | Part A | Appears in every paper]**

### Simple Explanation:
A real-world image is **continuous** (infinite resolution). To store it in a computer (which is digital/discrete), we must **sample** and **quantize** it.

**Analogy:** Imagine a smooth sine wave. To store it digitally:
- **Sampling** = read the value at fixed time intervals (discrete x positions)
- **Quantization** = round each value to one of a fixed set of allowed levels (discrete y values)

### Sampling:
- Divides the image into a **grid of pixels**
- Each pixel represents one small area of the original image
- **More samples = higher spatial resolution = sharper image**
- Sampling rate = number of pixels per unit length (or area)

### Quantization:
- Assigns a **discrete grey level** to each sampled pixel
- k-bit image → 2^k grey levels (e.g., 8-bit → 256 levels)
- **More bits = higher grey level resolution = smoother gradients**
- Too few levels → **false contouring** (visible banding in smooth areas)

### 3-mark answer:
> **Sampling (1.5 marks):** Converting a continuous image into discrete spatial coordinates by dividing it into a grid. Determines spatial resolution — more samples means finer detail.
>
> **Quantization (1.5 marks):** Converting continuous intensity values at each sampled pixel into discrete integer levels. A k-bit image has 2^k grey levels. More bits means smoother gradients.

---

## TOPIC 4 — Spatial vs Grey Level Resolution
**[6 marks | Appeared in papers]**

### Spatial Resolution:
- How many **pixels** are used to represent the image
- Measured in pixels per inch (PPI) or total pixels (e.g., 1920×1080)
- **Higher spatial resolution** = more pixels = sharper, more detailed image
- Reducing spatial resolution → image becomes **blocky/pixelated**

### Grey Level Resolution:
- How many **grey levels (shades of grey)** are used for pixel intensity
- Measured in **bits per pixel (bpp)**
- 1-bit: 2 levels (black & white only)
- 8-bit: 256 levels (standard greyscale)
- **Higher grey level resolution** = smoother shading, more realistic tones
- Reducing grey level resolution → **false contouring** (visible stepped bands)

### Comparison Table:

| Feature | Spatial Resolution | Grey Level Resolution |
|---|---|---|
| What varies | Number of pixels (grid size) | Number of grey shades |
| Controlled by | Sampling | Quantization |
| Effect of reduction | Blocky, pixelated look | False contours appear |
| Common measure | PPI or megapixels | Bits per pixel (bpp) |
| Formula | M × N total pixels | 2^k levels for k-bit image |

### For 6-mark answer:
- Spatial resolution: 3 marks (definition + effect of reduction + example)
- Grey level resolution: 3 marks (definition + effect of reduction + example)

---

## TOPIC 5 — Pixel Connectivity and Paths
**[8 marks | Numerical | Appears in EVERY paper]**

### Basic Pixel Relationships:

**4-Neighbours** of pixel P at (x,y) — up, down, left, right only:
```
         (x, y-1)
(x-1, y)    P    (x+1, y)
         (x, y+1)
```

**8-Neighbours** of pixel P at (x,y) — all 8 surrounding pixels (includes diagonals):
```
(x-1,y-1)  (x,y-1)  (x+1,y-1)
(x-1, y )     P     (x+1, y )
(x-1,y+1)  (x,y+1)  (x+1,y+1)
```

### Types of Paths:

**4-connected path:**
- Can only move **up, down, left, right** (no diagonals)
- Safer, less ambiguous
- May be longer or may not exist if diagonals are needed

**8-connected path:**
- Can move in **all 8 directions** including diagonals
- More flexible, paths are shorter
- Can cause ambiguity at some corners

**m-connected path (mixed connectivity):**
- Use diagonal step **only if there is NO 4-connected path** available at that step
- Eliminates ambiguity while still allowing diagonals when necessary
- Usually gives same or slightly longer path length than 8-connected

### Path Length:
Length of a path = **number of steps taken** (not counting the starting pixel, OR counting all pixels including start — check context of question)

---

### Worked Example — From May 2023 (8 marks)

**Given grid (read as row, column from top-left = row 0, col 0):**
```
Row 0:  3   1   2   1  ← q is at (row0, col3)
Row 1:  2   2   0   2
Row 2:  1   2   1   1
Row 3:  1   0   1   2  ← p is at (row3, col0)
```

**V = {0, 1}** (only pixels with value 0 or 1 are valid path pixels)

**Mark valid pixels (✓ = in V, ✗ = not in V):**
```
Row 0:  3(✗)  1(✓)  2(✗)  1(✓) ← q
Row 1:  2(✗)  2(✗)  0(✓)  2(✗)
Row 2:  1(✓)  2(✗)  1(✓)  1(✓)
Row 3:  1(✓)  0(✓)  1(✓)  2(✗) ← p
```

**4-connected path from p to q:**
- From p(r3,c0): can go right to (r3,c1)=0✓ → right to (r3,c2)=1✓ → up to (r2,c2)=1✓ → up to (r1,c2)=0✓
- From (r1,c2): up=(r0,c2)=2✗, right=(r1,c3)=2✗ → **stuck, no 4-connected path exists**

**8-connected path from p to q:**
- p(r3,c0) → (r3,c1)=0 → diagonal (r2,c2)=1 → (r1,c2)=0 → diagonal (r0,c3)=1=q
- Path: (r3,c0)→(r3,c1)→(r2,c2)→(r1,c2)→(r0,c3)
- **Length = 4** ✓

**m-connected path from p to q:**
- (r3,c0)→(r3,c1): 4-connected step ✓
- (r3,c1)→(r2,c2): diagonal — check if 4-path exists: (r3,c1)→(r2,c1)=2✗ or (r3,c2)→(r2,c2) possible? (r3,c2)=1✓ so 4-path via (r3,c2) exists → DON'T use diagonal, use: (r3,c1)→(r3,c2)→(r2,c2)
- Continue: (r2,c2)→(r1,c2)→(r0,c3): diagonal — (r0,c3)=1✓, check 4-path: (r1,c3)=2✗ → no 4-path → diagonal allowed
- m-path: (r3,c0)→(r3,c1)→(r3,c2)→(r2,c2)→(r1,c2)→(r0,c3)
- **Length = 5**

---

### Additional Example — V = {1, 2}

With V={1,2}, valid pixels are those with value 1 or 2:
```
Row 0:  3(✗)  1(✓)  2(✓)  1(✓) ← q
Row 1:  2(✓)  2(✓)  0(✗)  2(✓)
Row 2:  1(✓)  2(✓)  1(✓)  1(✓)
Row 3:  1(✓)  0(✗)  1(✓)  2(✓) ← p
```

- **4-connected path:** p(r3,c0)→(r2,c0)→(r2,c1)→(r2,c2)→(r2,c3)→(r1,c3)→(r0,c3)=q — Length = **6**
- **8-connected path:** p(r3,c0)→(r2,c1) diagonal→(r2,c2)→(r1,c1) diagonal→(r0,c2) diagonal? Check... p→(r2,c1)→(r1,c1) diagonal→(r0,c2) diagonal→(r0,c3) — **Length = 4**
- **m-connected path:** Length = **6** (same as 4-connected when 4-path is available at each step)

---

## TOPIC 6 — Pixel Connectivity Terms
**[3 marks | Part A | Appeared in June 2023, May 2019]**

### Key Definitions:

**Neighbours:** Pixels adjacent to a given pixel. 4-neighbours = 4 pixels (up/down/left/right). 8-neighbours = all 8 surrounding pixels.

**Boundary:** The set of pixels **inside a region** that have at least one neighbour that is **outside the region**.

**Path:** A sequence of distinct pixels (p₁, p₂, ..., pₙ) where each consecutive pair are neighbours (connected by 4 or 8 connectivity).

**Connected Component:** For a set S of pixels, two pixels p and q are connected in S if a path exists between them using only pixels in S. The set of all pixels connected to p forms a connected component.

### 3-mark answer:
Define any 3 of the above — one definition per mark.

---

## TOPIC 7 — Convolution
**[8 marks | Appeared in June 2023 and Dec 2019]**

### What is Convolution? (Simple explanation)
Convolution slides a small filter mask (kernel) over every pixel in an image. At each position, it multiplies the mask values with corresponding image pixel values and sums them to produce an output pixel.

**Think of it like:** Applying an Instagram filter — the filter mask decides the effect.

### Formula:
```
(f ★ g)(x,y) = Σ Σ f(s,t) · g(x-s, y-t)
```

### Steps to Apply a Convolution Mask:
1. Place mask centered on a pixel
2. Multiply each mask value with the corresponding image pixel value
3. Sum all products
4. That sum = output pixel value at that position
5. Slide mask to next pixel, repeat

### Example — Applying a 3×3 averaging mask:
```
Mask:                  Image region under mask:
1/9  1/9  1/9          10  20  30
1/9  1/9  1/9          40  50  60
1/9  1/9  1/9          70  80  90

Output = (1/9)(10+20+30+40+50+60+70+80+90) = 450/9 = 50
```

### Properties of Convolution:
1. **Commutative:** f ★ g = g ★ f
2. **Associative:** (f ★ g) ★ h = f ★ (g ★ h)
3. **Distributive:** f ★ (g + h) = (f ★ g) + (f ★ h)
4. **Shift invariant:** shifting input by x₀ shifts output by x₀

### Applications:
| Application | Mask type |
|---|---|
| Blurring / Smoothing | Averaging or Gaussian mask |
| Sharpening | Laplacian mask |
| Edge detection | Sobel or Prewitt mask |
| Noise removal | Gaussian or median filter |

### For 8-mark answer:
- Definition + formula: 2 marks
- Steps to apply convolution: 3 marks
- Properties: 2 marks
- Applications: 1 mark

---

## EXAM STRATEGY — Module IV

| Question | Content | Recommended |
|---|---|---|
| **Q17** | Fundamental steps diagram + Spatial vs Grey resolution | **Best choice — draw diagram for 4 marks** |
| **Q18** | Components diagram + Pixel path numerical OR Convolution | Pixel path numerical is scoring if you know it |

**Best choice: Q17** — The fundamental steps block diagram alone earns 4 marks just by drawing correctly.

---

## QUICK REVISION SUMMARY

| Topic | What to remember |
|---|---|
| Fundamental Steps | 7 steps: Acquisition→Enhancement→Restoration→Morphology→Segmentation→Representation→Recognition |
| Components | Sensors, Hardware, Computer, Display, Storage, Hardcopy, Network |
| Sampling | Discrete spatial grid → spatial resolution → more pixels = sharper |
| Quantization | Discrete grey levels → grey resolution → 2^k levels for k-bit |
| Spatial resolution | PPI or megapixels; reduction → blocky |
| Grey level resolution | Bits per pixel; reduction → false contours |
| 4-connected | Up/down/left/right only |
| 8-connected | All 8 directions |
| m-connected | Diagonal only if no 4-path available at that step |
| Convolution | Slide mask, multiply, sum → output pixel |
