<div align="center">

<img src="logos/odd_tech_light.png" alt="Open Data Detector" width="520" />

# The Open Data Detector

**A realistic, open, HL-LHC–style detector for the next generation of reconstruction algorithms.**

[![License](https://img.shields.io/badge/license-MPL--2.0-blue.svg)](LICENSE)
[![DD4hep](https://img.shields.io/badge/DD4hep-compatible-success)](https://dd4hep.web.cern.ch/dd4hep/)
[![ACTS](https://img.shields.io/badge/ACTS-integrated-orange)](https://acts-project.github.io)
[![Geant4](https://img.shields.io/badge/Geant4-ready-9cf)](https://geant4.web.cern.ch)
[![C++ Standard](https://img.shields.io/badge/C%2B%2B-17%2B-00599C?logo=cplusplus)](https://isocpp.org/)

[**Quick start**](#-quick-start) ·
[**Layout**](#-detector-layout) ·
[**Datasets (ColliderML)**](#-colliderml--data-releases) ·
[**Cite**](#-citing-the-odd) ·
[**Logos**](#-branding)

</div>

---

## ✨ What is the ODD?

The **Open Data Detector (ODD)** is a fictitious, experiment-agnostic silicon-tracking-and-calorimetry detector designed to look and behave like a real HL-LHC apparatus — without belonging to any one collaboration.

It is the successor to the **TrackML** geometry, built to support modern R&D in:

- 🧭 **Track reconstruction** — Kalman filtering, CKF, GNNs, transformers
- 🔬 **Detector simulation** — full Geant4 with realistic passive material
- 🤖 **End-to-end ML** — particle flow, jet tagging, low-level event reconstruction
- 📐 **Algorithm benchmarking** — a stable, shared baseline across the community

It is the reference geometry for [**ACTS**](https://acts.readthedocs.io) and the foundation of the [**ColliderML**](https://colliderml.com) benchmark dataset.

---

## 🧱 Detector Layout

The ODD is organized as a **modular, staged DD4hep description** — load only what you need.

| Stage | Composition | Inspired by |
|---|---|---|
| 🟦 **Inner Tracker** | High-resolution pixels → "strixel" modules → 2D strip layers | ATLAS Phase-2 ITk |
| 🌀 **Solenoid** | 3 T inner field, 0.5 T return field | Hybrid ATLAS / CMS |
| ⚡ **ECal** | 48 layers, tungsten absorber, 5.1 × 5.1 mm² Si sensors | FCC-ee detector proposals |
| 🔥 **HCal** | 36 layers, steel absorber, 30 × 30 mm² scintillator cells | CMS HGCal, CLD, AHCAL, SiD |
| 🧲 **Muon system** | Outer tracking layers | LHC general-purpose detectors |

Time information is modeled at hit level, enabling HGTD-style timing studies.

> Geometry overview adapted from the ColliderML release at ACAT 2025 (Murnane et al.).

### Subdetector files

```
xml/
├── OpenDataDetector.xml              # full detector (backward-compatible)
├── OpenDataDetectorDefs.xml          # global defs, materials, fields, envelopes
├── OpenDataDetectorActsSupport.xml   # ACTS material binning
├── OpenDataDetectorTracker.xml       # beampipe + pixels + strips + solenoid
├── OpenDataDetectorCalorimeter.xml   # ECal + HCal
└── OpenDataDetectorMuonSystem.xml    # muon stage
```

---

## 🚀 Quick Start

### Build

Dependencies: **Boost**, **DD4hep**, **ROOT**, **Geant4**.

```shell
cmake -S <source> -B <build> \
  -DDD4hep_DIR=<path_to_DD4hep> \
  -DGeant4_DIR=<path_to_Geant4> \
  -DROOT_DIR=<path_to_ROOT> \
  -DCMAKE_CXX_STANDARD=17

cmake --build <build>
```

### Visualize the full detector

```shell
geoDisplay xml/OpenDataDetector.xml -load
```

### Compose subsystems on the fly

Tracker only:
```shell
geoDisplay -input xml/OpenDataDetectorDefs.xml \
           -input xml/OpenDataDetectorTracker.xml -load
```

Tracker + calorimeter:
```shell
geoDisplay -input xml/OpenDataDetectorDefs.xml \
           -input xml/OpenDataDetectorTracker.xml \
           -input xml/OpenDataDetectorCalorimeter.xml -load
```

Full staged chain (global + tracker + calo + muon):
```shell
geoDisplay -input xml/OpenDataDetectorDefs.xml \
           -input xml/OpenDataDetectorTracker.xml \
           -input xml/OpenDataDetectorCalorimeter.xml \
           -input xml/OpenDataDetectorMuonSystem.xml -load
```

Or explore interactively with `geoPluginRun`:

```shell
geoPluginRun -input xml/OpenDataDetector.xml -interactive \
             -plugin DD4hep_GeometryDisplay -level 8
```

---

## 📊 ColliderML — Data Releases

The ODD is the geometry behind **[ColliderML](https://colliderml.com)**, the largest open simulated HL-LHC dataset to date — **roughly 1000× the data volume of TrackML**.

**Release 1 highlights:**

- 🎯 **1 M events** at √s = 14 TeV, ⟨μ⟩ = 200 pile-up
- 🧪 10 SM + BSM channels: tt̄, Z→ℓℓ, γγ, ggF Higgs, di-Higgs, SUSY, Z′, HNL, Hidden Valley
- 🧰 NLO ME (MadGraph aMC@NLO), Pythia 8 showering, FxFx/CKKW matching
- 🧱 Full Geant4 simulation + ACTS digitization + CKF tracking
- 📦 Two formats: **EDM4hep ROOT** (~100 TB, full truth) and **HDF5** (~1 TB, ML-friendly)
- 🌍 Mirrored at NERSC (US) and EOS (Europe)

→ See [`colliderml.com`](https://colliderml.com) for download tools and tutorials.

---

## 🧩 Use with ACTS

The ODD ships as the reference geometry inside ACTS' Python full-chain example. From the ACTS docs:

```python
from acts.examples.odd import getOpenDataDetector, getOpenDataDetectorDirectory
import acts

oddDir          = getOpenDataDetectorDirectory()
oddMaterialMap  = oddDir / "data/odd-material-maps.root"
oddMaterialDeco = acts.IMaterialDecorator.fromFile(oddMaterialMap)

detector, trackingGeometry, decorators = getOpenDataDetector(
    oddDir,
    mdecorator=oddMaterialDeco,
)
```

Pre-tuned configuration files are provided for digitization (`odd-digi-smearing-config.json`) and seeding (`odd-seeding-config.json`).

→ Full walkthrough: [ACTS — full chain ODD example](https://acts.readthedocs.io/en/latest/examples/full_chain_odd.html)

---

## 🎨 Branding

Logo assets live under [`logos/`](logos/).

| Variant | Use case |
|---|---|
| <img src="logos/odd_tech_light.png" alt="ODD tech light" width="220" /> | **Tech / light** — high-contrast treatment for documentation, slides, and print |
| <img src="logos/odd_retro_80s.png" alt="ODD retro 80s" width="220" /> | **Retro / synthwave** — neon lockup for informal or campaign-style use |
| <img src="logos/odd_console_data.png" alt="ODD console data" width="220" /> | **Console / data** — terminal-style treatment for tooling, demos, and hacker-lab contexts |

---

## 📚 Citing the ODD

If you use the ODD in published work, please cite it. Suggested references:

- **The Open Data Detector Tracking System** — Allaire et al., *J. Phys.: Conf. Ser.* (2023). [INSPIRE-HEP](https://inspirehep.net/literature/2628797)
- **A Common Tracking Software Project (ACTS)** — Ai et al. (2022). [arXiv:2106.13593](https://arxiv.org/abs/2106.13593)
- **ColliderML: First Release of an OpenDataDetector HL-LHC Benchmark Dataset** — Murnane et al., ACAT 2025.

---

## 🤝 Contributing & Community

The ODD lives at [**gitlab.cern.ch/acts/OpenDataDetector**](https://gitlab.cern.ch/acts/OpenDataDetector). Contributions, issues, and merge requests are very welcome — particularly:

- Subdetector refinements (muon system, timing layers)
- Material map improvements
- New benchmark scenarios for ColliderML releases
- Tooling for non-ACTS users (Key4hep, DUNE-style stacks, custom frameworks)

Discussion happens in the ACTS Mattermost channels and the connecting-the-dots community.

---

<div align="center">

**Built by and for the HEP reconstruction community.**
*A shared baseline so we can stop arguing about geometry and start comparing algorithms.*

</div>
