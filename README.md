# Analog (Integrated) Circuit Design

[![Quarto Publish](https://github.com/iic-jku/analog-circuit-design/actions/workflows/quarto-publish.yml/badge.svg?branch=main)](https://github.com/iic-jku/analog-circuit-design/actions/workflows/quarto-publish.yml)
[![Simulation Test](https://github.com/iic-jku/analog-circuit-design/actions/workflows/simulation-test.yml/badge.svg?branch=main)](https://github.com/iic-jku/analog-circuit-design/actions/workflows/simulation-test.yml)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.14387481.svg)](https://doi.org/10.5281/zenodo.14387481)
[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-online-brightgreen?logo=github)](https://iic-jku.github.io/analog-circuit-design/aicd.html)
[![License: Apache-2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)

**(c) 2024-2026 Harald Pretl and co-authors, Department for Integrated Circuits (ICD), Johannes Kepler University, Linz (JKU)**

Open course material for an intermediate-level MOSFET circuit design course, held at JKU under course number 336.009 ("KV Analoge Schaltungstechnik"). Everything — text, schematics, simulations, and sizing scripts — is built on a fully **open-source** toolchain and an **open-source PDK**, so you can reproduce every result yourself.

👉 **[Read the course online](https://iic-jku.github.io/analog-circuit-design/aicd.html)**

---

## Contents

- [What You Will Learn](#what-you-will-learn)
- [Toolchain](#toolchain)
- [Getting Started](#getting-started)
- [Repository Layout](#repository-layout)
- [Building the Document](#building-the-document)
- [Testing](#testing)
- [Contributing](#contributing)
- [Citation](#citation)
- [License](#license)

## What You Will Learn

The course progresses from single transistors to complete amplifiers, using the **gm/ID methodology** for systematic sizing and simulation for verification:

1. Introduction and first steps with the tools
2. Transistor sizing with gm/ID lookup tables
3. The MOSFET diode
4. The common-source amplifier
5. Current mirrors
6. The differential pair
7. A basic 5-transistor OTA
8. The cascode stage
9. Improved (cascoded) current mirrors
10. An improved OTA
11. Biasing (incl. bandgap references)
12. A fully differential OTA
13. An RC-OPAMP filter

## Toolchain

| Tool | Purpose |
| --- | --- |
| [**Xschem**](https://xschem.sourceforge.io) | Schematic entry |
| [**ngspice**](https://ngspice.sourceforge.io) | Circuit simulation |
| [**IHP SG13G2**](https://github.com/IHP-GmbH/IHP-Open-PDK) | 130 nm SiGe BiCMOS open-source PDK from [IHP Microelectronics](https://www.ihp-microelectronics.com) |
| [**pygmid**](https://pypi.org/project/pygmid/) + Jupyter | gm/ID-based transistor sizing |
| [**CACE**](https://github.com/fossi-foundation/cace) | Automated circuit characterization (PVT corners, Monte Carlo) |
| [**Quarto**](https://quarto.org) | Renders the course text to HTML and PDF |

All of the EDA tools and the PDK are bundled in the [**IIC-OSIC-TOOLS**](https://github.com/iic-jku/IIC-OSIC-TOOLS) Docker image, which is the recommended environment for the coursework.

## Getting Started

1. **Install IIC-OSIC-TOOLS** by following the instructions in its [repository](https://github.com/iic-jku/IIC-OSIC-TOOLS) (version 2025.03 or newer is recommended).
2. **Clone this repository** inside the container's designs directory:

   ```bash
   cd $DESIGNS
   git clone https://github.com/iic-jku/analog-circuit-design.git
   cd analog-circuit-design
   ```

3. **Load the environment.** The `.designinit` file switches the container to the IHP SG13G2 PDK and points Xschem at the local libraries. Source it (or restart the container shell from this directory):

   ```bash
   source .designinit
   ```

4. **Open a schematic** and simulate it from within Xschem:

   ```bash
   cd xschem
   xschem ota-5t_tb-ac.sch
   ```

5. **Run the sizing notebooks** (Python dependencies are listed in `requirements.txt`):

   ```bash
   pip install -r requirements.txt
   jupyter lab gmid/
   ```

## Repository Layout

```
.
├── aicd.qmd            # Main Quarto document (includes all chapters)
├── content/            # Chapter sources (.qmd), one folder per topic
├── xschem/             # Schematics, symbols, and testbenches (+ SVG renders)
├── gmid/               # gm/ID lookup tables and Jupyter sizing notebooks
├── cace/               # CACE characterization setups, templates, and reports
├── references.bib      # Bibliography
├── _quarto.yml         # Quarto project configuration
├── .designinit         # Environment setup for IIC-OSIC-TOOLS
├── convertsch2svg.sh   # Regenerate SVG figures from Xschem schematics
└── clean.sh            # Remove build artifacts
```

## Building the Document

The course text is written in [Quarto](https://quarto.org). To render it locally:

```bash
pip install -r requirements.txt
quarto render aicd.qmd            # HTML and Typst/PDF output
quarto preview aicd.qmd           # live preview while editing
```

The published version is built automatically by GitHub Actions on every push to `main`.

## Testing

Continuous integration keeps the material consistent:

- **Simulation test** — netlists and simulates every Xschem testbench with ngspice:

  ```bash
  xschem/run_simulation_tests.sh              # all testbenches
  JOBS=4 xschem/run_simulation_tests.sh       # limit parallelism
  ```

- **Notebook test** — executes every gm/ID notebook end-to-end:

  ```bash
  gmid/test_notebooks.sh
  ```

- **ShellCheck** lints all shell scripts, and SVG figures are regenerated automatically when schematics change.

## Contributing

We happily accept [pull requests](https://github.com/iic-jku/analog-circuit-design/pulls) to fix typos or add content! If something is unclear or you want to discuss an idea, please [open an issue](https://github.com/iic-jku/analog-circuit-design/issues/new).

When changing schematics, please make sure the simulation test still passes.

## Citation

If you use this material, please cite it (see [`CITATION.cff`](CITATION.cff)):

```bibtex
@misc{pretl_aicd,
  author = {Pretl, Harald and Koefinger, Michael and Dorrer, Simon},
  title  = {Analog Circuit Design},
  year   = {2024},
  doi    = {10.5281/zenodo.14387481},
  url    = {https://iic-jku.github.io/analog-circuit-design}
}
```

## License

All course material is publicly available and shared under the [Apache-2.0 license](LICENSE).
