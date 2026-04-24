# MODULE III — Clipping, Projections & 3D Viewing
## CST304 Computer Graphics | Study Guide

> **Exam weight:** 3 marks (Part A Q5 or Q6) + 14 marks (Part B Q15 or Q16)
> **Appeared in:** ALL 4 question papers
>
> 📂 **Your PDF for this module:** `Module 3/M3-CST304-Ktunotes.in.pdf`

---

## TOPIC 1: Window to Viewport Transformation (3 marks — Part A)
**[Appears in every paper]**

### Simple explanation:
- **Window** = the region of the world you want to see (in world coordinates)
- **Viewport** = the region on the screen where it gets displayed (in screen coordinates)

Think of it like: Window is what a camera sees, Viewport is the TV screen where it's shown.

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

**3-mark answer:** Write the two equations xv= and yv= and the formulas for Sx, Sy, tx, ty.

---

## TOPIC 2: Cohen-Sutherland Line Clipping (6-8 marks)
> 📖 **PDF Reference:** `Module 3/M3-CST304-Ktunotes.in.pdf` → Section: **"Cohen-Sutherland Line Clipping Algorithm"**. Look for the 9-region diagram with the 4-bit region codes. This diagram is essential — study it in the PDF after understanding my ASCII version.
**[Appears in ALL papers — includes a numerical]**

### What is clipping? (Simple explanation)
When you draw a line on screen, parts of it might go outside the visible window. Clipping removes those outside parts and keeps only what's visible. It's like using scissors to cut a drawing to fit inside a frame.

### Region Codes (The Core Idea):
Divide the screen into 9 regions. Each region gets a 4-bit code:

```
        |          |
1001    | 1000     | 1010
        |          |
--------|----------|--------   (Top boundary: Ymax)
        |          |
0001    | 0000     | 0010
        |  INSIDE  |
--------|----------|--------   (Bottom boundary: Ymin)
        |          |
0101    | 0100     | 0110
        |          |
```

Bit positions (from right to left): **Left | Right | Above | Below**
- Bit 1 (Left): x < x_min → 1, else 0
- Bit 2 (Right): x > x_max → 1, else 0
- Bit 3 (Below): y < y_min → 1, else 0
- Bit 4 (Above): y > y_max → 1, else 0

### Algorithm Steps:
1. **Assign region codes** to both endpoints P1 and P2
2. **Check:**
   - If code(P1) OR code(P2) = **0000** → both inside → **accept line** ✓
   - If code(P1) AND code(P2) ≠ **0000** → both outside same region → **reject line** ✗
   - Otherwise → **partially visible** → clip it
3. For clipping: find where the line intersects a boundary, replace the outside point with the intersection, repeat

### Intersection Formulas:
```
At left boundary (x = x_min):   y = y1 + m(x_min - x1)
At right boundary (x = x_max):  y = y1 + m(x_max - x1)
At bottom boundary (y = y_min): x = x1 + (x_min - y1)/m
At top boundary (y = y_max):    x = x1 + (x_max - y1)/m

where m = (y2-y1)/(x2-x1)
```

### WORKED EXAMPLE (From May 2023 — 6 marks)
**Clipping window: (0,0), (0,5), (8,5), (8,0)**
So: x_min=0, x_max=8, y_min=0, y_max=5

**Line: P1(-1,-2) to P2(9,7)**

**Step 1:** Assign codes
- P1(-1,-2): x<0 → left=1, y<0 → below=1 → Code = **0101**
- P2(9,7): x>8 → right=1, y>5 → above=1 → Code = **1010**

**Step 2:** Check:
- OR = 0101 | 1010 = **1111** ≠ 0 → not trivially accepted
- AND = 0101 & 1010 = **0000** = 0 → not rejected → partially visible, CLIP

**Step 3:** Find intersections
m = (7-(-2))/(9-(-1)) = 9/10 = 0.9

Start from P1 (code 0101, has Left and Below bits set):
- Clip at Bottom (y=0): x = x1 + (y_min - y1)/m = -1 + (0-(-2))/0.9 = -1 + 2.22 = **1.22**
  → New P1 = **(1.22, 0)**

Check new P1 code: x=1.22 (0<1.22<8), y=0 (y_min=0) → Code = **0000** ✓

From P2 (code 1010, has Right and Above bits set):
- Clip at Top (y=5): x = x1 + (y_max - y1)/m = -1 + (5-(-2))/0.9 = -1 + 7.78 = **6.78**
  → New P2 = **(6.78, 5)**

**Clipped line: (1.22, 0) to (6.78, 5)** ✓

### Mark breakdown (6 marks):
- Region codes for both endpoints: 1 mark
- Correct trivial accept/reject test: 2 marks
- Intersection calculation: 1 mark
- Final clipped line: 2 marks

---

## TOPIC 3: Types of Projections (8 marks)
> 📖 **PDF Reference:** `Module 3/M3-CST304-Ktunotes.in.pdf` → Section: **"Types of Projections"** or **"Classification of Projections"**. Look for the taxonomy tree diagram — drawing this exact diagram in the exam earns 3-4 marks alone. Also look for the individual figures showing Cavalier vs Cabinet projection and isometric projection.
**[Appears in ALL papers — diagram is KEY]**

### What is projection? (Simple explanation)
3D objects on a computer screen need to be converted to 2D images. Projection is how we do this — it's like making a shadow of a 3D object on a flat wall.

### TAXONOMY DIAGRAM (Draw this in exam — 4 marks just for correct diagram):

```
                    PROJECTIONS
                   /           \
         PARALLEL               PERSPECTIVE
        /        \              /    |    \
  Orthographic  Oblique       1-pt  2-pt  3-pt
   /    \        /  \        (vanishing points)
 Multi- Axono-  Cav- Cab-
 view   metric  alier inet
        /|\
  Iso- Di- Tri-
 metric metric metric
```

### Explanation of each type:

**PARALLEL PROJECTIONS** (projection lines are parallel):

1. **Orthographic (Multi-view):**
   - Lines of projection are perpendicular to view plane
   - Shows front, top, side views separately
   - Used in engineering drawings

2. **Orthographic (Axonometric):**
   - Isometric: Equal angles (120°) between axes — most common, looks nice
   - Dimetric: Two angles equal
   - Trimetric: All three angles different

3. **Oblique:**
   - Lines of projection at an angle to view plane
   - Cavalier: Length preserved along all directions
   - Cabinet: Depth foreshortened by half (more realistic)

**PERSPECTIVE PROJECTIONS** (lines converge at a point):
   - More realistic (like human eye)
   - One-point: One vanishing point (looking down a road)
   - Two-point: Two vanishing points (corner of a building)
   - Three-point: Three vanishing points (looking up at a skyscraper)

### For 8-mark answer:
- Taxonomy diagram: 3 marks
- Orthographic explanation: 1 mark
- Axonometric (isometric/dimetric/trimetric): 2 marks
- Oblique (cavalier/cabinet): 1 mark
- Perspective (1,2,3 point): 1 mark

---

## TOPIC 4: 3D Viewing Pipeline (3 marks — Part A)

### The Pipeline (draw this diagram):
```
MC → [Modeling Transform] → WC → [Viewing Transform] → VC
                                                         ↓
DC ← [Viewport Transform] ← NC ← [Normalization+Clipping] ← [Projection]
```

Where:
- **MC** = Modelling Coordinates (object's own space)
- **WC** = World Coordinates (common scene space)
- **VC** = Viewing Coordinates (camera space)
- **NC** = Normalized Coordinates
- **DC** = Device Coordinates (screen pixels)

**Simple analogy:** MC = actor's local position → WC = position on movie set → Camera view → Film frame → TV screen

---

## TOPIC 5: Depth Buffer Algorithm / Z-Buffer (6 marks)
> 📖 **PDF Reference:** `Module 3/M3-CST304-Ktunotes.in.pdf` → Section: **"Depth Buffer Method"** or **"Z-Buffer Algorithm"** or **"Visible Surface Detection"**. Look for the diagram showing the depth buffer and frame buffer side by side.
**[Appears in all papers]**

### What problem does it solve?
When multiple 3D objects overlap, which one do we draw in front? The one closer to the viewer should be visible.

### The Algorithm (Simple explanation):
Think of two tables:
1. **Frame buffer** — stores colour of each pixel
2. **Depth buffer (Z-buffer)** — stores how far away the object at each pixel is

**Steps:**
1. Initialize depth buffer to **maximum depth** (very far away) — usually ∞
2. Initialize frame buffer to **background colour**
3. For each polygon, for each pixel (x,y) covered:
   - Calculate depth z at that (x,y)
   - If z < depth_buffer[x][y] (object is CLOSER):
     - Update frame buffer with this object's colour
     - Update depth buffer with this z value
4. After all polygons processed, frame buffer has the final image

### Example:
```
Two triangles overlap at pixel (5,5):
- Triangle A at (5,5) has z = 3 (closer to viewer)
- Triangle B at (5,5) has z = 7 (farther from viewer)

Step 1: depth[5][5] = ∞, frame[5][5] = background
Step 2: Process Triangle B: z=7 < ∞ → update: depth[5][5]=7, frame[5][5]=blue
Step 3: Process Triangle A: z=3 < 7 → update: depth[5][5]=3, frame[5][5]=red
Final result: pixel (5,5) shows RED (Triangle A visible)
```

### For 6-mark answer:
- Algorithm steps: 4 marks
- Example: 2 marks

---

## TOPIC 6: Sutherland-Hodgeman Polygon Clipping (8 marks)
> 📖 **PDF Reference:** `Module 3/M3-CST304-Ktunotes.in.pdf` → Section: **"Sutherland-Hodgeman Algorithm"** or **"Polygon Clipping"**. Look for the step-by-step figure showing the polygon being clipped against each boundary — 4 stages.

### Idea (Simple explanation):
Instead of clipping all at once, clip the polygon against **one boundary at a time** — left, right, top, bottom. After each step, pass the result to the next boundary.

### Rules for each edge of polygon against each boundary:
There are 4 cases for each polygon edge (P_in = previous vertex, P_current = current vertex):

| Case | P_in | P_current | Output |
|---|---|---|---|
| 1 | Inside | Inside | P_current |
| 2 | Inside | Outside | Intersection point |
| 3 | Outside | Outside | Nothing |
| 4 | Outside | Inside | Intersection point + P_current |

### Steps:
1. Take all polygon vertices
2. Clip against LEFT boundary → get new vertex list
3. Clip new list against RIGHT boundary → get new vertex list
4. Clip against BOTTOM boundary → get new vertex list
5. Clip against TOP boundary → final clipped polygon

**For 8-mark answer:** Show the polygon being clipped against each boundary with the vertex list after each step. 2 marks per boundary (4 boundaries × 2 = 8 marks).

---

## PART A QUICK ANSWERS FOR MODULE III

### Q: Derive equations of Window to Viewport transformation (3 marks)
Write the equations: xv = Sx·xw + tx and yv = Sy·yw + ty with formulas for Sx, Sy, tx, ty as shown in Topic 1.

### Q: Explain the 3D viewing pipeline (3 marks)
Draw the block diagram and briefly explain each stage (MC → WC → VC → NC → DC).

---

## EXAM STRATEGY FOR MODULE III

**Q15 (Cohen-Sutherland numerical + Projections taxonomy):** Most students choose this
**Q16 (Sutherland-Hodgeman polygon clipping + Depth buffer):** Also manageable

**Recommended: Q15** — Region codes are straightforward, and taxonomy diagram alone gives 4 marks
