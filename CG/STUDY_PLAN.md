# CST304 — Computer Graphics & Image Processing
## Exam Preparation Plan | Target: 60+ / 100
### Based on thorough analysis of ALL 4 question papers

---

## EXAM PATTERN (CST304 — Your Current Scheme)

| Section | Details | Marks |
|---|---|---|
| **Part A** | 10 questions × 3 marks — **Answer ALL, no choice** | 30 |
| **Part B** | 5 modules × 14 marks — **Choose 1 out of 2 per module** | 70 |
| **Total** | | **100** |

> **Note:** The 2019 papers (CS401) had a different pattern — 4 marks per Part A question, 4 parts instead of 5 modules. Topics still overlap but your exam follows the CST304 (2019 scheme) format shown in May 2023 and June 2023 papers.

**Pass mark = 50. To reach 50:**
- Score 20/30 in Part A + 30/70 in Part B (about 2-3 decent module answers)

**To reach 60+:**
- Score 22-24/30 in Part A + 36-40/70 in Part B (3 good module answers)

---

## SCORING STRATEGY — The Smart Path to 50+

### Part A (30 marks) — Your easiest route
These 3-mark questions are **repeated almost identically** across all papers. If you memorize the answers in the cheat sheet, you can score 20-25 marks here with minimal effort.

### Part B (70 marks) — Pick your battles
Each module has **two choices** (Q11 or Q12, Q13 or Q14, etc.). You answer ONE per module.

**Best modules to score in:**
1. **Module I** — Bresenham/DDA are step-by-step tables. Very scoring if you practice even once.
2. **Module V** — Histogram equalization is formulaic. Follow the 5-step table, get 8 marks.
3. **Module III** — Types of projections diagram = 4 marks alone. Cohen-Sutherland has clear steps.
4. **Module II** — 3-step transformation method. Once you know it, you can apply to any question.
5. **Module IV** — Fundamental steps diagram = 4 marks alone. Easy theory.

---

## PRIORITY TOPICS — Based on ALL 4 Question Papers

### ⭐⭐⭐⭐ APPEARED IN ALL 4 PAPERS — DO THESE FIRST

**Part A (guaranteed):**
- Applications of Computer Graphics (6 points)
- Raster scan display architecture (diagram + explanation)
- Window to Viewport transformation (equations)

**Part B (guaranteed):**
- Bresenham's Line Drawing Algorithm (derivation + numerical)
- DDA Line Drawing Algorithm (numerical — plot the line)
- 2D Transformations — rotate/scale about arbitrary point (numerical)
- Scan-line polygon filling + data structures (ET and AET)
- Prove 2D rotations are additive / 2D scalings are multiplicative
- Cohen-Sutherland Line Clipping (region codes + numerical)
- Types of Projections with taxonomy diagram
- Depth Buffer (Z-buffer) algorithm
- DIP Fundamental Steps / Components of image processing system (diagram)
- Pixel path calculations — 4-path, 8-path, m-path (numerical with grid)
- Histogram Equalization (numerical — 5-step table)
- Sobel & Prewitt Edge Detectors (masks + explanation)
- Laplacian filter (masks + use)

---

### ⭐⭐⭐ APPEARED IN 3 OUT OF 4 PAPERS — HIGH PRIORITY

- Bresenham's Circle Drawing Algorithm (numerical)
- Beam Penetration CRT / Shadow Mask method (appeared June 2023, May 2023, Dec 2019)
- Boundary fill algorithm (4-connected or 8-connected)
- Sutherland-Hodgeman Polygon Clipping
- Intensity Transformations — Image Negatives, Power Law, Log
- Region Splitting and Merging (segmentation)

---

### ⭐⭐ APPEARED IN 2 PAPERS — PREPARE IF TIME ALLOWS

- Midpoint Circle Drawing Algorithm
- Reflection transformations (about y=x, y=-x) — in 2023 papers
- 3D Transformations (translation/scaling matrices) — appeared in Part A of 2023 papers
- Oblique Projections (detail) — appeared in June 2023 Part A and May 2019 Part B
- Spatial vs Grey Level Resolution
- Convolution (definition, properties, steps)
- Intensity thresholding / Global thresholding algorithm

---

### ⭐ APPEARED IN 1 PAPER — DO ONLY IF YOU HAVE TIME

- DVST (Direct View Storage Tube) — May 2019 only
- Weiler-Atherton Polygon Clipping — 2019 papers only (not in 2023 scheme)
- Depth sorting method — May 2019 only
- Back face detection — May 2019 only
- Illumination & Reflectance — June 2023 only

---

## PART A — COMPLETE QUESTION BANK (All papers combined)

Study ALL of these — they repeat. Your cheat sheet covers them all.

| Topic | Source Papers | File |
|---|---|---|
| Applications of CG | ALL 4 | PART_A_Cheatsheet.md Q1 |
| Raster scan architecture | May 2023, (May 2019 Part B) | PART_A_Cheatsheet.md Q2 |
| Boundary fill vs Flood fill | May 2023 | PART_A_Cheatsheet.md Q3 |
| Homogeneous coordinates | May 2023, May 2019 | PART_A_Cheatsheet.md Q4 |
| Window to Viewport | May 2023, June 2023, Dec 2019 | PART_A_Cheatsheet.md Q5 |
| 3D Viewing pipeline | May 2023 | PART_A_Cheatsheet.md Q6 |
| Connected component | May 2023 | PART_A_Cheatsheet.md Q7 |
| Sampling & Quantization | May 2023 | PART_A_Cheatsheet.md Q8 |
| Laplacian filter | May 2023 | PART_A_Cheatsheet.md Q9 |
| Intensity thresholding | May 2023 | PART_A_Cheatsheet.md Q10 |
| Aspect ratio vs Resolution | June 2023 | PART_A_Cheatsheet.md Q11 |
| 4-neighbour Flood-fill algorithm | June 2023 | PART_A_Cheatsheet.md Q12 |
| Basic 3D Transformations | June 2023, May 2019 | PART_A_Cheatsheet.md Q13 |
| Oblique Projections | June 2023 | PART_A_Cheatsheet.md Q14 |
| Pixel terms (neighbours, boundary, path) | June 2023, May 2019 | PART_A_Cheatsheet.md Q15 |
| Grayscale vs Colour image | June 2023 | PART_A_Cheatsheet.md Q16 |
| Global thresholding algorithm | June 2023 | PART_A_Cheatsheet.md Q17 |
| Smoothing vs Sharpening | June 2023 | PART_A_Cheatsheet.md Q18 |

---

## MODULE-WISE STUDY GUIDES

| Module | Study File | ⭐ Most Important Part B Question |
|---|---|---|
| I | [Module_I_Guide.md](Module_I_Guide.md) | Bresenham Line derivation + DDA numerical |
| II | [Module_II_Guide.md](Module_II_Guide.md) | 2D Transformation numerical (rotate + scale about arbitrary point) |
| III | [Module_III_Guide.md](Module_III_Guide.md) | Cohen-Sutherland + Types of Projections taxonomy |
| IV | [Module_IV_Guide.md](Module_IV_Guide.md) | Fundamental steps diagram + Pixel path numerical |
| V | [Module_V_Guide.md](Module_V_Guide.md) | Histogram Equalization numerical |

---

## HOW STUDY SESSIONS WORK

When you study a topic with me:
1. I will explain it simply (like you're 15 — no assumed knowledge)
2. I will draw a simple ASCII diagram to help you understand
3. I will tell you **which PDF and which section** to open for the real figure
4. We will work through numerical examples step by step
5. I will give you the exact answer format to write in the exam

**Diagram note:** During our chat, I create simple text diagrams for understanding. For the actual detailed figure, I'll always tell you: *"Open [Module X PDF] → look for section [name]"* so you can study the original image too.

---

## QUICK EXAM-DAY CHECKLIST

- [ ] Part A: Can write answers for all 18 topics in cheat sheet
- [ ] Module I: Can do Bresenham table from scratch
- [ ] Module II: Know the 3-step method for transformations about arbitrary point
- [ ] Module III: Know the 9 region codes for Cohen-Sutherland + taxonomy diagram
- [ ] Module IV: Can draw the DIP fundamental steps block diagram
- [ ] Module V: Can do histogram equalization table step by step

*If you have all 6 checked → you WILL pass. If you have 4+ checked → you will likely pass.*
