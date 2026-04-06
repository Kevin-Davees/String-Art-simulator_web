# String Art Simulator

A browser-based tool that converts any image into **realistic string art** using an iterative greedy algorithm.

The program analyzes an input image, identifies dark regions and edges, and progressively draws thousands of simulated threads between pins to recreate the image using **digital thread weaving**.

The entire application runs **fully in the browser using pure JavaScript and HTML5 Canvas** — no server or external libraries required.

---

# Features

- Convert images into **high-detail string art**
- Adjustable parameters for artistic control
- Edge-aware pin placement
- Greedy darkness subtraction algorithm
- Real-time generation progress
- Cancel generation anytime
- Download generated art as PNG
- Fully client-side (no uploads, privacy safe)

---

# Demo

Upload an image and the program will simulate thread weaving across pins to reproduce the image using line segments.

The generator works best with:

- portraits
- high contrast images
- objects with defined edges

---

# How It Works

The generator recreates an image by iteratively placing lines between pins.

The core idea is:

1. Convert the input image to grayscale
2. Enhance contrast using CLAHE
3. Detect edges
4. Place pins across the image
5. Iteratively add threads that reduce image darkness error

Each thread subtracts darkness from a residual image until the target image is approximated.

---

# Algorithm Pipeline

## 1. Image Preprocessing

The uploaded image is:

- resized to max **500px**
- converted to grayscale
- enhanced using **CLAHE (Contrast Limited Adaptive Histogram Equalization)**

Grayscale conversion formula:

```
Gray = 0.299R + 0.587G + 0.114B
```

CLAHE improves contrast so thread placement prioritizes details.

---

## 2. Edge Detection

The program calculates edges using a **Sobel gradient approximation**.

Edge strength is computed from horizontal and vertical gradients.

```
edge = sqrt(gx² + gy²)
```

Edge information helps guide pin placement.

---

## 3. Pin Generation

Pins represent anchor points where threads can connect.

Pins are created using:

- a regular grid
- additional pins biased toward edge regions

Edge-biased placement improves reconstruction of details.

Pin density is controlled by:

```
Pin density = grid subdivisions
```

Higher values produce more candidate pins.

---

## 4. Residual Darkness Map

A residual image is created:

```
residual = 1 - grayscale
```

This represents areas where darkness still needs to be reproduced.

Threads progressively reduce this residual.

---

## 5. Thread Selection (Greedy Optimization)

For each thread:

1. Randomly sample candidate pin pairs
2. Evaluate darkness along the line
3. Choose the pair that reduces the most residual error

The score is calculated by sampling points along the candidate line.

```
score = average residual along line
```

The line with the highest score is drawn.

---

## 6. Thread Rendering

Threads are drawn on the canvas with configurable parameters:

- opacity
- blur thickness
- minimum pin distance

Drawing is done using HTML5 canvas:

```
ctx.moveTo(x0, y0)
ctx.lineTo(x1, y1)
ctx.stroke()
```

---

## 7. Residual Update

After drawing a thread, darkness is subtracted along the line using **Bresenham's line algorithm**.

```
residual[pixel] -= thread_strength
```

This ensures future threads focus on remaining dark areas.

---

# Adjustable Parameters

### Number of Threads
Total number of simulated threads.

Higher values improve detail but increase runtime.

Typical range:

```
1000 – 20000
```

---

### Thread Strength

Controls opacity of each thread.

Higher values darken the image faster.

---

### Minimum Pin Gap

Minimum distance between connected pins.

Prevents short redundant lines.

---

### Thread Thickness

Adds blur to simulate real thread width.

---

### Pin Density

Controls number of candidate pins.

Higher density improves detail but increases computation.

---

### Edge Bias

Controls how strongly pins are placed near edges.

Useful for portraits and detailed objects.

---

# Performance

Runtime depends on:

- thread count
- pin density
- image resolution

Typical generation times:

| Threads | Time |
|-------|------|
| 1000 | 1–2 seconds |
| 5000 | 5–10 seconds |
| 20000 | 20–40 seconds |

Everything runs **in the browser** using JavaScript.

---

# File Structure

```
string-art-generator/
│
├── string_art.html
└── README.md
```

The entire application is contained in **one HTML file**.

---

# Running the Project

No installation required.

Simply open the file in a browser.

```
open string_art.html
```

Or double-click the file.

Works in:

- Chrome
- Firefox
- Edge
- Safari

---

# Exporting

Generated images can be downloaded using:

```
Download PNG
```

The output resolution matches the internal canvas size.

---

# Limitations

- Performance decreases with extremely high thread counts
- Works best with square or portrait images
- Highly detailed scenes may require very high thread counts

---

# Possible Improvements

Future enhancements could include:

- circular pin layouts
- GPU acceleration
- SVG thread export
- multi-color threads
- deterministic pin networks
- better optimization heuristics

---

# Technologies Used

- HTML5
- CSS
- JavaScript
- Canvas API

No external libraries.

---

# Author

Created as an experimental **string art simulation tool** demonstrating browser-based image optimization and procedural drawing.
