# MODULE 1 — Complete Study Notes
## CST304 Computer Graphics | All Topics + Exam Answers

---

## TOPIC 1 — Applications of Computer Graphics
**[3 marks | Appears in EVERY paper]**

### Exam Answer (list any 6):

1. **Entertainment** — Used in video games, animated movies, and visual effects.
2. **GIS (Geographic Information System)** — Used in satellite imagery, weather maps, and navigation systems.
3. **User Interfaces (UI)** — Used to render icons, windows, and buttons on screens.
4. **CAD/CAM** — Used to design and manufacture cars, buildings, and mechanical parts.
5. **Education & Simulation** — Used in virtual labs and flight/military simulators.
6. **Medical Imaging** — Used to visualize MRI scans, X-rays, and 3D body models.

> Bonus: Scientific Visualization (weather maps, molecular structures), Military Training

**Tip:** Any 6 applications + one line each = full 3 marks. No paragraph needed.

---

## TOPIC 2 — Raster Scan Display Architecture
**[3 marks Part A | 6 marks Part B | Appears in ALL papers]**

### What is it?
A raster scan display lights up pixels **row by row, left to right, top to bottom**, 60+ times per second. This sweeping process is called raster scanning.

### Block Diagram (draw this in exam):

```
CPU → Display Processor → Frame Buffer → Video Controller → Monitor
```

### Components:

| Component | Role |
|---|---|
| **CPU** | Sends high-level drawing commands |
| **Display Processor** | Converts commands into pixel data, writes to frame buffer |
| **Frame Buffer** | Memory that stores the colour value of every pixel |
| **Video Controller** | Reads frame buffer row by row, sends signal to monitor |
| **Monitor** | Receives signals and lights up the correct pixels |

### Frame Buffer Detail:
- Stores colour of **every pixel** on screen
- Memory required = Width × Height × Bits per pixel
- Example: 1024 × 768 × 24-bit colour = **~2.36 MB**
- Higher resolution = more memory needed

### For 3-mark answer:
Draw diagram + 1 line per component.

### For 6-mark answer:
Draw diagram + 2–3 lines per component + include frame buffer memory calculation.

---

## TOPIC 3 — DDA Line Drawing Algorithm
**[7 marks | Numerical | Appears in ALL papers]**

### What is DDA?
DDA = **Digital Differential Analyzer**. It draws a line on a pixel screen by stepping pixel by pixel and rounding to nearest integer position.

**Simple idea:** You want to go from point A to point B on a grid. DDA tells you which grid squares to step on.

### Algorithm Steps:

**Step 1:** Calculate slope → m = (y2 - y1) / (x2 - x1)

**Step 2:** Decide step direction:
- If **|m| ≤ 1** → step in X direction
  - x(k+1) = x(k) + 1
  - y(k+1) = y(k) + m
- If **|m| > 1** → step in Y direction
  - y(k+1) = y(k) + 1
  - x(k+1) = x(k) + 1/m

**Step 3:** Round each result to nearest integer for pixel position.

**Step 4:** Start from (x1, y1), repeat until (x2, y2).

---

### Worked Example — Plot line from (-2, -4) to (4, 3)

**Step 1:** m = (3 - (-4)) / (4 - (-2)) = 7/6 = **1.167**

**Step 2:** |m| > 1 → step in Y direction
- y(k+1) = y(k) + 1
- x(k+1) = x(k) + 1/m = x(k) + **0.857**

**Step 3:** Table (start at (-2, -4)):

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

### Mark Breakdown (7 marks):
- Finding slope: 1 mark
- Correct update equations: 2 marks
- Each pixel pair in table: 0.5 × 8 = 4 marks

### Advantages of DDA:
1. Simple to implement
2. Faster than direct line calculation

### Disadvantages of DDA:
1. Uses floating point arithmetic (slow)
2. Rounding errors accumulate over long lines
3. Slower than Bresenham's algorithm

---

## TOPIC 4 — Bresenham's Line Drawing Algorithm
**[8 marks | Derivation + Numerical | Appears in ALL papers — MOST IMPORTANT]**

### Why Bresenham over DDA?
DDA uses **floating point division and rounding** (slow).
Bresenham uses only **integer addition** (fast). No floating point at all.

### The Idea:
At each pixel, the next pixel is either:
- **East (E):** directly to the right → (x+1, y)
- **North-East (NE):** diagonally up-right → (x+1, y+1)

A **decision parameter p** tells you which one to pick.

### Formulas (for slope 0 ≤ m ≤ 1):

**Initial decision parameter:**
> p₀ = 2Δy - Δx

where Δx = x2 - x1, Δy = y2 - y1

**Update rules:**
- If **p(k) < 0** → choose E → next pixel: (x+1, y) → p(k+1) = p(k) + 2Δy
- If **p(k) ≥ 0** → choose NE → next pixel: (x+1, y+1) → p(k+1) = p(k) + 2Δy - 2Δx

---

### Worked Example — Plot line from (1,1) to (8,5)

**Step 1:** Δx = 8-1 = 7, Δy = 5-1 = 4
m = 4/7 ≈ 0.57 → slope ≤ 1 ✓ (use x-step formulas)

**Step 2:** p₀ = 2(4) - 7 = **1**

**Step 3:** Table:

| k | p(k) | Decision | x(k+1) | y(k+1) | p(k+1) |
|---|---|---|---|---|---|
| 0 | **1** ≥ 0 | NE | 2 | 2 | 1 + 2(4) - 2(7) = **-5** |
| 1 | **-5** < 0 | E | 3 | 2 | -5 + 2(4) = **3** |
| 2 | **3** ≥ 0 | NE | 4 | 3 | 3 + 8 - 14 = **-3** |
| 3 | **-3** < 0 | E | 5 | 3 | -3 + 8 = **5** |
| 4 | **5** ≥ 0 | NE | 6 | 4 | 5 + 8 - 14 = **-1** |
| 5 | **-1** < 0 | E | 7 | 4 | -1 + 8 = **7** |
| 6 | **7** ≥ 0 | NE | 8 | 5 | — (end) |

**Pixels plotted:** (1,1), (2,2), (3,2), (4,3), (5,3), (6,4), (7,4), (8,5) ✓

### Mark Breakdown (8 marks):
- Δx, Δy calculation: 1 mark
- p₀ formula: 1 mark
- Update rules written: 2 marks
- Table with all pixel pairs: 4 marks (0.5 each)

### Advantages of Bresenham:
1. Uses only integer arithmetic — very fast
2. No rounding errors
3. More accurate than DDA

---

## TOPIC 5 — Bresenham's Circle Drawing Algorithm
**[7 marks | Numerical | Appears in ALL papers]**

### The Idea:
Draw only **1/8th of the circle** (first octant), then use **8-way symmetry** to get all 8 parts. Very efficient.

### Starting point: (0, r) — top of circle

### Decision Parameter:

**Initial:**
> d₀ = 3 - 2r

**Update rules (starting from point (xi, yi)):**
- If **d(i) < 0** → next point: (xi+1, yi) → d(i+1) = d(i) + 4xi + 6
- If **d(i) ≥ 0** → next point: (xi+1, yi-1) → d(i+1) = d(i) + 4(xi - yi) + 10

**Stop when x ≥ y**

### 8-Way Symmetry Rule:
If one point is (x, y) relative to center, the 8 symmetric points are:
```
(x, y)   (-x, y)   (x, -y)   (-x, -y)
(y, x)   (-y, x)   (y, -x)   (-y, -x)
```
Add center (h, k) to each to get actual screen coordinates.

---

### Worked Example — Circle with centre (5,3), radius 5

**Step 1:** Start at (0, 5) relative to origin.
d₀ = 3 - 2(5) = **-7**

**Step 2:** Table (relative to origin, stop when x ≥ y):

| Step | xi | yi | d(i) | Decision | xi+1 | yi+1 | d(i+1) |
|---|---|---|---|---|---|---|---|
| 1 | 0 | 5 | -7 < 0 | keep y | 1 | 5 | -7 + 4(0) + 6 = **-1** |
| 2 | 1 | 5 | -1 < 0 | keep y | 2 | 5 | -1 + 4(1) + 6 = **9** |
| 3 | 2 | 5 | 9 ≥ 0 | decrease y | 3 | 4 | 9 + 4(2-5) + 10 = **7** |
| 4 | 3 | 4 | 7 ≥ 0 | decrease y | 4 | 3 | — |
| STOP | x=4 ≥ y=3 | | | | | | |

**First octant points (relative to origin):** (0,5), (1,5), (2,5), (3,4)

**Step 3:** Add center offset (h=5, k=3):

| Point | x + 5 | y + 3 |
|---|---|---|
| (0,5) | 5 | 8 |
| (1,5) | 6 | 8 |
| (2,5) | 7 | 8 |
| (3,4) | 8 | 7 |

**Step 4:** Apply 8-way symmetry for remaining 7 octants (mention this in exam for 1 mark).

### Mark Breakdown (7 marks):
- d₀ calculation: 1 mark
- Correct decision rules written: 2 marks
- Table with first octant points: 3 marks
- Mention of 8-way symmetry: 1 mark

---

## TOPIC 6 — Beam Penetration CRT & Shadow Mask Method
**[7 marks | Theory | Appeared in 3 papers]**

### Beam Penetration Method:

- CRT has **two phosphor layers** inside — red (outer) and green (inner)
- **Slow electrons** → hit only outer layer → **red colour**
- **Fast electrons** → penetrate deeper → **green colour**
- **Medium speed** → mix of both → **orange or yellow**

**Drawback:** Very limited colour range. Used only in older vector graphics systems.

```
Electron Gun → [Red Phosphor (outer)] [Green Phosphor (inner)] → Screen
```

### Shadow Mask Method:

- Uses **three separate electron guns** — Red, Green, Blue (RGB)
- A thin metal **shadow mask** (with tiny holes) sits in front of the phosphor screen
- Each gun fires through the holes, hitting only its own colour phosphor dot
- Mixing all three guns → **full colour range**
- Used in **all modern colour CRT monitors**

```
Red Gun   →  |  |  |  → Red phosphor dots
Green Gun →  |  |  |  → Green phosphor dots   → Screen
Blue Gun  →  |  |  |  → Blue phosphor dots
              Shadow Mask
```

### Key Comparison Table:

| Feature | Beam Penetration | Shadow Mask |
|---|---|---|
| Number of guns | 1 | 3 (R, G, B) |
| Colour range | Very limited (red/orange/yellow/green) | Full RGB colour |
| Quality | Poor | High |
| Used in | Old vector displays | Modern monitors |

### For 7-mark answer:
- Explain both methods (3 marks each) + comparison table or key difference (1 mark)

---

## EXAM STRATEGY — Module I

| Question | What to expect | Recommended choice |
|---|---|---|
| **Q11** | Bresenham Line derivation + DDA numerical OR Raster architecture + Bresenham | **Always choose Q11** |
| **Q12** | CRT theory + Bresenham Circle OR DDA + advantages | Choose only if Q11 looks hard |

**Best choice: Q11** — Line algorithms are step-by-step tables, most predictable.

---

## QUICK REVISION SUMMARY

| Topic | What to remember |
|---|---|
| Applications | Any 6: Entertainment, GIS, UI, CAD, Education, Medical |
| Raster Architecture | 5 components + block diagram + frame buffer formula |
| DDA | Slope → if |m|>1 step Y else step X → round → table |
| Bresenham Line | p₀ = 2Δy - Δx → if p<0: +2Δy, if p≥0: +2Δy-2Δx → table |
| Bresenham Circle | d₀ = 3-2r → if d<0: keep y, if d≥0: decrease y → 8-way symmetry |
| CRT Methods | Beam penetration = 2 phosphor layers, limited colour; Shadow mask = 3 guns, full RGB |
