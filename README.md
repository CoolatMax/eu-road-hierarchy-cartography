# OpenStreetMap Road Hierarchy & Print Cartography (QGIS & ETRS89)

![QGIS](https://img.shields.io/badge/QGIS-3.34_LTR-588240?logo=qgis&logoColor=white)
![Format](https://img.shields.io/badge/Output-A3_PDF_Map-red)
![CRS](https://img.shields.io/badge/Projection-EPSG%3A3035%20(ETRS89)-003399)

## Project Overview
This repository contains the cartographic pipeline for converting raw OpenStreetMap vector road geometries into a high-precision, print-ready A3 spatial map product.

The workflow focuses on cartographic design principles: rule-based multi-tier line symbology, curved street labeling with collision suppression, and production of a standardized ISO A3 publication layout.

## Objectives
1. **Cartographic Categorization:** Classify `highway` attributes into a structured 5-tier functional hierarchy (`Motorway`, `Primary / Secondary`, `Tertiary`, `Residential / Local`, `Pedestrian / Path`).
2. **Rule-Based Symbology:** Apply distinct stroke widths, color gradients, and join styles to visually separate arterial transit from local access roads.
3. **Curved Line Labeling:** Configure dynamic placement engines for curved street labels following line orientation, equipped with text buffers (halos) and scale-dependent visibility.
4. **Print Layout Composition:** Design a publication-ready ISO A3 Landscape map layout containing a dynamic legend, metric scale bar, locator map, grid coordinates, and data attribution.

---

## Cartographic Rules & Categorization

| Rank | Functional Class | OSM `highway` Values | Line Width (mm) | Visual Palette |
| :---: | :--- | :--- | :---: | :--- |
| **1** | Motorway & Trunk | `motorway`, `trunk`, `motorway_link` | 1.20 mm | Deep Coral Red (`#D32F2F`) |
| **2** | Primary & Secondary | `primary`, `secondary`, `primary_link` | 0.85 mm | Vibrant Amber (`#F57C00`) |
| **3** | Tertiary | `tertiary`, `tertiary_link` | 0.55 mm | Soft Yellow (`#FBC02D`) |
| **4** | Residential & Unclassified | `residential`, `unclassified`, `living_street` | 0.30 mm | Charcoal Grey (`#616161`) |
| **5** | Service & Pedestrian | `service`, `pedestrian`, `footway`, `path` | 0.15 mm | Light Grey / Dashed (`#9E9E9E`) |

---

## Workflow Implementation

### Step 1: Symbology Hierarchy
* **QGIS Manual Reference:** *Sections 2.4, 3.3*
* Established rule-based rendering using SQL expressions on the `highway` field.
* Applied symbol levels to ensure smooth road intersection rendering without line-overlap artifacts.

### Step 2: Line Labeling Engine
* **QGIS Manual Reference:** *Section 3.2.5*
* Modeled labels using the **Curved** placement algorithm along line geometries.
* Added a 0.8mm white mask buffer (halo) to prevent vector line clipping through street names.
* Configured scale-dependent visibility to render minor street labels only at scales larger than $1:15,000$.

### Step 3: Layout Composition & Export
* **QGIS Manual Reference:** *Sections 4.1, 4.2*
* Built an ISO A3 Landscape ($420 \times 297\text{ mm}$) canvas inside QGIS Print Layout.
* Added map items:
  * Primary map frame set to metric scale $1:25,000$.
  * Single-segment metric scale bar in kilometers ($km$).
  * Grouped legend filtering out non-visible canvas features.
  * Spatial metadata block detailing CRS (`EPSG:3035`), data provider (OpenStreetMap contributors), and author details.
* Exported to 300 DPI vector PDF: `exports/brussels_road_hierarchy_A3.pdf`.

---

## Repository Structure

```text
├── README.md
├── exports/
│   └── example.pdf
├── styles/
│   ├── example1.qml
│   └── example2.qml
├── data/
│   └── raw/
│       └── example.gpkg
│   └── processed/
│       └── example.gpkg
└── qgis/
    └── example.qgz
