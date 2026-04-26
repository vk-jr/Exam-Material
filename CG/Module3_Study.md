# MODULE 3 — Clipping, Projections & 3D Viewing
## CST304 Computer Graphics | All Topics + Exam Answers

---

## TOPIC 1 — Window to Viewport Transformation
**[3 marks | Part A | Appears in every paper]**

### Simple Explanation:
- **Window** = the region of the world you want to see (in world/scene coordinates)
- **Viewport** = the region on screen where it gets displayed (in screen coordinates)

**Analogy:** Window = what a camera sees. Viewport = the TV screen where it's shown.

### The Transformation Equations:

```
xv = Sx · xw + tx
yv = Sy · yw + ty

Where:
Sx = (xv_max - xv_min) / (xw_max - xw_min)    ← x scaling factor
Sy = (yv_max - yv_min) / (yw_max - yw_min)    ← y scaling factor

tx = (xw_max · xv_min - xw_min · xv_max) / (xw_max - xw_min)
ty = (yw_max · yv_min - yw_min · yv_max) / (yw_max - yw_min)
```

### 3-mark answer:
Write the two equations (xv = ... and yv = ...) and the formulas for Sx, Sy, tx, ty.

---

## TOPIC 2 — Cohen-Sutherland Line Clipping Algorithm
**[6-8 marks | Numerical | Appears in ALL papers]**

### What is Clipping? (Simple explanation)
When you draw a line, parts of it may go outside the visible window. Clipping removes those outside parts. Like using scissors to cut a drawing to fit inside a frame.

### Region Codes — The Core Idea:
Divide the screen into 9 regions. Each region gets a **4-bit code**:

```
         |          |
  1001   |  1000    |  1010
         |          |
---------|----------|----------  ← y_max (Top boundary)
         |          |
  0001   |  0000    |  0010
         | (INSIDE) |
---------|----------|----------  ← y_min (Bottom boundary)
         |          |
  0101   |  0100    |  0110
         |          |
      x_min       x_max
```

**Bit meaning (from right to left — memorize as: Left Right Below Above):**
- Bit 1 (Left): x < x_min → set to 1
- Bit 2 (Right): x > x_max → set to 1
- Bit 3 (Below): y < y_min → set to 1
- Bit 4 (Above): y > y_max → set to 1

**Memory trick: LRBA — Left Right Below Above**

### Algorithm Steps:
1. **Assign region codes** to both endpoints P1 and P2
2. **Check conditions:**
   - If code(P1) **OR** code(P2) = **0000** → line entirely inside → **Accept** ✓
   - If code(P1) **AND** code(P2) ≠ **0000** → both outside same region → **Reject** ✗
   - Otherwise → partially visible → **Clip it**
3. For clipping: find intersection of line with boundary, replace outside point with intersection, repeat

### Intersection Formulas:
```
At left boundary   (x = x_min):  y = y1 + m(x_min - x1)
At right boundary  (x = x_max):  y = y1 + m(x_max - x1)
At bottom boundary (y = y_min):  x = x1 + (y_min - y1)/m
At top boundary    (y = y_max):  x = x1 + (y_max - y1)/m

where m = (y2 - y1) / (x2 - x1)
```

---

### Worked Example — From May 2023 (6 marks)
**Clipping window:** x_min=0, x_max=8, y_min=0, y_max=5
**Line:** P1(-1, -2) to P2(9, 7)

**Step 1: Assign region codes**
- P1(-1,-2): x=-1 < 0 → Left bit=1; y=-2 < 0 → Below bit=1 → Code = **0101**
- P2(9,7): x=9 > 8 → Right bit=1; y=7 > 5 → Above bit=1 → Code = **1010**

**Step 2: Check**
- OR = 0101 | 1010 = **1111** ≠ 0000 → not trivially accepted
- AND = 0101 & 1010 = **0000** → not rejected → partially visible → CLIP

**Step 3: Find intersections**
m = (7-(-2)) / (9-(-1)) = 9/10 = **0.9**

**Clip P1 (code 0101 — has Left and Below bits):**
- Start with Below boundary (y = y_min = 0):
  x = x1 + (y_min - y1)/m = -1 + (0-(-2))/0.9 = -1 + 2.22 = **1.22**
  → New P1 = **(1.22, 0)**
- Check new P1 code: 0 < 1.22 < 8 ✓, y=0 is at boundary ✓ → Code = **0000** ✓

**Clip P2 (code 1010 — has Right and Above bits):**
- Start with Top boundary (y = y_max = 5):
  x = x1 + (y_max - y1)/m = -1 + (5-(-2))/0.9 = -1 + 7.78 = **6.78**
  → New P2 = **(6.78, 5)**

**Final clipped line: (1.22, 0) to (6.78, 5) ✓**

### Mark Breakdown (6 marks):
- Region codes for both endpoints: 1 mark
- OR/AND accept/reject test: 2 marks
- m calculation: 1 mark
- Intersection calculations: 2 marks

---

### Another Example — From June 2023
**Window:** x_min=1, x_max=6, y_min=1, y_max=5
**Line:** P1(0,0) to P2(7,6)

**Step 1: Codes**
- P1(0,0): x=0 < 1 → Left; y=0 < 1 → Below → Code = **0101**
- P2(7,6): x=7 > 6 → Right; y=6 > 5 → Above → Code = **1010**

**Step 2:** AND = 0000 → clip needed

m = (6-0)/(7-0) = 6/7 ≈ 0.857

**Clip P1 at bottom (y=1):** x = 0 + (1-0)/0.857 = **1.17** → New P1 = (1.17, 1)
**Clip P2 at top (y=5):** x = 0 + (5-0)/0.857 = **5.83** → New P2 = (5.83, 5)

**Clipped line: (1.17, 1) to (5.83, 5)** ✓

---

## TOPIC 3 — Types of Projections
**[8 marks | Diagram = 4 marks alone | Appears in ALL papers]**

### What is Projection? (Simple explanation)
3D objects must be converted to 2D for display on a flat screen. Projection does this — like making a shadow of a 3D object on a flat wall.

### TAXONOMY DIAGRAM — Draw this in exam (4 marks just for this):

```
                         PROJECTIONS
                        /            \
              PARALLEL               PERSPECTIVE
             /        \               /    |    \
      Orthographic   Oblique        1-pt  2-pt  3-pt
       /      \       /   \       (vanishing points)
   Multi-   Axono-  Cav-  Cab-
   view     metric  alier inet
             /|\
          Iso  Di  Tri
         metric metric metric
```

---

### Explanation of Each Type:

#### PARALLEL PROJECTIONS (projection lines are parallel to each other):

**1. Orthographic — Multi-view:**
- Lines of projection are **perpendicular** to the view plane
- Shows object from multiple views: **front, top, side** separately
- Used in engineering/architectural drawings

**2. Orthographic — Axonometric:**
All three views combined into one. Three sub-types:
- **Isometric:** Equal angles (120°) between the three principal axes. Most commonly used. Looks natural and balanced.
- **Dimetric:** Two of the three angles are equal.
- **Trimetric:** All three angles are different.

**3. Oblique:**
- Lines of projection are at an **angle** to the view plane (not perpendicular)
- Two types:
  - **Cavalier:** Depth lines drawn at full length (no foreshortening). Lengths preserved in all directions.
  - **Cabinet:** Depth lines drawn at **half length** (foreshortened by 0.5). Looks more realistic than Cavalier.

#### PERSPECTIVE PROJECTIONS (projection lines converge at a point):
- More **realistic** — like human vision
- Objects farther away appear smaller
- **One-point perspective:** One vanishing point (e.g., looking down a railway track)
- **Two-point perspective:** Two vanishing points (e.g., corner of a building)
- **Three-point perspective:** Three vanishing points (e.g., looking up at a skyscraper)

### For 8-mark answer:
- Taxonomy diagram: 3-4 marks
- Orthographic: 1 mark
- Axonometric (isometric/dimetric/trimetric): 2 marks
- Oblique (cavalier/cabinet): 1 mark
- Perspective (1,2,3 point): 1 mark

---

## TOPIC 4 — 3D Viewing Pipeline
**[3 marks | Part A | Appeared in May 2023]**

### The Pipeline — Draw this diagram:

```
MC → [Modeling Transform] → WC → [Viewing Transform] → VC → [Projection] → NC → [Clipping + Viewport] → DC
```

### What each step means:

| Stage | Full Name | What happens |
|---|---|---|
| **MC** | Modelling Coordinates | Object's own local coordinate space |
| **Modeling Transform** | — | Place objects in the scene (translate/rotate/scale) |
| **WC** | World Coordinates | All objects placed in common scene space |
| **Viewing Transform** | — | Camera is positioned and oriented |
| **VC** | Viewing Coordinates | Scene from camera's perspective |
| **Projection** | — | 3D → 2D (parallel or perspective) |
| **NC** | Normalized Coordinates | Scaled to standard view volume + clipping applied |
| **DC** | Device Coordinates | Final screen pixel positions |

**Simple analogy:** MC = actor's local position → WC = position on movie set → Camera frames the shot → Film frame → TV screen pixel

**3-mark answer:** Draw the pipeline diagram + briefly explain each coordinate system (MC → WC → VC → NC → DC).

---

## TOPIC 5 — Depth Buffer (Z-Buffer) Algorithm
**[6 marks | Appears in ALL papers]**

### What problem does it solve?
When multiple 3D objects overlap, we must determine which one is **closest to the viewer** and should be visible.

### The Algorithm:

Uses two equal-sized buffers (same as screen resolution):
1. **Frame buffer** — stores the **colour** of each pixel
2. **Depth buffer (Z-buffer)** — stores the **depth (z value)** of the object currently at each pixel

**Steps:**
1. Initialize depth buffer to **∞** (maximum depth = very far)
2. Initialize frame buffer to **background colour**
3. For each polygon, for each pixel (x,y) it covers:
   - Calculate depth **z** at that (x,y)
   - If **z < depth_buffer[x][y]** (new object is CLOSER):
     - Update frame_buffer[x][y] with this polygon's colour
     - Update depth_buffer[x][y] = z
4. After all polygons are processed → frame buffer holds the final image

### Example:

Two triangles overlap at pixel (5,5):
- Triangle A: colour = red, depth z = **3** (closer)
- Triangle B: colour = blue, depth z = **7** (farther)

```
Start:  depth[5][5] = ∞,    frame[5][5] = background
After B: z=7 < ∞  → depth[5][5]=7,  frame[5][5]=blue
After A: z=3 < 7  → depth[5][5]=3,  frame[5][5]=red
Final result: pixel (5,5) shows RED ✓
```

### Advantages:
1. Simple to implement
2. Works for any shape (no sorting needed)
3. Handles any level of complexity

### Disadvantage:
1. Requires large memory (one depth value per pixel)

### For 6-mark answer:
Algorithm steps: 4 marks | Example: 2 marks

---

## TOPIC 6 — Sutherland-Hodgeman Polygon Clipping
**[8 marks | Appeared in ALL papers]**

### Idea (Simple explanation):
Clip the polygon against **one boundary at a time** — left, right, bottom, top — passing the result of each clip to the next boundary.

### 4 Cases for Each Polygon Edge:

For each edge of the polygon, check the two endpoints (P_in = previous vertex, P_current = current vertex):

| Case | P_in | P_current | What to output |
|---|---|---|---|
| 1 | Inside | Inside | Output P_current |
| 2 | Inside | Outside | Output intersection point only |
| 3 | Outside | Outside | Output nothing |
| 4 | Outside | Inside | Output intersection point + P_current |

### Steps:
1. Start with all polygon vertices
2. Clip against **LEFT** boundary → new vertex list
3. Clip new list against **RIGHT** boundary → new vertex list
4. Clip against **BOTTOM** boundary → new vertex list
5. Clip against **TOP** boundary → final clipped polygon

### For 8-mark answer:
Show the polygon being clipped against each boundary with the vertex list after each step.
2 marks per boundary × 4 boundaries = 8 marks.

---

## EXAM STRATEGY — Module III

| Question | Content | Recommended |
|---|---|---|
| **Q15** | Cohen-Sutherland numerical + Types of Projections | **Best choice — choose Q15** |
| **Q16** | Sutherland-Hodgeman polygon clipping + Depth buffer | Also good |

**Best choice: Q15** — Region codes are straightforward, and the taxonomy diagram alone = 4 marks.

---

## QUICK REVISION SUMMARY

| Topic | What to remember |
|---|---|
| Window to Viewport | xv = Sx·xw + tx, Sx = (vmax-vmin)/(wmax-wmin) |
| Cohen-Sutherland | 9 regions, 4-bit codes (LRBA), OR=accept, AND=reject |
| Types of Projections | Draw taxonomy tree — parallel (ortho, oblique) vs perspective (1,2,3 pt) |
| 3D Viewing Pipeline | MC→WC→VC→NC→DC |
| Z-Buffer | Init depth=∞, update if new z < stored z |
| Sutherland-Hodgeman | Clip one boundary at a time, 4 cases per edge |
