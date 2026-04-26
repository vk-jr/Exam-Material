# MODULE 5 — Image Enhancement & Segmentation
## CST304 Computer Graphics | All Topics + Exam Answers

---

## TOPIC 1 — Basic Intensity Transformations
**[6 marks | Appears in every paper]**

### What are Intensity Transformations? (Simple explanation)
We modify the **brightness/darkness** of every pixel in an image using a mathematical function.

**General formula:** s = T(r)
- r = input pixel value (original brightness)
- s = output pixel value (new brightness)

---

### (i) Image Negatives
**[3 marks]**

**Formula:** s = (L - 1) - r

Where L = number of grey levels (for 8-bit image: L = 256, so L-1 = 255)

**What it does:** Flips bright pixels dark and dark pixels bright. Like a photo negative film.

**Example:**
- r = 200 (bright) → s = 255 - 200 = **55** (dark)
- r = 50 (dark) → s = 255 - 50 = **205** (bright)

**When used:** Enhancing white/grey detail embedded in dark regions — very useful in medical X-ray images.

**Graph shape (draw this):**
```
s ↑
  |*
  | *
  |  *
  |   *
  |    *
  +———————→ r
```
A straight diagonal line from top-left to bottom-right.

---

### (ii) Power Law (Gamma) Transformation
**[3 marks]**

**Formula:** s = c · r^γ

Where:
- c = constant (usually 1)
- γ (gamma) = the power value

**What it does based on γ:**
- **γ < 1** (e.g., 0.4): Maps dark inputs to brighter outputs → **brightens dark images**
- **γ > 1** (e.g., 2.5): Maps bright inputs to darker outputs → **darkens bright images**
- **γ = 1**: No change (linear — same as input)

**Real-world use:** Monitor/TV **gamma correction** — compensates for non-linear display response.

**Graph shape (draw this):**
```
s ↑
  |  γ<1 (curves up — brightening)
  | ___/
  |/  ← γ=1 (straight)
  |\
  | \ γ>1 (curves down — darkening)
  +————→ r
```

---

### (iii) Log Transformation
**Formula:** s = c · log(1 + r)

**What it does:** Brightens dark pixels more than bright ones. Compresses high dynamic range.

**Used for:** Displaying Fourier spectra where the range of values is very large (small → very large).

---

## TOPIC 2 — Histogram Equalization
**[8 marks | Numerical | Appears in EVERY paper — most scoring in Module V]**

### What is a Histogram? (Simple explanation)
A bar graph showing **how many pixels have each grey level** in an image.

Example: If 100 pixels have brightness value 5, the bar at 5 has height 100.

### What is Histogram Equalization? (Simple explanation)
**Spreads out** the pixel intensities so all grey levels are used more uniformly. This usually **increases contrast**.

**Analogy:** If all exam scores are between 40-60, equalization spreads them across the full 0-100 range, making differences more visible.

### Formula / 5-Step Procedure:

**Step 1:** Count pixels at each grey level: **nk** = number of pixels with value k

**Step 2:** Total pixels: **n** = total number of pixels in image (= width × height)

**Step 3:** Probability: **pr(rk) = nk / n**

**Step 4:** Cumulative sum: **CDF(k) = Σ pr(rj) for j = 0 to k** (add up probabilities from 0 to k)

**Step 5:** New level: **sk = round( (L-1) × CDF(k) )**

Where L = number of grey levels (3-bit → L=8, L-1=7)

---

### Worked Example 1 — From May 2023 (8 marks)
**3-bit image (L=8, values 0-7), size 4×4 = 16 pixels**

**Original image:**
```
4  3  2  1
5  5  6  6
6  6  7  6
2  3  7  6
```

**Step 1: Count pixels at each level:**
| rk | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|---|
| nk | 0 | 1 | 2 | 2 | 1 | 2 | 6 | 2 |

**Step 2: n = 16**

**Step 3-5: Complete equalization table:**
| rk | nk | pr = nk/16 | Cumulative (CDF) | 7 × CDF | sk (rounded) |
|---|---|---|---|---|---|
| 0 | 0 | 0.000 | 0.000 | 0.00 | **0** |
| 1 | 1 | 0.063 | 0.063 | 0.44 | **0** |
| 2 | 2 | 0.125 | 0.188 | 1.31 | **1** |
| 3 | 2 | 0.125 | 0.313 | 2.19 | **2** |
| 4 | 1 | 0.063 | 0.375 | 2.63 | **3** |
| 5 | 2 | 0.125 | 0.500 | 3.50 | **4** (or 3) |
| 6 | 6 | 0.375 | 0.875 | 6.13 | **6** |
| 7 | 2 | 0.125 | 1.000 | 7.00 | **7** |

**Mapping:** 0→0, 1→0, 2→1, 3→2, 4→3, 5→4, 6→6, 7→7

**Output image (apply mapping):**
```
Original → Output
4 → 3    3 → 2    2 → 1    1 → 0
5 → 4    5 → 4    6 → 6    6 → 6
6 → 6    6 → 6    7 → 7    6 → 6
2 → 1    3 → 2    7 → 7    6 → 6

Output:
3  2  1  0
4  4  6  6
6  6  7  6
1  2  7  6
```

**Also draw:** Bar chart histogram before (shows all values clustered at 6) and after (spread more evenly).

### Mark Breakdown (8 marks):
- Frequency table (nk values): 2 marks
- Complete equalization table (CDF, sk): 4 marks
- Before and after histogram bar charts: 2 marks

---

### Worked Example 2 — From June 2023 (8 marks)
**3-bit image (L=8), size 4×5 = 20 pixels**

**Original image:**
```
0  1  1  3  4
7  2  5  5  7
6  3  2  1  1
1  4  4  2  1
```

**Step 1: Pixel count:**
| rk | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|---|
| nk | 1 | 5 | 3 | 2 | 3 | 2 | 2 | 2 |

**Step 2: n = 20**

**Step 3-5: Equalization table:**
| rk | nk | pr = nk/20 | CDF | 7 × CDF | sk |
|---|---|---|---|---|---|
| 0 | 1 | 0.05 | 0.05 | 0.35 | **0** |
| 1 | 5 | 0.25 | 0.30 | 2.10 | **2** |
| 2 | 3 | 0.15 | 0.45 | 3.15 | **3** |
| 3 | 2 | 0.10 | 0.55 | 3.85 | **4** |
| 4 | 3 | 0.15 | 0.70 | 4.90 | **5** |
| 5 | 2 | 0.10 | 0.80 | 5.60 | **6** |
| 6 | 2 | 0.10 | 0.90 | 6.30 | **6** |
| 7 | 2 | 0.10 | 1.00 | 7.00 | **7** |

**Mapping:** 0→0, 1→2, 2→3, 3→4, 4→5, 5→6, 6→6, 7→7

**Output image:**
```
0  2  2  4  5
7  3  6  6  7
6  4  3  2  2
2  5  5  3  2
```

---

## TOPIC 3 — Laplacian Filter
**[3 marks | Part A | Appears in every paper]**

### What is it? (Simple explanation)
A filter that finds **edges** in an image by detecting rapid changes in pixel intensity. It is a **second-order derivative** filter.

### The Filter Masks (write one in exam):
```
Mask 1 (4-connected):      Mask 2 (8-connected):
  0   1   0                  1   1   1
  1  -4   1                  1  -8   1
  0   1   0                  1  -8   1 ← typo in some books, correct is below:

Mask 2 (correct):
  1   1   1
  1  -8   1
  1   1   1
```

### How to apply:
Place the 3×3 mask centered on a pixel, multiply each mask value with the corresponding pixel, sum all 9 products = output for that pixel.

### Key property:
- **Positive result** = bright spot surrounded by dark
- **Negative result** = dark spot surrounded by bright
- **Zero** = uniform region (no edge)

### For image sharpening (enhancement):
Subtract Laplacian from original: **g(x,y) = f(x,y) − ∇²f(x,y)**

### 3-mark answer:
> The Laplacian filter is a second-order derivative filter used for edge detection and image sharpening. The filter masks are [write Mask 1]. It highlights regions of rapid intensity change. To sharpen an image, the Laplacian result is subtracted from the original: g(x,y) = f(x,y) − ∇²f(x,y).

---

## TOPIC 4 — Edge Detection: Sobel & Prewitt Operators
**[8 marks | Appears in ALL papers]**

### What is Edge Detection? (Simple explanation)
Edges = places where pixel brightness changes suddenly. Detecting edges helps identify objects in an image.

**Analogy:** In a photo of a cup, the edge is where bright background suddenly meets the dark cup. Edge detection highlights these transitions.

---

### Sobel Edge Detector
**[4 marks]**

Uses two 3×3 masks — one detects vertical edges (Gx), one detects horizontal edges (Gy):

```
Gx (vertical edges):        Gy (horizontal edges):
-1   0  +1                  -1  -2  -1
-2   0  +2                   0   0   0
-1   0  +1                  +1  +2  +1
```

**Steps:**
1. Convolve image with Gx → get Gx values
2. Convolve image with Gy → get Gy values
3. Gradient magnitude: **G = √(Gx² + Gy²)** or approximation: **G = |Gx| + |Gy|**
4. If G > threshold → edge pixel; else → not an edge

**Key features:**
- Centre row/column has weight **2** (more sensitive to nearby pixels)
- Good noise tolerance due to smoothing effect of the weight 2

---

### Prewitt Edge Detector
**[4 marks]**

Similar to Sobel but uses **equal weights** (no weight-2 centre):

```
Gx:                          Gy:
-1   0  +1                  -1  -1  -1
-1   0  +1                   0   0   0
-1   0  +1                  +1  +1  +1
```

**Steps:** Same as Sobel — apply Gx, apply Gy, combine magnitudes.

**Formula:** **G = √(Gx² + Gy²)**

---

### Sobel vs Prewitt Comparison Table:

| Feature | Sobel | Prewitt |
|---|---|---|
| Centre weight | **2** | **1** |
| Noise sensitivity | Less sensitive (better) | Slightly more sensitive |
| Accuracy | Slightly better | Simpler |
| Complexity | Slightly more complex | Simpler |

### For 8-mark answer:
- Sobel: both masks (Gx + Gy) + explanation + formula = 4 marks
- Prewitt: both masks (Gx + Gy) + explanation + comparison = 4 marks

---

## TOPIC 5 — Intensity Thresholding
**[3 marks | Part A | Appears in every paper]**

### Simple explanation:
Choose a threshold value **T**. Every pixel brighter than T → **white (1)** (object). Every pixel darker or equal to T → **black (0)** (background).

### Formula:
```
g(x,y) = 1    if f(x,y) > T    (object pixel)
g(x,y) = 0    if f(x,y) ≤ T    (background pixel)
```

### Types:
- **Global thresholding:** One value of T used for entire image
- **Variable / Adaptive thresholding:** T changes across different regions (used when lighting is uneven)

### Global Thresholding Algorithm:
1. Select an initial estimate for T (e.g., midpoint of intensity range)
2. Segment image into G1 (pixels > T) and G2 (pixels ≤ T)
3. Compute mean intensities μ1 (of G1) and μ2 (of G2)
4. Update T = (μ1 + μ2) / 2
5. Repeat steps 2-4 until T stops changing (convergence)

**Used in:** Separating text from paper, medical image analysis, fingerprint detection.

---

## TOPIC 6 — Region Splitting and Merging
**[6 marks | Appeared in ALL papers]**

### What is Region-Based Segmentation? (Simple explanation)
Instead of looking for edges, divide the image into **regions** where all pixels share similar properties (e.g., similar intensity).

### Region Splitting:
- Start with the **whole image** as one region
- If region is NOT homogeneous (pixels are not uniform), **split it into 4 equal quadrants**
- Keep splitting non-uniform quadrants recursively
- This creates a **Quad-tree** structure

```
Step 1:        Step 2:           Step 3:
+---------+    +----+----+       +--+--+----+
|         |    | ?  | ?  |       |  |  | ok |
| NOT     | →  |    |    |   →   +--+--+    |
| uniform |    +----+----+       |  |  +----+
|         |    | ok | ?  |       +--+--+ ?  |
+---------+    +----+----+       | ok|     |
                                 +---+-----+
```

### Region Merging:
- Start with small individual regions (or even individual pixels)
- **Merge adjacent regions** if they satisfy a similarity predicate P
  - Example: all pixels in merged region have intensity within range [a, b]
- Continue merging until no more regions satisfy the merge condition

### Combined Split-and-Merge Algorithm:
1. Start: whole image = one region
2. **Split** any non-homogeneous region into 4 quadrants
3. **Merge** any adjacent regions that are similar enough
4. Stop when no more splits or merges are possible
5. Result = final segmented regions

**Predicate P example:** Pixels in region all have intensity in range [50, 100]

### For 6-mark answer:
- Region splitting with quad-tree diagram: 3 marks
- Region merging with steps/example: 3 marks

---

## EXAM STRATEGY — Module V

| Question | Content | Recommended |
|---|---|---|
| **Q19** | Intensity transformations + Histogram equalization numerical | **ALWAYS choose Q19 if histogram equalization appears** |
| **Q20** | Edge detectors Sobel/Prewitt + Region splitting/merging | Also good — masks easy to memorize |

**Best choice: Q19** — Histogram equalization table is 100% mechanical and formulaic. No guessing needed.

---

## QUICK REVISION SUMMARY

| Topic | What to remember |
|---|---|
| Image Negatives | s = 255 - r (for 8-bit); dark→bright, bright→dark |
| Power Law | s = c·r^γ; γ<1 brightens, γ>1 darkens |
| Log Transform | s = c·log(1+r); compresses high dynamic range |
| Histogram Equalization | 5 steps: count→prob→CDF→multiply by (L-1)→round |
| Laplacian filter | Second-order derivative; masks with -4 or -8 at centre; for edge detection + sharpening |
| Sobel | Gx and Gy masks with weight 2 at centre row/col; G = √(Gx²+Gy²) |
| Prewitt | Same idea but equal weights (1 everywhere); simpler |
| Sobel vs Prewitt | Sobel less noisy; Prewitt simpler |
| Thresholding | g=1 if f>T else g=0; global vs adaptive |
| Region Split | Quad-tree: split if not homogeneous |
| Region Merge | Merge adjacent similar regions until stable |
