# MODULE II — 2D Transformations & Polygon Filling
## CST304 Computer Graphics | Study Guide

> **Exam weight:** 3 marks (Part A Q3 or Q4) + 14 marks (Part B Q13 or Q14)
> **Appeared in:** ALL 4 question papers
>
> 📂 **Your PDF for this module:** `Module 2/M2-CST304-Ktunotes.in.pdf`

---

## TOPIC 1: Boundary Fill vs Flood Fill (3 marks — Part A)
**[Appears in every paper as a Part A question]**

### Simple explanation:
Both are ways to **colour the inside of a shape** on screen.

| Point | Flood Fill | Boundary Fill |
|---|---|---|
| How area is defined | By a specific **interior colour** | By a specific **boundary colour** |
| What gets coloured | Replaces one colour with new colour | Colours everything that is NOT the boundary |
| Use case | When interior has single colour | When boundary is clearly defined |
| Complexity | May use unpredictable memory | More controlled |
| Speed | Time consuming | Less time consuming |

**3-mark answer:** Write any 3 rows from the table above. 1 mark per difference.

---

## TOPIC 2: Homogeneous Coordinates (3 marks — Part A)

### What are they? (Simple explanation)
Normally a 2D point is (x, y). In **homogeneous coordinates**, we add a third value 'w' and write it as (x, y, w). For real points, we usually set w = 1, so the point (x, y) becomes **(x, y, 1)**.

**Why do we need them?**
- Translation cannot be done with 2×2 matrices alone
- With homogeneous coordinates, ALL transformations (translate, rotate, scale) can be represented as **3×3 matrix multiplication**
- This allows us to **combine multiple transformations into one matrix** by multiplying matrices together

**3-mark answer:**
- Definition (1.5 marks): A point (x,y) is represented as (x, y, w) where w is typically 1. Actual coordinates are x/w and y/w.
- Why needed (1.5 marks): Allows translation to be expressed as matrix multiplication, enabling all 2D transformations to be combined using matrix multiplication.

---

## TOPIC 3: 2D Transformations using Homogeneous Coordinates
> 📖 **PDF Reference:** `Module 2/M2-CST304-Ktunotes.in.pdf` → Section: **"2D Transformations"** or **"Homogeneous Coordinates"**. Look for the 3×3 matrix diagrams for translation, rotation, and scaling.
**(8 marks — most important Part B topic)**
**[Appears in ALL 3 papers — MUST know this]**

### The 3 Basic Transformation Matrices:

#### 1. Translation by (tx, ty):
```
T = | 1  0  tx |
    | 0  1  ty |
    | 0  0  1  |

New point: [x'] = [x + tx]
           [y'] = [y + ty]
           [1 ]   [  1   ]
```

#### 2. Scaling by (Sx, Sy):
```
S = | Sx  0   0 |
    | 0   Sy  0 |
    | 0   0   1 |

New point: x' = Sx * x,  y' = Sy * y
```

#### 3. Rotation by angle θ (about origin):
```
R = | cos θ  -sin θ  0 |
    | sin θ   cos θ  0 |
    | 0       0      1 |

New point: x' = x·cosθ - y·sinθ
           y' = x·sinθ + y·cosθ
```
For 90°: cos90=0, sin90=1
```
R(90°) = | 0  -1  0 |
         | 1   0  0 |
         | 0   0  1 |
```
For 45°: cos45=sin45=1/√2 ≈ 0.707

---

### ROTATION ABOUT AN ARBITRARY POINT (most common 8-mark question!)

**The trick:** You can only rotate about the **origin** directly. If you want to rotate about any other point (a, b), do these 3 steps:

1. **Translate** so that (a,b) moves to origin → T(-a, -b)
2. **Rotate** by θ → R(θ)
3. **Translate back** → T(a, b)

**Combined matrix:** M = T(a,b) · R(θ) · T(-a,-b)

---

### WORKED EXAMPLE (From May 2023 — 8 marks)
**Square vertices: (2,6), (6,6), (6,2), (2,2)**
**(i) Rotate 45° about vertex (2,6)**
**(ii) Scale by factor 2 about its centre**

#### Part (i): Rotate 45° about (2,6)

**Step 1:** Translate so (2,6) goes to origin → tx=-2, ty=-6
```
T(-2,-6) = | 1  0  -2 |
           | 0  1  -6 |
           | 0  0   1 |
```

**Step 2:** Rotate 45° (cos45=sin45=0.707)
```
R(45°) = | 0.707  -0.707  0 |
         | 0.707   0.707  0 |
         | 0       0      1 |
```

**Step 3:** Translate back → tx=2, ty=6
```
T(2,6) = | 1  0  2 |
         | 0  1  6 |
         | 0  0  1 |
```

**Combined: M = T(2,6) · R(45°) · T(-2,-6)**

Apply to each vertex. For example, vertex (6,6):
- After T(-2,-6): (6-2, 6-6) = (4, 0)
- After R(45°): x' = 4·0.707 - 0·0.707 = 2.83, y' = 4·0.707 + 0·0.707 = 2.83
- After T(2,6): (2.83+2, 2.83+6) = **(4.83, 8.83)**

Do same for all 4 vertices.

#### Part (ii): Scale by 2 about centre (4,4)
Centre of square = ((2+6)/2, (2+6)/2) = **(4,4)**

**Step 1:** Translate so (4,4) goes to origin → tx=-4, ty=-4
**Step 2:** Scale by Sx=Sy=2
**Step 3:** Translate back → tx=4, ty=4

For vertex (2,6):
- After T(-4,-4): (2-4, 6-4) = (-2, 2)
- After Scale(2,2): (-4, 4)
- After T(4,4): (-4+4, 4+4) = **(0, 8)**

For vertex (6,6): → (8, 8)
For vertex (6,2): → (8, 0)
For vertex (2,2): → (0, 0)

**New vertices: (0,8), (8,8), (8,0), (0,0)** ✓ (doubled in size)

### Mark breakdown (8 marks):
- Identifying correct 3-step approach: 2 marks
- Writing translation/rotation/scaling matrices: 2 marks
- Calculating new coordinates: 4 marks

---

### SCALING ABOUT ARBITRARY POINT (same 3-step approach):

1. Translate point to origin: T(-px, -py)
2. Scale: S(Sx, Sy)
3. Translate back: T(px, py)

---

## TOPIC 4: Proving 2D Rotation is Additive (6 marks)

**The claim:** Two successive rotations R(θ1) then R(θ2) = R(θ1 + θ2)

**Proof:**
```
R(θ1) = | cos θ1  -sin θ1  0 |     R(θ2) = | cos θ2  -sin θ2  0 |
        | sin θ1   cos θ1  0 |             | sin θ2   cos θ2  0 |
        | 0        0       1 |             | 0        0       1 |
```

Multiply R(θ1) × R(θ2):
- Top-left: cos θ1·cos θ2 - sin θ1·sin θ2 = **cos(θ1+θ2)** ✓ (trig identity)
- Top-right: -cos θ1·sin θ2 - sin θ1·cos θ2 = **-sin(θ1+θ2)** ✓
- Bottom-left: sin θ1·cos θ2 + cos θ1·sin θ2 = **sin(θ1+θ2)** ✓

Result = R(θ1+θ2) → **Rotations are ADDITIVE** ✓

**For scaling:** S(sx1,sy1) × S(sx2,sy2) gives S(sx1·sx2, sy1·sy2) → **Scaling is MULTIPLICATIVE** ✓

---

## TOPIC 5: Scan Line Polygon Filling (8 marks)
> 📖 **PDF Reference:** `Module 2/M2-CST304-Ktunotes.in.pdf` → Section: **"Scan Line Fill Algorithm"** or **"Polygon Filling"**. Look for the diagram showing horizontal scan lines cutting through a polygon, and the Edge Table structure.

### The Idea (simple):
Imagine horizontal lines sweeping from top to bottom of a polygon. At each scan line (horizontal row of pixels), find where it intersects the polygon edges, then fill in the pixels between those intersection points.

### Algorithm Steps:
1. Find the **minimum and maximum Y values** of the polygon
2. For each scan line y (from y_min to y_max):
   a. Find all **intersections** of the scan line with polygon edges
   b. **Sort** intersections by x value
   c. **Fill pixels** between pairs of intersections (1st to 2nd, 3rd to 4th, etc.)

### Data Structures Used:
**1. Edge Table (ET):**
- Stores all edges of the polygon
- For each edge: y_max, x at y_min, 1/slope (Δx)
- Edges sorted by y_min

**2. Active Edge Table (AET):**
- Contains only edges currently being crossed by the scan line
- Updated at each new y value
- Sorted by x intersection value

### Important Rules:
- Horizontal edges are ignored (not added to ET)
- At a vertex, if both edges go same direction (both up or both down) → count as 1 intersection
- If edges go different directions (one up, one down) → count as 2 intersections

### For 8-mark answer:
- Algorithm explanation: 5 marks
- Data structures (ET and AET with diagram): 3 marks

---

## PART A QUICK ANSWERS FOR MODULE II

### Q: What are homogeneous coordinates and why are they necessary? (3 marks)
A point (x,y) in 2D is represented as (x, y, 1) in homogeneous coordinates by adding a third coordinate w=1. **Why needed:** Translation cannot be represented as matrix multiplication in 2D. Adding the extra coordinate allows ALL geometric transformations (translate, rotate, scale, shear) to be represented as 3×3 matrices and combined by matrix multiplication.

### Q: Differentiate boundary filling and flood filling (3 marks)
[See table in Topic 1 above — write any 3 rows]

---

## EXAM STRATEGY FOR MODULE II

**Q13 (2D transformation numerical + 3D transforms OR Flood-fill comparison + scan-line data structures):** Most scoring choice
**Q14 (Scan line fill + prove additive/multiplicative OR 2D transformation numerical):**

**Recommended: Whichever has the 2D transformation numerical** — 3-step method is formulaic and safe.

---

## NEW TOPICS ADDED (from 2023 papers)

### Reflection Transformations (6 marks — appeared in June 2023)
> 📖 **PDF Reference:** `Module 2/M2-CST304-Ktunotes.in.pdf` → Section: **"Reflections"** or **"2D Transformations — Reflection"**. Look for the diagrams showing reflection about x-axis, y-axis, and the line y=x.

**Reflection about line y = x:**
- Swap the x and y coordinates of every point
- Matrix:
```
| 0  1  0 |
| 1  0  0 |
| 0  0  1 |
```
- Point (x, y) → (y, x)
- Example: Triangle A(20,10) → A'(10,20), B(80,20) → B'(20,80), C(50,70) → C'(70,50)

**Reflection about line y = -x:**
- Negate and swap coordinates
- Matrix:
```
|  0  -1  0 |
| -1   0  0 |
|  0   0  1 |
```
- Point (x, y) → (-y, -x)
- Example: Triangle A(20,10) → A'(-10,-20)

### Inside/Outside Point Techniques (4 marks — June 2023)
Two methods to check if a point is inside a polygon:

**1. Odd-Even Rule (Ray Casting):**
- Draw a ray from the point to infinity in any direction
- Count how many polygon edges it crosses
- **Odd count** = inside, **Even count** = outside

**2. Non-Zero Winding Number Rule:**
- Draw a ray from the point in any direction
- Count edge crossings: left-to-right = +1, right-to-left = -1
- **Sum ≠ 0** = inside, **Sum = 0** = outside
