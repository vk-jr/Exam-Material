# MODULE V — Image Enhancement & Segmentation
## CST304 Computer Graphics | Study Guide

> **Exam weight:** 3 marks (Part A Q9 or Q10) + 14 marks (Part B Q19 or Q20)
> **Appeared in:** ALL 4 question papers
>
> 📂 **Your PDF for this module:** `Module 5/M5-CST304-Ktunotes.in.pdf`

---

## TOPIC 1: Basic Intensity Transformations (6 marks)
> 📖 **PDF Reference:** `Module 5/M5-CST304-Ktunotes.in.pdf` → Section: **"Basic Intensity Transformation Functions"** or **"Intensity Transformations"**. Look for the graph figure showing the s = T(r) curves — one for image negatives (diagonal line top-left to bottom-right), one for power law (curved lines for γ<1 and γ>1), and one for log transformation.
**[Appears in every paper]**

### What are Intensity Transformations? (Simple explanation)
We modify the brightness/darkness of every pixel using a mathematical function.
**Formula:** s = T(r) where r = input pixel value, s = output pixel value

---

### (i) Image Negatives (3 marks)

**Formula:** s = (L-1) - r

Where L = number of grey levels (usually 256 for 8-bit image, so L-1 = 255)

**What it does:** Flips bright pixels to dark and dark pixels to bright. Like a photo negative.

**Example:** If r = 200 (bright pixel), s = 255 - 200 = 55 (now dark)
If r = 50 (dark pixel), s = 255 - 50 = 205 (now bright)

**When used:** Enhancing white detail in dark regions of an image (e.g., medical X-rays)

**Graph shape:** A straight line going from top-left to bottom-right
```
s |
  |*
  | *
  |  *
  |   *
  |    *
  ——————————— r
```

---

### (ii) Power Law (Gamma) Transformation (3 marks)

**Formula:** s = c · r^γ (gamma)

Where c = constant (usually 1), γ (gamma) = the power

**What it does:**
- **γ < 1** (e.g., 0.5): Brightens dark images — maps dark input to brighter output
- **γ > 1** (e.g., 2.0): Darkens bright images — maps bright input to darker output
- **γ = 1**: No change (same as input)

**Real-world use:** Monitor/TV **gamma correction** — different displays have different gamma values. Used to compensate.

**Graph shape:**
```
s |        γ<1 (curves up — brightening)
  |    ___/
  |   /  ← γ=1 (straight line)
  |  / γ>1 (curves down — darkening)
  | /
  |/
  ——————————— r
```

---

### (iii) Log Transformation (sometimes asked instead of Power Law)

**Formula:** s = c · log(1 + r)

**What it does:** Brightens dark pixels more than bright ones. Compresses the dynamic range.

**Used for:** Displaying Fourier spectra where values range from very small to very large.

---

## TOPIC 2: Histogram Equalization (8 marks — NUMERICAL)
> 📖 **PDF Reference:** `Module 5/M5-CST304-Ktunotes.in.pdf` → Section: **"Histogram Equalization"** or **"Histogram Processing"**. Look for the before/after histogram bar graph figures, and any worked example table. The visual comparison of histogram before and after equalization is very important to study.
**[Appears in EVERY paper — most scoring numerical in Module V]**

### What is a Histogram? (Simple explanation)
A bar graph showing how many pixels have each grey level in an image.

**Example:** If your image has 100 pixels at brightness 5, the bar at 5 is 100 high.

### What is Histogram Equalization? (Simple explanation)
The idea is to **spread out** the pixel intensities so all grey levels are used more uniformly. This usually **increases contrast** in the image.

**Analogy:** Imagine most students in a class got marks between 40-60. Equalization would spread them out so some get 0-40 and others 60-100 too, making better use of the full range.

### Steps for Histogram Equalization:

1. Count pixels at each grey level: **nk** = number of pixels with value k
2. Total pixels: **n** = total number of pixels in image
3. Calculate probability: **pr(rk) = nk / n**
4. Calculate cumulative sum: **CDF(k) = Σ pr(rj) for j=0 to k**
5. Calculate new level: **sk = round( (L-1) × CDF(k) )**

Where L = number of grey levels (for 3-bit image: L=8, so L-1=7)

---

### WORKED EXAMPLE (From May 2023 paper — 8 marks)
**3-bit image (L=8, values 0-7), size 4×4 = 16 pixels total:**
```
4  3  2  1
5  5  6  6
6  6  7  6
2  3  7  6
```

**Step 1:** Count pixels at each level:
| rk | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|---|
| nk | 0 | 1 | 2 | 2 | 1 | 2 | 6 | 2 |

*(Count: 1 appears once, 2 appears twice, 3 appears twice, 4 appears once, 5 appears twice, 6 appears SIX times, 7 appears twice)*

**Step 2:** Total n = 16

**Step 3:** Complete table:

| rk | nk | pr(rk)=nk/16 | Cumulative Sum | (L-1)×CDF = 7×CDF | sk (rounded) |
|---|---|---|---|---|---|
| 0 | 0 | 0/16 = 0 | 0 | 0 | **0** |
| 1 | 1 | 1/16 = 0.06 | 0.06 | 0.42 | **0** |
| 2 | 2 | 2/16 = 0.12 | 0.18 | 1.26 | **1** |
| 3 | 2 | 2/16 = 0.12 | 0.30 | 2.10 | **2** |
| 4 | 1 | 1/16 = 0.06 | 0.36 | 2.52 | **3** |
| 5 | 2 | 2/16 = 0.12 | 0.48 | 3.36 | **3** |
| 6 | 6 | 6/16 = 0.37 | 0.85 | 5.95 | **6** |
| 7 | 2 | 2/16 = 0.12 | 0.97 | 6.79 | **7** |

**Mapping:** 0→0, 1→0, 2→1, 3→2, 4→3, 5→3, 6→6, 7→7

**Output image (apply mapping):**
```
Original: 4→3  3→2  2→1  1→0
          5→3  5→3  6→6  6→6
          6→6  6→6  7→7  6→6
          2→1  3→2  7→7  6→6

Output:
3  2  1  0
3  3  6  6
6  6  7  6
1  2  7  6
```

**Plot histogram before and after:** Draw bar charts for both frequency tables.

### Mark breakdown (8 marks):
- Frequency table: 2 marks
- Complete equalization table: 4 marks
- Histogram plots before and after: 2 marks

---

## TOPIC 3: Laplacian Filter (3 marks — Part A)
**[Appears in every paper]**

### What is the Laplacian Filter? (Simple explanation)
It's a filter used to detect **edges** and **sharp changes** in an image. It finds areas where pixel intensity changes rapidly (which = edges).

### The Filter Masks (two common forms):
```
Mask 1:        Mask 2:
0  1  0        1  1  1
1 -4  1        1 -8  1
0  1  0        1  1  1
```

### How to apply:
Place the 3×3 mask on the image, multiply each value with corresponding pixel, sum all 9 products = output for centre pixel.

### Key property:
- **Positive result** = bright spot in dark area
- **Negative result** = dark spot in bright area
- **Zero** = uniform region (no edge)

### Enhanced image: f(x,y) - Laplacian result = sharpened image

### 3-mark answer:
Describe the Laplacian filter as a second-order derivative filter used for edge detection/image sharpening. Write one of the filter masks. State that it highlights regions of rapid intensity change. For enhancement: subtract Laplacian from original: g(x,y) = f(x,y) - ∇²f(x,y).

---

## TOPIC 4: Edge Detection — Sobel & Prewitt (8 marks)
> 📖 **PDF Reference:** `Module 5/M5-CST304-Ktunotes.in.pdf` → Section: **"Edge Detection"** or **"Sobel Operator"** or **"Prewitt Operator"**. Look for the 3×3 filter mask figures for both Gx and Gy for Sobel and Prewitt. Also look for any figure showing an image before/after applying edge detection.
**[Appears in ALL papers]**

### What is edge detection? (Simple explanation)
Edges in an image = places where pixel brightness changes suddenly. Finding edges helps us identify objects.

**Analogy:** In a photo, the edge of a cup is where bright background suddenly becomes the dark cup. Edge detection highlights these boundaries.

---

### Sobel Edge Detector (4 marks):

Uses two 3×3 masks — one for horizontal edges (Gx), one for vertical edges (Gy):

```
Gx (detects vertical edges):    Gy (detects horizontal edges):
-1  0  +1                        -1  -2  -1
-2  0  +2                         0   0   0
-1  0  +1                        +1  +2  +1
```

**Steps:**
1. Convolve image with Gx mask → get Gx result
2. Convolve image with Gy mask → get Gy result
3. Gradient magnitude: **G = √(Gx² + Gy²)** or approximation: **G = |Gx| + |Gy|**
4. If G > threshold → edge pixel, else not edge

**Key features:**
- Weight of 2 on centre row/column (more sensitive to closer pixels)
- Good at detecting edges even with some noise

---

### Prewitt Edge Detector (4 marks):

Similar to Sobel but uses equal weights:

```
Gx:                              Gy:
-1  0  +1                        -1  -1  -1
-1  0  +1                         0   0   0
-1  0  +1                        +1  +1  +1
```

**Steps:** Same as Sobel — apply Gx, apply Gy, combine: **G = √(Gx² + Gy²)**

**Difference from Sobel:**
| | Sobel | Prewitt |
|---|---|---|
| Centre weight | 2 | 1 |
| Noise sensitivity | Less noisy | Slightly more noisy |
| Accuracy | Slightly better | Simpler |

### For 8-mark answer:
- Sobel mask (Gx + Gy) with explanation: 4 marks
- Prewitt mask (Gx + Gy) with explanation: 4 marks
- Show gradient magnitude formula for both

---

## TOPIC 5: Intensity Thresholding (3 marks — Part A)

### Simple explanation:
Choose a threshold value T. Every pixel brighter than T becomes **white (object)**, every pixel darker than T becomes **black (background)**.

**Formula:**
```
g(x,y) = 1    if f(x,y) > T    (object point)
g(x,y) = 0    if f(x,y) ≤ T    (background point)
```

**Global thresholding:** T is the same for the whole image
**Variable/Adaptive thresholding:** T changes across different regions of image (used when lighting is uneven)

**Used in:** Separating text from paper, medical image analysis, fingerprint detection

---

## TOPIC 6: Region Splitting and Merging (6 marks)
> 📖 **PDF Reference:** `Module 5/M5-CST304-Ktunotes.in.pdf` → Section: **"Region-Based Segmentation"** or **"Region Splitting and Merging"**. Look for the quad-tree diagram showing how an image is split into 4 quadrants recursively — this is the key figure for this topic.

### Region-Based Segmentation (Simple explanation):
Instead of looking at edges, divide image into **regions** where all pixels share similar properties.

### Region Splitting:
- Start with whole image as one region
- If region is NOT homogeneous (not uniform), **split it into 4 quadrants**
- Keep splitting non-uniform regions until all regions are uniform
- This is called a **Quad-tree** approach

```
Step 1: Whole image (not uniform)
Step 2: Split into 4 quadrants
Step 3: Some quadrants are uniform (stop), some are not (split again)
Step 4: Continue until all regions are uniform
```

### Region Merging:
- Start with small regions (or pixels)
- **Merge adjacent regions** if they are similar enough (satisfy some predicate P)
- Continue until no more regions can be merged

### Combined Split-and-Merge:
1. Start with one region = whole image
2. Split non-homogeneous regions
3. Merge adjacent regions that are similar
4. Result = final segmented image

**Predicate P example:** All pixels in region have intensity within range [a, b]

**For 6-mark answer:**
- Region splitting with quad-tree diagram: 3 marks
- Region merging with example: 3 marks

---

## PART A QUICK ANSWERS FOR MODULE V

### Q: Explain use of Laplacian filter (3 marks)
[See Topic 3 above — write filter mask + explanation of use]

### Q: How is intensity thresholding used in image segmentation? (3 marks)
[See Topic 5 above — write the formula + global vs variable thresholding]

---

## EXAM STRATEGY FOR MODULE V

**Q19 (Intensity transformations + Histogram equalization OR Histogram equalization numerical + Edge filters):**
→ If histogram equalization numerical appears → **ALWAYS CHOOSE THIS** — easiest 8 marks in the paper

**Q20 (Edge detectors Sobel/Prewitt + Region splitting/merging):** Also good — masks are easy to memorize

**Recommended: Q19** — Histogram equalization table is 100% formulaic, no guessing needed.

---

## JUNE 2023 HISTOGRAM EQUALIZATION EXAMPLE (4×5 image, 3-bit)

**Image given:**
```
0  1  1  3  4
7  2  5  5  7
6  3  2  1  1
1  4  4  2  1
```
Total pixels = 4×5 = **20**, L = 8 (3-bit), so L-1 = 7

**Step 1: Count pixels at each level:**
| rk | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|---|
| nk | 1 | 5 | 3 | 2 | 3 | 2 | 2 | 2 |

**Step 2: Apply equalization table:**
| rk | nk | pr=nk/20 | CDF | 7×CDF | sk |
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
