# CLD Studio

**An interactive causal modeling tool for building, visualizing, and simulating Causal Loop Diagrams (CLD).**

A standalone web app (a single HTML file), running entirely in the browser, no installation or server required.

---

## Table of contents

- [What the tool is](#what-the-tool-is)
- [How a CLD works](#how-a-cld-works)
- [What the tool can do](#what-the-tool-can-do)
- [The 3D model](#the-3d-model)
- [Where it has been used](#where-it-has-been-used)
- [How to use it](#how-to-use-it)
- [Data structure (Excel)](#data-structure-excel)
- [Online deployment](#online-deployment)
- [Repository structure](#repository-structure)
- [License](#license)

---

## What the tool is

CLD Studio lets you build Causal Loop Diagrams and, above all, make them **computable**: rather than remaining a simple map of arrows, every variable in the graph can be linked to real data and recalculated automatically.

Its distinguishing feature compared to classic diagramming tools (drawio, Miro, Kumu), which stay purely qualitative, is the ability to replace an arrow with a calculation or a formula wired to a data table, without resorting to the heavier formalism of system dynamics tools (Vensim, Stella). The tool is still under active development (*work in progress*).

The tool is generic: no business logic is hard-coded. The dataset used to illustrate it (timber construction, insulation) is just one example application among many — the tool can model any system with feedback effects (public health, energy, agriculture, circular economy, logistics…).

## How a CLD works

A **causal link** connects two variables A → B and carries a **polarity**: it answers the question "if A increases, all else being equal, what happens to B?"

- **Positive link (+)**: A and B move in the same direction.
- **Negative link (−)**: A and B move in opposite directions.

This is not an observed correlation but a hypothesis of influence posed by the modeler, to be tested rather than taken for granted. A link can also carry a **delay**: the effect on the target variable only appears after some time.

A **loop** is a closed path of causal links that connects a variable back to itself. It is the loop, not an isolated variable, that explains the system's behavior over time. Two types of loops, automatically classified according to the parity of the number of negative links they contain:

- **Reinforcing loop (R)** — an **even** number of negative links (including zero). A change amplifies itself with each pass: growth, runaway, a vicious or virtuous circle.
- **Balancing loop (B)** — an **odd** number of negative links. A change counteracts itself with each pass, toward a target or a constraint: stabilization, scarcity, saturation.

## What the tool can do

**Building the model**
- Create variables (name, unit, tag/category, strength, fixed or calculated value) and causal links (sign, strength, delay), directly from the interface.
- Import and export the entire model via a structured Excel file (Variables, Links, Data, Functions, Project sheets).
- Follow an Excel file live: any change on disk is automatically reloaded, useful in a collaborative workshop.

**Detecting and exploring loops**
- Automatic detection of every closed loop in the graph, classified as R or B.
- Bookmark loops of interest as favorites, with a dedicated filter to isolate them visually.
- Explore a loop's neighborhood through successive **proximity levels** (1, 2, 3…), to understand its context without being overwhelmed by the whole graph.
- Automatic graph layout (by centrality, by readability, by categories/tags).

**Calculating from data**
- Link a variable to an external data table (materials catalog, scenarios…): its value is then computed by looking up a selected row in that table.
- An **iteration** engine recalculates the whole model step by step, converting link delays into calculation steps, to observe a trajectory over time rather than a simple qualitative direction.
- Switching hypotheses with one click (selecting a different row in the data table) instantly recalculates every linked variable.
- An iterations table and a calculation error log (missing reference, non-converging loop…).
- A **Summary** view of all variables, sorted by the origin of their value (hypothesis, formula, data) and by strength.

**Other features**
- Search for a variable by name, with automatic view centering.
- Export the graph as a PDF and the full project as a new Excel file.
- Built-in interactive guide (step-by-step tutorial, replayable at any time), interface available in French, English, Spanish, and German.

## The 3D model

A dedicated module lets you visualize the **cross-section of a wall** (here, a timber-frame panel) directly from the CLD model:

- Choice of wall composition: exterior cladding, interior cladding, insulation — each choice can be linked to a row in one of the model's data tables.
- Adjustable spacing between layers for a clearer reading of the cross-section.
- The **Calculate** button reruns the CLD's global calculation (as the iterations module would) and updates both the 3D view and a results panel (key variables: thermal resistance, cost, etc.), keeping it consistent with the rest of the model.
- Interactive view: zoom and rotate with the mouse to inspect the cross-section from every angle.

This module makes it possible to move from an abstraction (variables and links) to a concrete, directly interpretable representation of the physical object being modeled.

## Where it has been used

The tool was developed and tested on a **timber frame facade (TFF)** case study: modeling the full chain, from workshop manufacturing to on-site installation (material choices, transport, site logistics), with a real dataset of 123 variables and 251 links.

This case in particular was used to question the choice of insulation (bio-based straw versus conventional glass wool), going beyond a simple comparison of unit price or material-by-material carbon footprint, by highlighting the feedback loops of the overall system.

## How to use it

The tool (`index.html`) is a standalone web page, no installation or server required:

1. Open `index.html` in a browser.
2. Import a data workbook via the **File** menu, or build a model from scratch with **+ Create**.
3. Explore the graph: zoom, search for a variable, filter favorite loops, proximity levels.
4. Run a calculation (**Iterative** menu) to link variables to data and simulate their evolution.
5. Open **3D Model** to visualize the wall cross-section based on the chosen composition.

## Data structure (Excel)

The imported workbook structures the model into several sheets:

- **Variables**: list of variables (id, name, unit, tag, strength, value, formula…).
- **Links**: list of causal links (source, target, polarity, strength, delay).
- **Data — \***: reference tables (e.g., an insulation catalog), used by lookup-type formulas.
- **Functions**: formulas linking a variable to a column of a data table.
- **Project**: project metadata (name, favorite loops…).

## Online deployment

To make the tool accessible via a public URL (e.g., GitHub Pages):

1. Place `index.html` and the data workbook at the root of a public repository.
2. Enable GitHub Pages under **Settings → Pages** (branch `main`, folder `/root`).
3. The tool is then accessible at `https://<username>.github.io/<repo>/`.

## Repository structure

```
├── index.html   # CLD Studio (full tool)
├── *.xlsx       # Dataset(s) (variables, links, reference tables)
├── LICENSE      # CC BY-NC 4.0 license
└── README.md    # This document
```

## License

This project — the CLD Studio tool and the accompanying dataset — is published under the **Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)** license.

You are free to:
- **Share** — copy and redistribute the material, in any medium or format;
- **Adapt** — remix, transform, and build upon the material;

under the following terms:
- **Attribution** — give appropriate credit to the author (Benoit Mathis), indicate if changes were made, and provide a link to the license;
- **NonCommercial** — the material may not be used for commercial purposes.

See the [LICENSE](./LICENSE) file for the full text, or the [human-readable summary on creativecommons.org](https://creativecommons.org/licenses/by-nc/4.0/).

© 2026 Benoit Mathis
