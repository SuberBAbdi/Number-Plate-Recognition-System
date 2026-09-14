# Automated License Plate Recognition (ALPR) System

A Graphic User Interface (GUI) application developed in MATLAB App Designer to process vehicle images, analyze image quality metrics, and isolate license plates using advanced mathematical morphology and spatial domain filtering[cite: 1, 3].

Developed for the course **Sensors and Signals (5ELEN021W.1)** at the **University of Westminster** by **Group 1**[cite: 3]:
* **Suber Abdi** (w2069210)[cite: 3]
* **Christian Dave Bergonia** (w1985516)[cite: 3]
* **Caleb Suleaudu** (w1910808)[cite: 3]

---

## Overview

The system provides an interactive multi-image workspace capable of executing end-to-end license plate extraction[cite: 1, 3]. It handles up to three vehicle images simultaneously, allowing side-by-side evaluation of raw inputs, converted grayscale representations, pixel intensity histograms, and final segmented binary masks[cite: 1, 3]. 

The primary goal is to isolate rectangular license plate candidate regions from complex background environments while maintaining low computational requirements suitable for real-time edge processing[cite: 3].

---

## Key Features

* **Multi-Canvas GUI Workspace**: Interactive MATLAB App Designer UI featuring interactive axes (`UIAxes1` through `UIAxes9`) for tracking batch operations[cite: 1, 3].
* **NTSC Weighted Grayscale Reduction**: Converts 24-bit RGB images into 8-bit luminance representations based on human visual sensitivity curves[cite: 3].
* **RMS Contrast Evaluation**: Quantifies image contrast levels using Root Mean Square standard deviation metrics[cite: 3].
* **Custom Intensity Histogram Analysis**: Computes pixel frequency across 256 intensity bins ($0$ to $255$) without relying on black-box utilities[cite: 1, 3].
* **Morphological Plate Isolation**: Integrates 2D median filtering with rectangular structuring elements to strip impulse noise and extract high-aspect-ratio plate boundaries[cite: 3].

---

## Technical & Algorithm Pipeline

```
[Raw RGB Image] ──> [Grayscale Conversion] ──> [Median Noise Filter] ──> [Binary Thresholding] ──> [Morphological Opening/Closing] ──> [Isolated Plate Region]
```

### 1. Grayscale Conversion
Converts multi-channel RGB matrix values into single-channel luminance intensity $J$ using NTSC standard weighting coefficients[cite: 1, 3]:
$$\text{Gray} = 0.299 \cdot R + 0.587 \cdot G + 0.114 \cdot B$$
This weighting optimizes feature preservation by prioritizing the green spectrum, which aligns with human visual perception[cite: 3].

### 2. Root Mean Square (RMS) Contrast Analysis
Computes contrast standard deviation across all pixels within an $M \times N$ image matrix[cite: 3]:
$$C = \sqrt{\frac{1}{M \cdot N} \sum_{i=1}^{M} \sum_{j=1}^{N} \left(I(i,j) - \bar{I}\right)^2}$$
where $\bar{I}$ represents mean image intensity[cite: 3]. Higher values indicate greater luminance dynamic range[cite: 3].

### 3. Spatial Noise Reduction (2D Median Filtering)
A $3 \times 3$ median filter (`medfilt2`) is applied across the grayscale matrix to eliminate salt-and-pepper transmission noise while preserving critical high-frequency edges around characters and plate borders[cite: 3].

### 4. Binarization & Thresholding
Converts the filtered intensity image into a binary mask ($0$ for background, $1$ for foreground) via mid-range thresholding (`imbinarize`)[cite: 3].

### 5. Mathematical Morphology
To segment the license plate from background clutter, a custom $3 \times 15$ rectangular structuring element ($SE$) matching standard license plate aspect ratios is applied[cite: 3]:
* **Opening (`imopen`)**: Performs erosion followed by dilation to remove small background noise and thin extrusions without altering object sizes[cite: 3].
* **Closing (`imclose`)**: Performs dilation followed by erosion to seal small internal voids, connect adjacent character regions, and solidify the rectangular boundary of the plate[cite: 3].

---

## Pipeline Summary

| Processing Stage | Implementation / Function | Math / Parameters | Operational Objective |
| :--- | :--- | :--- | :--- |
| **Image Loading** | `uigetfile`, `imread` | File Explorer Interface | Loads RGB vehicle image into system memory[cite: 1, 3]. |
| **Grayscale Conversion** | Custom function `Grayscale()` | $0.299R + 0.587G + 0.114B$ | Reduces 24-bit color to single 8-bit luminance channel[cite: 1, 3]. |
| **Contrast Calculation** | Custom function `rmsContrast()` | $C = \sqrt{\frac{\sum (I - \bar{I})^2}{M \cdot N}}$ | Measures overall pixel intensity distribution[cite: 3]. |
| **Histogram Generation** | Custom function `Hist()` | 256 Intensity Bins ($0\text{--}255$) | Displays luminance density across all image pixels[cite: 1, 3]. |
| **Noise Reduction** | `medfilt2()` | Window Size: $[3 \times 3]$ | Removes impulse noise while preserving sharp plate edges[cite: 3]. |
| **Binarization** | `imbinarize()` | Dynamic / Fixed Thresholding | Converts gray scale matrix to boolean binary mask[cite: 3]. |
| **Plate Morphing** | `imopen()`, `imclose()` | `strel('rectangle', [3 15])` | Fills character gaps and isolates rectangular plate shape[cite: 3]. |

---

## Repository Structure

```
├── Final.m                                           # MATLAB App Designer GUI & logic codebase
├── Number_Plate_Recognition_System_Report.pdf        # Comprehensive academic project report
├── Number_Plate_Recognition_System_Presentation.pptx # Project presentation deck
├── LICENSE                                           # MIT License file
└── README.md                                         # Project documentation
```

---

## Prerequisites & Installation

### Requirements
* **MATLAB**: Version R2025b or newer[cite: 3].
* **Toolboxes**: MATLAB Image Processing Toolbox.

### Execution Steps
1. Clone this repository to your local directory:
   ```bash
   git clone [https://github.com/SuberBAbdi/Number-Plate-Recognition-System.git](https://github.com/SuberBAbdi/Number-Plate-Recognition-System.git)
   ```
2. Open MATLAB and navigate to the project directory.
3. Launch the application from the Command Window:
   ```matlab
   app = Final;
   ```

---

## User Interface Guide

1. **Load Image**: Click `Load Image 1`, `Load Image 2`, or `Load Image 3` to import vehicle images into the left axes column[cite: 1, 3].
2. **Convert to Grayscale**: Click `Grayscale Image` to process the loaded input into single-channel luminance format and print the calculated RMS contrast to the console[cite: 1, 3].
3. **Generate Histogram**: Click `Histogram of Image` to populate the 256-bin brightness distribution on the right axes column[cite: 1, 3].
4. **Isolate License Plate**: Click `Processed Version of Image` to execute median filtering, binarization, and morphological opening/closing in a dedicated output window[cite: 3].
5. **Reset Canvas**: Click the red **RESET** button to wipe all axes and restore the UI to its initial state[cite: 1, 3].

---

## Broader Impact & Considerations

* **Ethical Considerations**: Automated license plate recognition technologies must adhere to data privacy standards to avoid unauthorized surveillance, requiring clear retention limits and strict access controls[cite: 3].
* **Social Impact**: Enhances traffic management efficiency, automated toll collection, parking operations, and public security when deployed responsibly within legal frameworks[cite: 3].
* **Environmental Impact**: Algorithmic optimization and lightweight morphological techniques minimize computational demand, reducing energy consumption compared to heavy deep learning pipelines[cite: 3].

---

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
