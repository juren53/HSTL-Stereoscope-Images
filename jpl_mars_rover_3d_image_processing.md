# Processing Stereoscopic Mars Rover Imagery: Deep Dive & Source Guide

*Last updated: 2026-11-04-1522*

To share the Martian landscape with the general public in 3D, NASA’s Jet Propulsion Laboratory (JPL) developed a sophisticated end-to-end processing pipeline. Because rovers like *Spirit*, *Opportunity*, *Curiosity*, and *Perseverance* are built with stereo camera pairs (such as Navcams, Hazcams, and Mastcams/Pancams) emulating human binocular vision, they capture natural offset views of the terrain.

JPL converts this raw telemetry and imaging data into public 3D formats using several core stages:

---

## 1. Technical Pipeline Overview

### 1.1 Telemetry Ingestion and Preprocessing
When raw image data packets are beamed back to Earth via the Deep Space Network, JPL computers first unpack the data and perform radiometric and geometric corrections. This involves:
* **Linearization and Flat-Fielding:** Removing camera sensor artifacts, correcting for electronic noise, and adjusting pixel brightness across uneven lighting.
* **Camera Calibration Correction:** Applying known laboratory calibration matrices (e.g., CAHV, CAHVOR, and CAHVORE camera models) for focal length, optical distortion, and exact physical separation (baseline) between the left and right camera lenses.

### 1.2 Image Rectification
Before stereoscopic math can happen, the left and right images must be mathematically aligned. JPL software processes the image pairs so that corresponding features (like a rock or ridge) lie on the exact same horizontal pixel row (epipolar line). This eliminates vertical offset errors, ensuring that subsequent computer algorithms or human eyes only need to scan horizontally to calculate depth.

### 1.3 Producing Public-Friendly 3D Formats
Depending on the media and the era of the mission, JPL distributes these processed stereo pairs in several primary ways for the public:

* **Red-Blue/Red-Cyan Anaglyphs:** For decades, JPL’s imaging teams quickly generated classic 3D anaglyphs for media releases and the public web. Software tints the left-eye image red and the right-eye image cyan (or blue), then superimposes them with a slight horizontal offset (disparity). When viewed through red-blue cardboard glasses, each eye receives only its designated channel, causing the flat image to pop into crisp 3D relief.
* **Stereo Pairs (Side-by-Side / Cross-Eyed):** JPL routinely releases raw and processed left- and right-eye image files directly to public image galleries. Enthusiasts can view them side-by-side using free stereoscopes or by cross-focusing their eyes.
* **Digital Elevation Models (DEMs) and Mesh Generation:** For deeper scientific visualization and advanced public web tools, JPL and partner institutions utilize heavy-duty photogrammetry tools (such as the open-source NASA Ames Stereo Pipeline). The software runs automated pixel-correlator algorithms across the stereo pairs to measure pixel disparity. Because the geometry and camera angles are precisely known, disparity is automatically translated into metric range data, creating dense 3D point clouds, wireframe meshes, and textured terrain models.

### 1.4 Interactive Web Experiences and VR
In later missions, JPL took stereoscopic processing a step further for public outreach by collaborating with tech partners (such as Google on projects like *Access Mars*). By feeding sequences of overlapping stereo navcam and hazard camera images into automated photogrammetry pipelines, JPL converted raw orbital and rover stereo data into fully rendered, interactive 3D 360-degree environments.

---

## 2. Where to Access Source Data & Documentation

If you want to explore the math, software tools, raw telemetry files, or interactive models yourself, several primary public resources are available:

### Open-Source Processing Software & Tools
* **The NASA Ames Stereo Pipeline (ASP):** Developed by the Intelligent Systems Division at NASA Ames Research Center, ASP is the premier suite of automated geodesy and stereogrammetry software used to process planetary imagery.
  * **Repository:** [GitHub - NeoGeographyToolkit/StereoPipeline](https://github.com/NeoGeographyToolkit/StereoPipeline)
  * **Documentation & Tutorials:** [ASP ReadTheDocs](https://stereopipeline.readthedocs.io/)
  * **Beginner Quickstart Guides:** [GitHub - uw-cryo/stereopipeline-quickstart](https://github.com/uw-cryo/stereopipeline-quickstart)
* **USGS Integrated Software for Imagers and Spectrometers (ISIS):** Maintained by the U.S. Geological Survey’s Astrogeology Science Center, ISIS handles radiometric calibration, camera models, and geometric corrections for planetary datasets before stereo correlation.
  * **Software Overview & Manual:** [USGS ISIS Documentation](https://isis.astrogeology.usgs.gov/7.0.0/UserInspire/index.html)

### Primary Data Archives (Raw Telemetry)
To pull raw left/right stereo image pairs (`.img` / PDS format) and camera geometry files:
* **NASA Planetary Data System (PDS) Image Atlas:** The complete, uncompressed historical raw data archive returned by Mars missions.
  * **Main Portal:** [NASA PDS Image Atlas](https://pds-imaging.jpl.nasa.gov/)
  * **Dataset Catalog:** [NASA Open Data Portal - PDS](https://data.nasa.gov/dataset/planetary-data-system-pds)
* **Mission Raw Image Galleries:**
  * [Perseverance (Mars 2020) Raw Images](https://science.nasa.gov/mission/mars-2020-perseverance/multimedia/raw-images/)
  * [Curiosity (MSL) Raw Images](https://science.nasa.gov/mission/msl-curiosity/multimedia/raw-images/)

### Public 3D & Interactive Platforms
* **NASA Mars Trek & Access Mars:** Web-based 3D visualization tools developed by JPL and tech partners that allow users to explore interactive maps, 3D terrain, and 3D environments derived from real rover stereo data.
* **AreoBrowser:** A popular independent visualization tool that aggregates public raw image streams and allows interactive viewing of rover stereo pairs in cross-eyed, side-by-side, and anaglyph modes.

### Key Literature & Technical Papers
* *The Ames Stereo Pipeline: NASA's Open Source Software for Deriving and Processing Terrain Data* (Journal of Earth and Space Science).
* JPL Robotics technical papers on *CAHVOR camera models* and *Autonomous Rover Navigation & Stereo Disparity Calculation*.

---

*Last updated: 2026-11-04-1522*
