---
icon: lucide/rocket
---

# Simulation of laser beam propagation (BPM)

## Project
We study the propagation of a Gaussian laser beam along z in a medium whose refractive index varies transversally.

The field is computed step by step with the split-step Fourier method: diffraction is applied in Fourier space, and the refractive index modulation in real space.

An interactive interface built with NiceGUI lets you choose the beam, the medium and the guide parameters, and displays the resulting intensity map $|A(x,z)|^2$ with a colorbar.
By varying these parameters across the five cases, we study diffraction, single-mode and multimode guiding and evanescent coupling.

## Team
The work was divided by component: each of us took charge of one part of the project, and all the code and this documentation are tracked on GitHub. [:fontawesome-brands-github: Code: bpm-1](https://github.com/OPTIQ-2026/bpm-1){ .md-button }

<div style="text-align: center;" markdown>
| Member   | Work                                        | GitHub |
| -------- | ------------------------------------------- | ------ |
| **SEVIM**   | Calculation class and simple index profile  | [:fontawesome-brands-github: Aylasy-12](https://github.com/Aylasy-12) |
| **PHILIPPE** | Benchmarks and physical analysis            | [:fontawesome-brands-github: Philippe-RUET 	](https://github.com/Philippe-RUET 	) |
| **MIRIAM**   | NiceGUI interface and documentation         | [:fontawesome-brands-github: miriamlaidi](https://github.com/miriamlaidi) |
</div>