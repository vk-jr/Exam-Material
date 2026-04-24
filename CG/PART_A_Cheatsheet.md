# PART A — Complete Answer Cheat Sheet
## All 18 Possible 3-mark Questions | CST304
### Based on ALL 4 Question Papers (May 2019, Dec 2019, May 2023, June 2023)

> **Strategy:** Part A = 30 marks (10 questions × 3 marks). These answers repeat across papers.
> Study all 18 topics below — your exam will pick 10 from this pool.

---

## Q1: List any 6 applications of Computer Graphics (3 marks)
*(Appeared in ALL 4 papers — easiest 3 marks)*
*(0.5 marks each — just list them)*

1. Entertainment & Gaming — movies, video games, 3D animation
2. Computer-Aided Design (CAD) — designing cars, buildings, circuits
3. Medical Imaging — MRI visualization, surgical simulation, 3D anatomy
4. Scientific Visualization — weather maps, molecular models, data charts
5. Geographic Information Systems (GIS) — digital maps, satellite images
6. User Interface (UI) Design — icons, buttons, windows on your phone/PC
7. Education & Training — flight simulators, virtual labs
8. Art & Graphic Design — digital paintings, typography

---

## Q2: Explain architecture of raster scan display system (3 marks)
*(Appeared in May 2023 as Part A, and in ALL papers as Part B)*

**Block Diagram (draw this — 2 marks just for the diagram):**
```
CPU → Display Processor → Frame Buffer → Video Controller → Monitor
         ↑                                    ↑
      System Bus ←—————————————————————————————
                    I/O Devices
```

**Explanation (1 mark):**
- **CPU** sends drawing commands to the Display Processor
- **Display Processor** converts commands to pixel values, stores in Frame Buffer
- **Frame Buffer** (video RAM) stores colour value of every pixel on screen
- **Video Controller** reads frame buffer row-by-row (raster scan) at ~60fps
- **Monitor** displays the pixels

📖 **PDF Reference:** Open `Module 1/M1-CST304-Ktunotes.in.pdf` → look for section **"Raster Scan Display"** or **"Architecture of Raster Graphics System"** (early pages, typically page 5-12). Study the actual block diagram figure in the PDF.

---

## Q3: Differentiate boundary filling and flood filling (3 marks)
*(Appeared in May 2023 — write any 3 differences)*

| Flood Fill | Boundary Fill |
|---|---|
| Area defined by a specific interior colour | Area defined by a specific boundary colour |
| Replaces all pixels of one colour with new colour | Fills everything that is NOT the boundary colour |
| Does not need a defined boundary | Needs a clearly visible boundary |
| May cause unexpected fills if interior has multiple colours | More controlled — stops at boundary |
| Time consuming in some cases | Generally faster |

*(1 mark per difference — write any 3 rows)*

---

## Q4: What are homogeneous coordinates and why are they necessary? (3 marks)
*(Appeared in May 2023 and May 2019)*

**Definition (1.5 marks):**
Homogeneous coordinates represent a 2D point (x, y) as a 3-element vector **(x, y, w)** where w is typically 1. The actual Cartesian coordinates are (x/w, y/w).

**Why necessary (1.5 marks):**
In standard 2D, translation cannot be represented as matrix multiplication with a 2×2 matrix. By adding the homogeneous coordinate (making it 3×3), ALL geometric transformations — translation, rotation, scaling, shear — can be represented as matrix multiplications. This allows multiple transformations to be **combined into one matrix**.

---

## Q5: Derive equations of Window to Viewport transformation (3 marks)
*(Appeared in May 2023, June 2023, Dec 2019)*

**Window** = view region in world coordinates (xw_min, yw_min) to (xw_max, yw_max)
**Viewport** = display region on screen (xv_min, yv_min) to (xv_max, yv_max)

**Equations:**
```
xv = Sx · xw + tx
yv = Sy · yw + ty

Sx = (xv_max - xv_min) / (xw_max - xw_min)
Sy = (yv_max - yv_min) / (yw_max - yw_min)

tx = (xw_max · xv_min - xw_min · xv_max) / (xw_max - xw_min)
ty = (yw_max · yv_min - yw_min · yv_max) / (yw_max - yw_min)
```

📖 **PDF Reference:** Open `Module 3/M3-CST304-Ktunotes.in.pdf` → look for **"Window to Viewport Transformation"** section.

---

## Q6: Explain the 3D viewing pipeline (3 marks)
*(Appeared in May 2023)*

**Diagram (draw this):**
```
MC → [Modeling Transform] → WC → [Viewing Transform] → VC
                                                         ↓
DC ← [Viewport Transform] ← NC ← [Normalization+Clipping] ← [Projection]
```

**One line each:**
- **MC→WC:** Object placed in world space
- **WC→VC:** Scene transformed to camera/eye coordinates
- **VC→Projection:** 3D projected to 2D (parallel or perspective)
- **Clipping+NC:** Cut to view volume, normalize
- **NC→DC:** Map to screen pixels

📖 **PDF Reference:** Open `Module 3/M3-CST304-Ktunotes.in.pdf` → look for **"3D Viewing Pipeline"** or **"Viewing Coordinate System"** section.

---

## Q7: Define connected component (3 marks)
*(Appeared in May 2023)*

Let S represent a subset of pixels in an image. Two pixels **p** and **q** are said to be **connected in S** if there exists a path between them consisting entirely of pixels in S.

For any pixel **p** in S, the **set of all pixels that are connected to it in S** is called a **connected component** of S.

**Types of connectivity:**
- **4-connectivity**: Connected to 4 horizontal/vertical neighbours only
- **8-connectivity**: Connected to all 8 surrounding neighbours (includes diagonals)

---

## Q8: Write short notes on sampling and quantization (3 marks)
*(Appeared in May 2023)*

**Sampling (1.5 marks):**
Converting a continuous image into discrete spatial coordinates by dividing it into a grid of pixels. Each grid position is one sample. More samples = higher **spatial resolution** = sharper image.

**Quantization (1.5 marks):**
Converting the continuous intensity value at each sample into a discrete integer value. For a k-bit image, there are **2^k** possible grey levels (8-bit = 256 levels). Too few levels causes **false contouring** — visible bands in smooth regions.

---

## Q9: Explain use of the Laplacian filter (3 marks)
*(Appeared in May 2023 and June 2023)*

**Filter masks (write one of these):**
```
Mask 1:         Mask 2:
 0  1  0         1  1  1
 1 -4  1         1 -8  1
 0  1  0         1  1  1
```

**Use:**
- **Edge detection** — highlights regions of rapid intensity change (edges)
- **Image sharpening** — subtracting the Laplacian from original enhances fine details

**Formula:** g(x,y) = f(x,y) − ∇²f(x,y)

It is a **second-order derivative filter**, **isotropic** (works equally in all directions), but sensitive to noise.

📖 **PDF Reference:** Open `Module 5/M5-CST304-Ktunotes.in.pdf` → look for **"Laplacian Filter"** section.

---

## Q10: How is intensity thresholding used in image segmentation? (3 marks)
*(Appeared in May 2023)*

**Concept:** Separates image into object vs background based on pixel brightness.

**Formula:**
```
g(x,y) = 1    if f(x,y) > T    ← object pixel (white)
g(x,y) = 0    if f(x,y) ≤ T    ← background pixel (black)
```

**Types:**
- **Global thresholding**: Single T value for the whole image
- **Variable/Adaptive thresholding**: T changes across the image (handles uneven lighting)

**Use case:** Separating text from paper, medical imaging, fingerprint analysis.

---

## Q11: Differentiate aspect ratio and resolution of a raster scan display (3 marks)
*(Appeared in June 2023)*

| Aspect Ratio | Resolution |
|---|---|
| Ratio of width to height of the display | Number of pixels in width × height |
| Example: 16:9, 4:3 | Example: 1920×1080, 1280×720 |
| Describes the shape of the screen | Describes how many pixels are used |
| Does not tell us image sharpness | Higher resolution = more detail |
| Stays same as screen size changes | Changes with screen quality |

*(Write any 3 rows — 1 mark each)*

---

## Q12: Write down the 4-neighbour Flood-filling algorithm (3 marks)
*(Appeared in June 2023)*

**Algorithm (pseudocode):**
```
Procedure FloodFill4(x, y, fillColour, oldColour):
    if (getPixel(x, y) == oldColour):
        setPixel(x, y, fillColour)       ← colour this pixel
        FloodFill4(x+1, y, fillColour, oldColour)   ← right
        FloodFill4(x-1, y, fillColour, oldColour)   ← left
        FloodFill4(x, y+1, fillColour, oldColour)   ← up
        FloodFill4(x, y-1, fillColour, oldColour)   ← down
```

**Explanation:** Starting from a seed pixel, the algorithm checks if the pixel has the old colour. If yes, it fills it with the new colour and recursively calls itself for all 4 neighbours (up, down, left, right — no diagonals).

---

## Q13: Describe the basic 3D transformations (3 marks)
*(Appeared in June 2023 Part A, May 2019 Part A)*

**Three basic 3D transformations:**

**1. Translation** — moves object by (tx, ty, tz):
```
x' = x + tx,  y' = y + ty,  z' = z + tz
```

**2. Scaling** — resizes object by (Sx, Sy, Sz):
```
x' = Sx·x,  y' = Sy·y,  z' = Sz·z
```

**3. Rotation** — rotates about an axis:
- About Z-axis: x'=x·cosθ − y·sinθ, y'=x·sinθ + y·cosθ, z'=z
- About X-axis: y'=y·cosθ − z·sinθ, z'=y·sinθ + z·cosθ, x'=x
- About Y-axis: z'=z·cosθ − x·sinθ, x'=z·sinθ + z·cosθ, y'=y

*(For 3 marks: name the 3 transformations, write the equations for translation and scaling, mention rotation about axes)*

---

## Q14: Illustrate the Oblique Projections (3 marks)
*(Appeared in June 2023)*

**What is Oblique Projection?**
A type of parallel projection where the projection lines hit the view plane at an angle (not 90°). The front face of the object is shown undistorted, but side faces are drawn at an angle.

**Two types:**
```
1. Cavalier Projection:
   - Projection angle = 45°
   - All edges drawn at true length (no foreshortening)
   - Depth lines at 45° to horizontal, at FULL length
   - Looks somewhat unrealistic (depth appears too long)

2. Cabinet Projection:
   - Projection angle = 45°
   - Depth lines at 45° to horizontal, but at HALF length
   - More realistic appearance than Cavalier
   - Used in furniture and cabinet design (hence the name)
```

**Diagram:**
```
        Cavalier          Cabinet
      ___________       ___________
     /          /|     /          /|
    /          / |    /          / |
   |__________|  |   |__________|  |
   |          | /    |          | /
   |__________|/     |__________|/
   (depth = full)    (depth = half)
```

📖 **PDF Reference:** Open `Module 3/M3-CST304-Ktunotes.in.pdf` → look for **"Oblique Projection"** or **"Cavalier and Cabinet Projection"** section.

---

## Q15: Define terms related to pixel of an image: Neighbours, Boundary, Path (3 marks)
*(Appeared in June 2023 — 1 mark each)*

**Neighbours (1 mark):**
Pixels that are adjacent to a given pixel p.
- **4-neighbours:** The 4 pixels directly up, down, left, right of p
- **8-neighbours:** All 8 surrounding pixels including the 4 diagonals

**Boundary (1 mark):**
The set of pixels in a region R that have at least one neighbour that does NOT belong to R. It is the "edge" of a region — pixels that are inside the region but touching the outside.

**Path (1 mark):**
A sequence of distinct pixels (p₁, p₂, p₃, ..., pₙ) where each consecutive pair of pixels are neighbours. The length of the path is the number of pixel pairs in it (n−1 steps).

---

## Q16: Differentiate grayscale image and colour image (3 marks)
*(Appeared in June 2023)*

| Grayscale Image | Colour Image |
|---|---|
| Each pixel has ONE intensity value (0-255) | Each pixel has 3 values (R, G, B) |
| 1 channel | 3 channels (RGB) |
| 8 bits per pixel | 24 bits per pixel (8 per channel) |
| Shows only brightness — no colour | Shows full colour |
| File size smaller | File size 3× larger |

---

## Q17: Write the algorithm for basic Global Thresholding (3 marks)
*(Appeared in June 2023)*

**Global Thresholding Algorithm:**
```
Step 1: Select an initial estimate for T (e.g., T = average pixel intensity of image)
Step 2: Segment image using T:
        G1 = all pixels with intensity > T  (object)
        G2 = all pixels with intensity ≤ T  (background)
Step 3: Compute new threshold:
        T_new = (mean of G1 + mean of G2) / 2
Step 4: If |T_new - T| < small value (e.g., 0.5):
        STOP — T_new is the final threshold
        Else: T = T_new, go to Step 2
Step 5: Apply final T to segment the image
```

**Key idea:** This is an **iterative** method — it keeps refining the threshold until it converges to a stable value.

---

## Q18: Compare Smoothing and Sharpening techniques (3 marks)
*(Appeared in June 2023)*

| Smoothing | Sharpening |
|---|---|
| Reduces noise and fine detail | Enhances edges and fine detail |
| Uses **averaging / low-pass** filters | Uses **derivative / high-pass** filters |
| Makes image look blurry | Makes image look crisper |
| Examples: Mean filter, Gaussian filter | Examples: Laplacian, Sobel, Prewitt |
| Removes high-frequency components | Amplifies high-frequency components |
| Used to reduce noise in image | Used to highlight object boundaries |

---

## BONUS: Frame Buffer Size Calculation (sometimes in Part A)
*(Appeared in Dec 2019 and May 2019 as Part A)*

**Formula:** Frame buffer size = Width × Height × Bits per pixel (in bits)
Divide by 8 to get bytes, divide by 1024 for KB, by 1024 again for MB.

**Example (Dec 2019):** 8 inches × 100 ppi = 800 pixels wide; 10 inches × 100 ppi = 1000 pixels tall; 6 bits per pixel
= 800 × 1000 × 6 bits = 4,800,000 bits = 600,000 bytes = **600 KB**
