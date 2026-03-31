# Gene Alignment Visualizer

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen?style=flat-square)](https://nullvoid42.github.io/gene-alignment-visualizer)
[![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](LICENSE)
[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![No Dependencies](https://img.shields.io/badge/dependencies-none-success?style=flat-square)](package.json)
[![Live Demo](https://img.shields.io/badge/demo-live-blue?style=flat-square)](https://nullvoid42.github.io/gene-alignment-visualizer)

An interactive, browser-based visualization tool for classic biological sequence alignment algorithms. Designed for students, educators, and researchers who want to explore and understand the mechanics of **Needleman-Wunsch** (global alignment) and **Smith-Waterman** (local alignment) through animated, step-by-step DP matrix construction and traceback.

> **Live Demo:** [https://nullvoid42.github.io/gene-alignment-visualizer](https://nullvoid42.github.io/gene-alignment-visualizer)

---

## Table of Contents

- [Features](#features)
- [Algorithms](#algorithms)
- [Scoring Scheme](#scoring-scheme)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Tech Stack](#tech-stack)
- [Contributing](#contributing)
- [License](#license)

> 📖 中文说明：[README_zh.md](README_zh.md)

---

## Features

- **Dual algorithm support** — switch between Needleman-Wunsch (global) and Smith-Waterman (local) alignment
- **Multi-sequence type** — accepts DNA, RNA, and protein sequences
- **DP matrix visualization** — renders the full dynamic programming scoring matrix with per-cell values and directional arrows
- **Animated step-by-step walkthrough** — watch the matrix fill cell-by-cell with adjustable playback speed
- **Traceback path highlighting** — optimal alignment path rendered directly on the matrix
- **Aligned sequence display** — final aligned sequences with match/mismatch/gap annotations
- **Zero dependencies** — runs entirely in the browser; no build step, no npm, no framework
- **Dark theme UI** — GitHub-inspired dark color scheme optimized for extended use

---

## Algorithms

### Needleman-Wunsch (Global Alignment)

Introduced by Saul Needleman and Christian Wunsch in 1970, this algorithm finds the **optimal global alignment** between two sequences by filling an (m+1) × (n+1) DP matrix using the recurrence:

```
F(i, j) = max(
    F(i-1, j-1) + s(a_i, b_j),   // match / mismatch
    F(i-1, j)   + gap,            // deletion
    F(i,   j-1) + gap             // insertion
)
```

Initialization: `F(i, 0) = i × gap`, `F(0, j) = j × gap`

Best suited for sequences of **similar length** and **biological origin** (e.g., homologous protein families).

---

### Smith-Waterman (Local Alignment)

Introduced by Temple Smith and Michael Waterman in 1981, this algorithm finds the **highest-scoring local subsequence alignment** by modifying the recurrence to prevent negative scores:

```
H(i, j) = max(
    0,
    H(i-1, j-1) + s(a_i, b_j),   // match / mismatch
    H(i-1, j)   + gap,            // deletion
    H(i,   j-1) + gap             // insertion
)
```

Traceback begins from the **maximum-value cell** and terminates at the first `0`. Ideal for identifying **conserved domains** within longer, divergent sequences.

---

## Scoring Scheme

| Event     | Score |
|-----------|------:|
| Match     |   +2  |
| Mismatch  |   −1  |
| Gap       |   −2  |

These parameters follow a commonly-used educational default. The affine gap model (gap-open + gap-extend) is not implemented in the current version.

---

## Getting Started

No installation required. Clone or download the repository and open `index.html` directly in any modern browser.

```bash
git clone https://github.com/nullvoid42/gene-alignment-visualizer.git
cd gene-alignment-visualizer
open index.html        # macOS
# or
xdg-open index.html    # Linux
# or simply drag index.html into your browser
```

For the live hosted version, visit:
[https://nullvoid42.github.io/gene-alignment-visualizer](https://nullvoid42.github.io/gene-alignment-visualizer)

---

## Usage

1. **Select algorithm** — click the **Needleman-Wunsch** or **Smith-Waterman** tab at the top
2. **Enter sequences** — type or paste your sequences into the *Sequence A* and *Sequence B* input fields
3. **Select sequence type** — choose DNA, RNA, or Protein from the dropdown
4. **Run** — click the **Run** button to start the animation
5. **Adjust speed** — use the speed slider to control animation playback rate
6. **Inspect** — hover over individual matrix cells to inspect scores and directional pointers
7. **Reset** — click **Reset** to clear the matrix and start over

**Example inputs:**

```
DNA:     ACGTACGT  /  ACGTTGCA
RNA:     AUGCUAUG  /  AUGCUAUC
Protein: HEAGAWGHEE  /  PAWHEAE
```

---

## Tech Stack

| Layer      | Technology |
|------------|------------|
| Markup     | HTML5      |
| Styling    | CSS3 (custom properties, CSS Grid, Flexbox) |
| Logic      | Vanilla JavaScript (ES6+) |
| Rendering  | DOM manipulation |
| Hosting    | GitHub Pages |

No build tools, bundlers, or package managers are required.

---

## Contributing

Contributions are welcome. To propose a change:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/affine-gap`
3. Commit your changes with a clear message
4. Open a pull request describing the motivation and implementation

Potential areas for contribution:
- Affine gap penalty model
- Multiple sequence alignment (MSA)
- FASTA file import
- Exportable alignment result (SVG / PNG)
- Accessibility improvements (keyboard navigation, screen reader support)

Please open an issue first for significant changes to discuss the approach before investing effort.

---

## License

This project is released under the [MIT License](LICENSE).

---

## References

1. Needleman, S. B., & Wunsch, C. D. (1970). A general method applicable to the search for similarities in the amino acid sequence of two proteins. *Journal of Molecular Biology*, 48(3), 443–453.
2. Smith, T. F., & Waterman, M. S. (1981). Identification of common molecular subsequences. *Journal of Molecular Biology*, 147(1), 195–197.
3. Durbin, R., Eddy, S. R., Krogh, A., & Mitchison, G. (1998). *Biological Sequence Analysis*. Cambridge University Press.
