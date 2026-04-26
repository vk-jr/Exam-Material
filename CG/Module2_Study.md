# MODULE 2 — 2D Transformations & Polygon Filling
## CST304 Computer Graphics | All Topics + Exam Answers

---

## TOPIC 1 — Boundary Fill vs Flood Fill
**[3 marks | Part A | Appears in every paper]**

Both are methods to **colour the inside of a shape** on screen.

| Point | Flood Fill | Boundary Fill |
|---|---|---|
| How area is defined | By a specific **interior colour** | By a specific **boundary colour** |
| What gets coloured | Replaces one colour with new colour | Colours everything that is NOT the boundary |
| Use case | When interior has single colour | When boundary is clearly defined |
| Memory usage | Unpredictable (can be large) | More controlled |
| Speed | Slower | Faster |

**Exam tip:** Write any 3 rows from the table = full 3 marks. One row = one difference = one mark.

---

## TOPIC 2 — Homogeneous Coordinates
**[3 marks | Part A | Appeared in May 2023, May 2019]**

### What are they?
Normally a 2D point is (x, y). In homogeneous coordinates, we add a third value **w** and write it as **(x, y, w)**. For real points, w = 1, so the point (x, y) becomes **(x, y, 1)**.

### Why do we need them?
- Translation **cannot** be done with 2×2 matrices alone
- Adding the extra coordinate allows ALL transformations (translate, rotate, scale) to be represented as **3×3 matrix multiplication**
- This allows us to **combine multiple transformations** into one matrix by multiplying matrices together

### 3-mark answer format:
> A point (x, y) is represented as (x, y, 1) in homogeneous coordinates by adding a third coordinate w = 1. This is needed because translation cannot be expressed as matrix multiplication in 2D. By using 3×3 matrices, all geometric transformations — translation, rotation, and scaling — can be combined through matrix multiplication into a single composite transformation.

---

## TOPIC 3 — 2D Transformation Matrices
**[8 marks | Most important Part B topic | Appears in ALL papers]**

### The 3 Basic Transformation Matrices (always write these first):

#### 1. Translation by (tx, ty):
```
T = | 1  0  tx |
    | 0  1  ty |
    | 0  0   1 |

Result: x' = x + tx,  y' = y + ty
```

#### 2. Scaling by (Sx, Sy):
```
S = | Sx   0   0 |
    |  0  Sy   0 |
    |  0   0   1 |

Result: x' = Sx × x,  y' = Sy × y
```

#### 3. Rotation by angle θ (about origin):
```
R = | cosθ  -sinθ  0 |
    | sinθ   cosθ  0 |
    |  0      0    1 |

Result: x' = x·cosθ - y·sinθ
        y' = x·sinθ + y·cosθ
```

**Common angle values:**
- 90°: cos90 = 0, sin90 = 1
- 45°: cos45 = sin45 = 0.707
- 30°: cos30 = 0.866, sin30 = 0.5
- 60°: cos60 = 0.5, sin60 = 0.866

---

## TOPIC 4 — Rotation / Scaling About an Arbitrary Point
**[Most common 8-mark question | Appears in ALL papers]**

### The 3-Step Method:

You can only rotate/scale directly about the **origin**. For any other point (a, b):

1. **Translate** so that (a, b) moves to origin → T(-a, -b)
2. **Rotate or Scale** about origin
3. **Translate back** → T(a, b)

**Combined matrix:** M = T(a,b) · R(θ) · T(-a,-b)

---

### Worked Example — From May 2023 (8 marks)
**Square vertices: (2,6), (6,6), (6,2), (2,2)**
**(i) Rotate 45° about vertex (2,6)**
**(ii) Scale by factor 2 about its centre**

---

#### Part (i): Rotate 45° about (2,6)

**Step 1:** Translate so (2,6) → origin
```
T(-2,-6) = | 1  0  -2 |
           | 0  1  -6 |
           | 0  0   1 |
```

**Step 2:** Rotate 45° (cos45 = sin45 = 0.707)
```
R(45°) = | 0.707  -0.707  0 |
         | 0.707   0.707  0 |
         |  0       0     1 |
```

**Step 3:** Translate back
```
T(2,6) = | 1  0  2 |
         | 0  1  6 |
         | 0  0  1 |
```

**Apply to vertex (6,6) as example:**
- After T(-2,-6): (6-2, 6-6) = (4, 0)
- After R(45°): x' = 4×0.707 - 0×0.707 = **2.83**, y' = 4×0.707 + 0×0.707 = **2.83**
- After T(2,6): (2.83+2, 2.83+6) = **(4.83, 8.83)**

**Apply to all 4 vertices similarly.**

---

#### Part (ii): Scale by 2 about centre (4,4)

Centre of square = ((2+6)/2, (2+6)/2) = **(4, 4)**

**Step 1:** Translate so (4,4) → origin → T(-4,-4)
**Step 2:** Scale by Sx=Sy=2
**Step 3:** Translate back → T(4,4)

**Apply to each vertex:**

| Vertex | After T(-4,-4) | After Scale(2,2) | After T(4,4) | Result |
|---|---|---|---|---|
| (2,6) | (-2, 2) | (-4, 4) | (0, 8) | **(0,8)** |
| (6,6) | (2, 2) | (4, 4) | (8, 8) | **(8,8)** |
| (6,2) | (2, -2) | (4, -4) | (8, 0) | **(8,0)** |
| (2,2) | (-2, -2) | (-4, -4) | (0, 0) | **(0,0)** |

**New vertices: (0,8), (8,8), (8,0), (0,0)** ✓ (square doubled in size)

### Mark Breakdown (8 marks):
- Identifying correct 3-step approach: 2 marks
- Writing correct matrices: 2 marks
- Calculating new coordinates for all vertices: 4 marks

---

## TOPIC 5 — Reflection Transformations
**[6 marks | Appeared in June 2023]**

### Reflection about line y = x:
- Swap the x and y coordinates of every point
- Matrix:
```
| 0  1  0 |
| 1  0  0 |
| 0  0  1 |
```
- Point (x, y) → (y, x)
- **Example:** A(20,10) → A'(10,20), B(80,20) → B'(20,80), C(50,70) → C'(70,50)

### Reflection about line y = -x:
- Negate and swap coordinates
- Matrix:
```
|  0  -1  0 |
| -1   0  0 |
|  0   0  1 |
```
- Point (x, y) → (-y, -x)
- **Example:** A(20,10) → A'(-10,-20)

### Reflection about x-axis:
- Matrix:
```
| 1   0  0 |
| 0  -1  0 |
| 0   0  1 |
```
- Point (x, y) → (x, -y)

### Reflection about y-axis:
- Matrix:
```
| -1  0  0 |
|  0  1  0 |
|  0  0  1 |
```
- Point (x, y) → (-x, y)

---

## TOPIC 6 — Proving 2D Rotations are Additive / Scalings are Multiplicative
**[6 marks | Appeared in ALL papers]**

### Proof: 2D Rotations are ADDITIVE

**Claim:** R(θ1) followed by R(θ2) = R(θ1 + θ2)

```
R(θ1) = | cosθ1  -sinθ1  0 |     R(θ2) = | cosθ2  -sinθ2  0 |
        | sinθ1   cosθ1  0 |             | sinθ2   cosθ2  0 |
        |  0       0     1 |             |  0       0     1 |
```

**Multiply R(θ1) × R(θ2), top-left element:**
> cosθ1·cosθ2 - sinθ1·sinθ2 = **cos(θ1+θ2)** ✓ (trig identity)

**Top-right element:**
> -cosθ1·sinθ2 - sinθ1·cosθ2 = **-sin(θ1+θ2)** ✓

**Bottom-left element:**
> sinθ1·cosθ2 + cosθ1·sinθ2 = **sin(θ1+θ2)** ✓

**Result = R(θ1+θ2) → Rotations are ADDITIVE ✓**

---

### Proof: 2D Scalings are MULTIPLICATIVE

**Claim:** S(sx1,sy1) followed by S(sx2,sy2) = S(sx1·sx2, sy1·sy2)

```
S(sx1,sy1) × S(sx2,sy2) = | sx1  0  0 |   | sx2  0  0 |   | sx1·sx2    0     0 |
                           |  0  sy1 0 | × |  0  sy2 0 | = |    0    sy1·sy2  0 |
                           |  0   0  1 |   |  0   0  1 |   |    0       0     1 |
```

**Result = S(sx1·sx2, sy1·sy2) → Scalings are MULTIPLICATIVE ✓**

---

## TOPIC 7 — Scan Line Polygon Filling
**[8 marks | Appeared in ALL papers]**

### The Idea (simple):
Horizontal lines sweep from top to bottom of a polygon. At each scan line (row), find where it intersects the edges, then fill pixels between those intersection points.

### Algorithm Steps:
1. Find minimum Y (y_min) and maximum Y (y_max) of polygon
2. For each scan line y from y_min to y_max:
   a. Find all **intersection points** of scan line with polygon edges
   b. **Sort** intersection points by x value
   c. **Fill pixels** between pairs: 1st↔2nd, 3rd↔4th, etc.

### Data Structures Used:

**1. Edge Table (ET):**
Stores all non-horizontal edges. For each edge, stores:
- **y_max** — upper y value of edge
- **x at y_min** — x coordinate where edge starts (at lower y)
- **1/slope (Δx)** — how much x changes as y increases by 1
- Edges sorted by y_min

**2. Active Edge Table (AET):**
- Contains only edges currently intersected by the scan line
- Updated at each new y value — edges added when scan line reaches y_min, removed when scan line passes y_max
- Always sorted by x intersection value

### Important Rules:
- **Horizontal edges** are ignored (not added to ET)
- At a vertex where both edges go **same direction** (both up or both down) → count as **1 intersection**
- At a vertex where edges go **different directions** (one up, one down) → count as **2 intersections**

### For 8-mark answer:
- Algorithm explanation + scan line diagram: 5 marks
- ET and AET data structures with explanation: 3 marks

---

## TOPIC 8 — Inside/Outside Test
**[4 marks | Appeared in June 2023]**

Two methods to check if a point is inside a polygon:

### 1. Odd-Even Rule (Ray Casting):
- Draw a ray from the test point in any direction (usually horizontal right)
- Count how many polygon edges it crosses
- **Odd count = INSIDE**, **Even count = OUTSIDE**

```
Example:
Point P → ────────────→ (crosses 3 edges = odd = INSIDE)
Point Q → ────────────→ (crosses 2 edges = even = OUTSIDE)
```

### 2. Non-Zero Winding Number Rule:
- Draw a ray from the test point in any direction
- Count edge crossings: left-to-right crossing = **+1**, right-to-left crossing = **-1**
- **Sum ≠ 0 = INSIDE**, **Sum = 0 = OUTSIDE**

---

## EXAM STRATEGY — Module II

| Question | Content | Recommended |
|---|---|---|
| **Q13** | 2D transformation numerical + 3D transforms | **Choose Q13 if transformation numerical appears** |
| **Q14** | Scan line fill + prove additive/multiplicative | Also manageable |

**Best approach:** Whichever question has the 2D transformation numerical — the 3-step method is formulaic and safe.

---

## QUICK REVISION SUMMARY

| Topic | What to remember |
|---|---|
| Flood fill vs Boundary fill | Flood fill = interior colour; Boundary fill = boundary colour |
| Homogeneous coordinates | (x,y) → (x,y,1) ; enables matrix multiplication for all transforms |
| Translation | Matrix has tx, ty in last column |
| Scaling | Matrix has Sx, Sy on diagonal |
| Rotation | Matrix has cosθ, sinθ — remember sign: top-right is -sinθ |
| Arbitrary point transform | 3 steps: Translate to origin → Transform → Translate back |
| Reflection y=x | Swap (x,y)→(y,x) |
| Reflection y=-x | Negate and swap (x,y)→(-y,-x) |
| Rotations additive | Use trig identity: cos(A+B), sin(A+B) |
| Scalings multiplicative | S1×S2 multiplies the scale factors |
| Scan line fill | ET → AET → fill between pairs of intersections |
