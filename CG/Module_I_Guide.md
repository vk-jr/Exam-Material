# MODULE I — Raster Graphics, Line & Circle Drawing
## CST304 Computer Graphics | Study Guide

> **Exam weight:** 3 marks (Part A Q1 or Q2) + 14 marks (Part B Q11 or Q12)
> **Appeared in:** ALL 4 question papers — this module is MUST PREPARE
>
> **How to use figures:** During our chat sessions, I create simple ASCII diagrams to help you understand. For the real detailed figure, each topic below tells you exactly which PDF section to open. Open the PDF alongside our chat — study my diagram first, then study the real figure to lock it in memory.
>
> 📂 **Your PDF for this module:** `Module 1/M1-CST304-Ktunotes.in.pdf`

---

## TOPIC 1: Applications of Computer Graphics (3 marks)
**[Appears in EVERY paper — easiest 3 marks in Part A]**

Just list 6 applications, 0.5 marks each:

1. **Entertainment** — movies, video games, animation (Pixar, gaming)
2. **CAD/CAM** — designing cars, buildings, machines
3. **Medical imaging** — MRI scans, X-rays, 3D body models
4. **Education** — simulations, virtual labs
5. **Geographic Information Systems (GIS)** — maps, satellite imagery
6. **User Interfaces (UI)** — icons, windows, buttons on your phone/PC
7. **Scientific visualization** — weather maps, molecular structures
8. **Simulation & Training** — flight simulators, military training

**Exam tip:** Just list any 6. One line each is enough.

---

## TOPIC 2: Raster Scan Display Architecture (3 marks in Part A, 6 marks in Part B)
> 📖 **PDF Reference:** `Module 1/M1-CST304-Ktunotes.in.pdf` → Section: **"Raster Scan Display"** or **"Architecture of Raster Graphics System"**. Look for the block diagram figure showing CPU → Display Processor → Frame Buffer → Video Controller → Monitor.

### What is a Raster Scan Display? (Simple explanation)
Think of your TV or computer monitor. It's made up of thousands of tiny dots called **pixels**. The screen draws images by lighting up these dots row by row, from top-left to bottom-right, very fast (60+ times per second). This is called **raster scanning**.

### Architecture Diagram:
```
CPU  ──►  Display Processor  ──►  Frame Buffer (Memory)
                                        │
                              Video Controller
                                        │
                                    Monitor
                                   (Screen)
```

### Key Components:
| Component | What it does | Simple analogy |
|---|---|---|
| **CPU** | Sends drawing commands | Like giving instructions |
| **Display Processor** | Converts commands to pixel data | Like a translator |
| **Frame Buffer** | Stores the image as pixels | Like a photograph of the screen |
| **Video Controller** | Reads frame buffer, sends to screen | Like a film projector |
| **Monitor** | Displays the pixels | The actual screen |

### Frame Buffer:
- Stores the **colour of every pixel** on screen
- A 1024×768 screen with 24-bit colour needs: 1024 × 768 × 24 bits = **~2.36 MB**
- Higher resolution = more memory needed

### For 3-mark answer: Draw the diagram + 2 lines of explanation
### For 6-mark answer: Draw diagram + explain each component (2-3 lines each)

---

## TOPIC 3: DDA Line Drawing Algorithm (7-10 marks — NUMERICAL)
> 📖 **PDF Reference:** `Module 1/M1-CST304-Ktunotes.in.pdf` → Section: **"DDA Line Drawing Algorithm"**. Look for the step table and the plotted pixel grid diagram.
**[Appears in ALL 3 papers — must practice the steps]**

### What is it? (Simple explanation)
DDA = **Digital Differential Analyzer**. It's a method to draw a line on a pixel screen. Since screens have only whole-number pixel positions, we need to figure out which pixels to light up between two points.

**Think of it like:** You want to walk from point A to point B on a grid. DDA tells you which grid squares to step on.

### The Algorithm:

**Step 1:** Calculate slope m = (y2 - y1) / (x2 - x1)

**Step 2:** Decide which direction to step:
- If |m| ≤ 1 → step in x direction (x increases by 1 each step)
  - x(k+1) = x(k) + 1
  - y(k+1) = y(k) + m
- If |m| > 1 → step in y direction (y increases by 1 each step)
  - y(k+1) = y(k) + 1
  - x(k+1) = x(k) + 1/m

**Step 3:** Round each result to nearest integer for pixel position.

**Step 4:** Start from (x1, y1), repeat until you reach (x2, y2).

---

### WORKED EXAMPLE (From May 2023 paper — 7 marks)
**Plot line from (-2, -4) to (4, 3)**

**Step 1:** m = (3 - (-4)) / (4 - (-2)) = 7/6 = **1.167**

**Step 2:** Since |m| > 1, step in Y direction
- y(k+1) = y(k) + 1
- x(k+1) = x(k) + 1/m = x(k) + **0.857**

**Step 3:** Build the table starting at (-2, -4):

| Step | x (calculated) | x (rounded) | y (calculated) | y (rounded) |
|---|---|---|---|---|
| Start | -2 | **-2** | -4 | **-4** |
| 1 | -2 + 0.857 = -1.143 | **-1** | -4 + 1 = -3 | **-3** |
| 2 | -1.143 + 0.857 = -0.286 | **0** | -3 + 1 = -2 | **-2** |
| 3 | -0.286 + 0.857 = 0.571 | **1** | -2 + 1 = -1 | **-1** |
| 4 | 0.571 + 0.857 = 1.428 | **1** | -1 + 1 = 0 | **0** |
| 5 | 1.428 + 0.857 = 2.285 | **2** | 0 + 1 = 1 | **1** |
| 6 | 2.285 + 0.857 = 3.142 | **3** | 1 + 1 = 2 | **2** |
| 7 | 3.142 + 0.857 = 3.999 | **4** | 2 + 1 = 3 | **3** |

**Pixels to plot:** (-2,-4), (-1,-3), (0,-2), (1,-1), (1,0), (2,1), (3,2), (4,3)

### Mark breakdown (7 marks):
- Finding slope: 1 mark
- Correct update equations: 2 marks
- Each pixel coordinate pair: 0.5 marks × 8 pairs = 4 marks

### Advantages of DDA:
1. Simple to implement
2. Faster than direct calculation

### Disadvantages of DDA:
1. Floating point arithmetic is slow
2. Rounding errors can accumulate
3. Not as fast as Bresenham's algorithm

---

## TOPIC 4: Bresenham's Line Drawing Algorithm (7 marks — NUMERICAL + DERIVATION)
> 📖 **PDF Reference:** `Module 1/M1-CST304-Ktunotes.in.pdf` → Section: **"Bresenham's Line Drawing Algorithm"**. Look for the derivation diagram showing decision parameter and the pixel grid with E (East) vs NE (NorthEast) pixel choices.
**[Appears in ALL 3 papers — most important numerical]**

### Why Bresenham's is better than DDA?
DDA uses **division and rounding** (slow). Bresenham uses only **integer addition** (fast). No floating point!

### The Idea (Super simple):
At each step, you're at some pixel. The next pixel is either directly to the right, or diagonally up-right. Bresenham uses a **decision parameter (p)** to choose which one.

### The Decision Parameter Formula:
**Initial:** p₀ = 2Δy - Δx (where Δx = x2-x1, Δy = y2-y1)

**At each step:**
- If p(k) < 0: next pixel is (x+1, y) → p(k+1) = p(k) + 2Δy
- If p(k) ≥ 0: next pixel is (x+1, y+1) → p(k+1) = p(k) + 2Δy - 2Δx

*(This is for slope 0 ≤ m ≤ 1. For other slopes, rules change slightly)*

### WORKED EXAMPLE (From June 2023 paper — 8 marks)
**Plot line from (1,1) to (8,5)**

**Step 1:** Δx = 8-1 = 7, Δy = 5-1 = 4
m = 4/7 ≈ 0.57 → slope ≤ 1, so step in x direction ✓

**Step 2:** Initial decision parameter: p₀ = 2Δy - Δx = 2(4) - 7 = **1**

**Step 3:** Build the table:

| k | p(k) | x(k+1) | y(k+1) | p(k+1) |
|---|---|---|---|---|
| 0 | p₀ = **1** ≥ 0 | 1+1=2 | 1+1=2 | 1 + 2(4) - 2(7) = **-5** |
| 1 | **-5** < 0 | 2+1=3 | 2 (no change) | -5 + 2(4) = **3** |
| 2 | **3** ≥ 0 | 3+1=4 | 2+1=3 | 3 + 8 - 14 = **-3** |
| 3 | **-3** < 0 | 4+1=5 | 3 (no change) | -3 + 8 = **5** |
| 4 | **5** ≥ 0 | 5+1=6 | 3+1=4 | 5 + 8 - 14 = **-1** |
| 5 | **-1** < 0 | 6+1=7 | 4 (no change) | -1 + 8 = **7** |
| 6 | **7** ≥ 0 | 7+1=8 | 4+1=5 | — |

**Pixels plotted:** (1,1), (2,2), (3,2), (4,3), (5,3), (6,4), (7,4), (8,5) ✓

### Mark breakdown (8 marks):
- Δx, Δy calculation: 1 mark
- Initial p₀ formula: 1 mark
- Update rules written: 2 marks
- Table with all pixel pairs: 4 marks (0.5 each)

---

## TOPIC 5: Bresenham's Circle Drawing Algorithm (7 marks — NUMERICAL)
> 📖 **PDF Reference:** `Module 1/M1-CST304-Ktunotes.in.pdf` → Section: **"Bresenham's Circle Drawing Algorithm"** or **"Midpoint Circle Algorithm"**. Look for the 8-way symmetry diagram showing all 8 octants of a circle.
**[Appears in ALL 3 papers]**

### The Idea:
Drawing a circle on a pixel grid. We draw only 1/8th of the circle (one octant) and then use **8-way symmetry** to get all 8 parts. This is very efficient!

### Starting point: (0, r) — top of the circle

### Decision Parameter:
**Initial:** d₀ = 3 - 2r

**At each step starting from (xi, yi):**
- If d(i) < 0: next point is (xi+1, yi) → d(i+1) = d(i) + 4xi + 6
- If d(i) ≥ 0: next point is (xi+1, yi-1) → d(i+1) = d(i) + 4(xi - yi) + 10

**Stop when** x ≥ y

### 8-way Symmetry Rule:
If one point on the circle (relative to center) is (x, y), then all 8 symmetric points are:
```
(x,y), (-x,y), (x,-y), (-x,-y)
(y,x), (-y,x), (y,-x), (-y,-x)
```
To get actual screen coordinates: add center (h,k) to each point.

### WORKED EXAMPLE (From May 2023 paper — 7 marks)
**Circle with centre (5,3), radius 5**

**Step 1:** Start at (0, 5) relative to origin.
d₀ = 3 - 2(5) = **-7**

**Step 2:** Build table (working relative to origin):

| Step | xi | yi | d(i) | Decision | xi+1 | yi+1 | d(i+1) |
|---|---|---|---|---|---|---|---|
| 1 | 0 | 5 | -7 < 0 | keep y | 1 | 5 | -7+4(0)+6 = **-1** |
| 2 | 1 | 5 | -1 < 0 | keep y | 2 | 5 | -1+4(1)+6 = **9** |
| 3 | 2 | 5 | 9 ≥ 0 | decrease y | 3 | 4 | 9+4(2-5)+10 = **7** |
| 4 | 3 | 4 | 7 ≥ 0 | decrease y | 4 | 3 | — |
| Stop | x=4 ≥ y=3 | STOP | | | | | |

**Points in first octant (relative to origin):** (0,5), (1,5), (2,5), (3,4)

**Step 3:** Add center offset (h=5, k=3):

| xi + h | yi + k |
|---|---|
| 0+5 = **5** | 5+3 = **8** |
| 1+5 = **6** | 5+3 = **8** |
| 2+5 = **7** | 5+3 = **8** |
| 3+5 = **8** | 4+3 = **7** |

**Step 4:** Use 8-way symmetry to find remaining points (mention this in exam for 1 mark)

### Mark breakdown (7 marks):
- d₀ calculation: 1 mark
- Correct decision rules: 2 marks
- Table with first octant points: 3 marks
- 8-way symmetry mention: 1 mark

---

## PART A QUICK ANSWERS FOR MODULE I

### Q: List any 6 applications of Computer Graphics (3 marks)
Entertainment, CAD/CAM, Medical Imaging, Education/Simulation, GIS/Maps, UI Design, Scientific Visualization, Flight Simulation — pick any 6.

### Q: Explain the architecture of raster scan display system (3 marks)
**Answer:** A raster scan system has: CPU → Display Processor → Frame Buffer → Video Controller → Monitor. The frame buffer stores the colour value of every pixel. The video controller reads the frame buffer row by row (raster scan) and sends signals to the monitor to light up the correct pixels. [Draw the block diagram for full 3 marks]

---

## EXAM STRATEGY FOR MODULE I

**Q11 (Bresenham Line derivation + DDA numerical OR Raster architecture + Bresenham Line):**
→ **ALWAYS CHOOSE Q11** — both parts have exact steps, no guessing needed

**Q12 (CRT theory + Bresenham Circle OR DDA + advantages/disadvantages):**
→ Choose only if you can't do Q11

**Recommended choice: Q11** — Line algorithms are the most predictable and practiced

---

## NEW TOPICS ADDED (from all 4 papers)

### Beam Penetration CRT & Shadow Mask Method (7 marks — appeared in 3 papers)
> 📖 **PDF Reference:** `Module 1/M1-CST304-Ktunotes.in.pdf` → Section: **"Colour CRT Monitors"** or **"Beam Penetration CRT"**. Look for the diagram showing electron beam and phosphor layers.

**Beam Penetration Method:**
- Has **two layers of phosphor** coated inside the CRT — red (outer) and green (inner)
- Slow electrons → hit only outer red layer → **red colour**
- Fast electrons → penetrate deeper to inner green layer → **green colour**
- Medium speed → mix of red and green → **orange or yellow**
- Limited colours, poor quality — mostly used in older vector graphics systems

**Shadow Mask Method:**
- Uses **three electron guns** (red, green, blue — RGB)
- A thin metal **shadow mask** with tiny holes sits in front of the phosphor screen
- Each gun fires electrons through the holes, each hitting only its own colour phosphor dot
- Mixing the three guns → full colour range
- Used in all modern colour monitors

**Key difference:** Beam penetration gives few colours; shadow mask gives full RGB colour.
