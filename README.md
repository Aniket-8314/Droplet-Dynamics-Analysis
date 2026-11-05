# Droplet Dynamics Analysis

A MATLAB-based image-processing pipeline for detecting a droplet in
high-speed video, measuring its geometry, tracking its motion, and
identifying bounce events.

The project processes a video frame by frame and converts pixel
measurements into physical dimensions using an automatically detected
calibration reference.

## Features

-   **Video processing:** Reads the input video and converts RGB frames
    to grayscale.
-   **Automatic image calibration:** Detects a blue reference boundary
    and a red 2 mm scale bar in a calibration image to calculate the
    physical scale in mm/pixel.
-   **Droplet segmentation:** Uses image binarization, region filtering,
    and hole filling to isolate the droplet.
-   **Boundary and geometry extraction:** Detects the droplet boundary
    and calculates its area and bounding rectangle.
-   **Frame-wise measurements:** Extracts horizontal and vertical
    diameters, centroid position, top and bottom boundary heights, and
    estimated volume.
-   **Bounce detection:** Smooths the centroid trajectory with a
    Savitzky-Golay filter and detects candidate bounce points using
    local minima.
-   **Visualization and export:** Generates plots showing the evolution
    of droplet dimensions, volume, centroid motion, and boundary
    positions.

## Technologies

-   MATLAB
-   Image Processing Toolbox
-   Signal Processing functionality (`smoothdata`, `findpeaks`)
-   MATLAB VideoReader

## Project Workflow

1.  **Load the video:** Read the first frame and determine the image
    dimensions.
2.  **Calibrate the image:** Read `calibration_img.png`, detect the blue
    reference boundary and red 2 mm scale bar, and calculate
    `mmPerPixel`.
3.  **Segment the droplet:** Convert frames to grayscale, apply image
    binarization, remove irrelevant image regions, fill holes, and
    retain the largest connected region.
4.  **Analyze a single frame:** Extract and visualize the droplet
    boundary, calculate its area in pixels and mm², and draw its
    bounding rectangle.
5.  **Process the full video:** For each frame, calculate the droplet's
    horizontal and vertical bounding-box dimensions, centroid height,
    top and bottom boundary heights, and estimated volume.
6.  **Detect bounce events:** Smooth the centroid-height signal and
    identify sufficiently prominent local minima as candidate
    contact/bounce events.
7.  **Generate plots:** Export the measurement plots for further
    analysis.

## Input Files

Place the following files in the MATLAB current working folder:

  -----------------------------------------------------------------------
  File                                Purpose
  ----------------------------------- -----------------------------------
  `ME3101_project1.mlx`               MATLAB Live Script containing the
                                      analysis pipeline

  `sample.mp4`                        High-speed video to be analyzed

  `calibration_img.png`               Calibration image containing the
                                      reference boundary and 2 mm scale
                                      bar
  -----------------------------------------------------------------------

The calibration image is expected to contain the blue reference boundary
and red scale bar used by the script's colour-thresholding procedure.

## Requirements

-   MATLAB
-   Image Processing Toolbox
-   A MATLAB installation that supports Live Scripts (`.mlx`)
-   Input video and calibration image in the current working folder

The script uses functions including `VideoReader`, `rgb2gray`,
`imbinarize`, `bwareaopen`, `imfill`, `bwareafilt`, `bwboundaries`,
`regionprops`, `smoothdata`, `findpeaks`, and `exportgraphics`.

## How to Run

1.  Clone or download this repository.
2.  Open MATLAB.
3.  Set the MATLAB current folder to the project directory.
4.  Add `sample.mp4` and `calibration_img.png` to that directory.
5.  Open `ME3101_project1.mlx`.
6.  Run the Live Script.

The script processes the video and exports the analysis figures into the
current working directory.

## Generated Outputs

  -----------------------------------------------------------------------
  Output                              Description
  ----------------------------------- -----------------------------------
  `Figure2.jpg`                       Detected droplet boundary on the
                                      first frame

  `Figure3.jpg`                       Droplet bounding rectangle on the
                                      first frame

  `Figure4.jpg`                       Horizontal and vertical diameter
                                      evolution over time

  `Figure5.jpg`                       Estimated droplet volume over time

  `Figure6.jpg`                       Centroid vertical position with
                                      detected bounce points

  `Figure7.jpg`                       Top and bottom boundary height
                                      tracking
  -----------------------------------------------------------------------

The script also prints the frame dimensions, calibration scale,
first-frame droplet area, and detected bounce count in the MATLAB
Command Window.

## Measurement Notes and Assumptions

-   The video frame rate is set in the script to **3000 frames per
    second** (`fps_actual = 3000`). Update this value if the input video
    was recorded at a different frame rate.
-   The calibration routine assumes the calibration image contains
    detectable blue and red reference features, with the red scale bar
    representing **2 mm**.
-   The segmentation uses fixed image-row exclusions and threshold-based
    foreground extraction. These settings may need adjustment for videos
    with different framing, lighting, background, or droplet appearance.
-   The volume is estimated by treating each image row through the
    segmented droplet as a circular cross-section and summing the
    corresponding disk volumes. This is an axisymmetric approximation,
    not a direct 3D measurement.
-   Bounce detection identifies candidate local minima in the smoothed
    centroid-height signal using configured distance and prominence
    thresholds. Results depend on video quality and parameter settings.

## Repository Structure

``` text
Droplet-Dynamics-Analysis/
├── ME3101_project1.mlx
├── sample.mp4                 # Input video (provide separately)
├── calibration_img.png        # Calibration image (provide separately)
└── README.md
```

## Future Improvements

-   Make segmentation thresholds adaptive to changing illumination and
    backgrounds.
-   Expose frame rate, calibration scale, and segmentation parameters as
    user-configurable settings.
-   Add validation of detected droplets and handling for frames where
    segmentation fails.
-   Export frame-wise measurements to CSV for downstream analysis.
-   Compare estimated volume and bounce events against experimental
    reference measurements.

## Author

**Aniket Kumar**\
B.Tech, Mechanical Engineering\
Indian Institute of Technology Patna
