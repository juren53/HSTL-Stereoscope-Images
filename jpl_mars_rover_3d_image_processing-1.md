# NASA/JPL Mars Rover Stereoscopic 3D Processing Pipeline: Documentation & Resources

To share the Martian landscape with the general public in 3D, NASA’s Jet Propulsion Laboratory (JPL) developed a sophisticated end-to-end processing pipeline. Because rovers like *Spirit*, *Opportunity*, *Curiosity*, and *Perseverance* are built with stereo camera pairs (such as Navcams, Hazcams, and Mastcams/Pancams) emulating human binocular vision, they capture natural offset views of the terrain.

JPL converted this raw telemetry and imaging data into public 3D formats using several core stages:

---

## Core Processing Stages

### 1. Telemetry Ingestion and Preprocessing
When raw image data packets are beamed back to Earth via the Deep Space Network, JPL computers first unpack the data and perform radiometric and geometric corrections. This involves:
* **Linearization and Flat-Fielding:** Removing camera sensor artifacts, correcting for electronic noise, and adjusting pixel brightness across uneven lighting.
* **Camera Calibration Correction:** Applying known laboratory calibration matrices for focal length, optical distortion, and exact physical separation (baseline) between the left and right camera lenses.

### 2. Image Rectification
Before stereoscopic math can happen, the left and right images must be mathematically aligned. JPL software processed the image pairs so that corresponding features (like a rock or ridge) lie on the exact same horizontal pixel row (epipolar line). This eliminates vertical offset errors, ensuring that subsequent computer algorithms or human eyes only need to scan horizontally to calculate depth.

### 3. Producing Public-Friendly 3D Formats
Depending on the media and the era of the mission, JPL distributed these processed stereo pairs in a few primary ways for the public:
* **Red-Blue/Red-Cyan Anaglyphs:** For decades, JPL’s imaging teams quickly generated classic 3D anaglyphs for media releases and the public web. Software tinted the left-eye image red and the right-eye image cyan (or blue), then superimposed them with a slight horizontal offset (disparity). When viewed through red-blue cardboard glasses, each eye received only its designated channel, causing the flat image to pop into crisp 3D relief.
* **Stereo Pairs (Side-by-Side / Cross-Eyed):** JPL routinely released raw and processed left- and right-eye image files directly to public image galleries. Enthusiasts could view them side-by-side using free stereoscopes or by cross-focusing their eyes.
* **Digital Elevation Models (DEMs) and Mesh Generation:** For deeper scientific visualization and advanced public web tools, JPL and partner institutions utilized heavy-duty photogrammetry tools (such as the open-source NASA Ames Stereo Pipeline). The software ran automated pixel-correlator algorithms across the stereo pairs to measure pixel disparity, translating it into metric range data to create dense 3D point clouds, wireframe meshes, and textured terrain models.

### 4. Interactive Web Experiences and VR
In later missions, JPL took stereoscopic processing a step further for public outreach by collaborating with tech partners (such as Google on projects like *Access Mars*). By feeding sequences of overlapping stereo navcam and hazard camera images into automated photogrammetry pipelines, JPL converted raw orbital and rover stereo data into fully rendered, interactive 3D 360-degree environments.

---

## Source Information & Where to Get Raw Data

### Technical Software & Open-Source Tools
* **NASA Ames Stereo Pipeline (ASP):** The premier suite of automated geodesy and stereogrammetry software used to process planetary imagery.
  * Repository & Code: [GitHub - NeoGeographyToolkit/StereoPipeline](https://github.com/NeoGeographyToolkit/StereoPipeline)
  * Official Documentation & Tutorials: [ASP ReadTheDocs](https://stereopipeline.readthedocs.io/)
  * Beginner Quickstart Guides: [GitHub - uw-cryo/stereopipeline-quickstart](https://github.com/uw-cryo/stereopipeline-quickstart)
* **USGS Integrated Software for Imagers and Spectrometers (ISIS):** Maintained by the U.S. Geological Survey’s Astrogeology Science Center for handling radiometric calibration and camera models.
  * Software Overview & Manual: [USGS ISIS Documentation](https://isis.astrogeology.usgs.gov/7.0.0/UserInspire/index.html)

### Primary Data Archives (Raw Files & Telemetry)
* **NASA Planetary Data System (PDS) Image Atlas:** Contains uncompressed raw data archives returned by every Mars mission.
  * Main Portal: [NASA PDS Image Atlas](https://pds-imaging.jpl.nasa.gov/)
  * Dataset Catalog: [NASA Open Data Portal - PDS](https://data.nasa.gov/dataset/planetary-data-system-pds)
* **Mission-Specific Raw Image Galleries:**
  * [Perseverance (Mars 2020) Raw Images](https://science.nasa.gov/mission/mars-2020-perseverance/multimedia/raw-images/)
  * [Curiosity (MSL) Raw Images](https://science.nasa.gov/mission/msl-curiosity/multimedia/raw-images/)

### Public 3D & Interactive Platforms
* **NASA Mars Trek / Access Mars:** Web-based 3D visualization tools developed by JPL allowing users to explore interactive maps and 3D environments derived from real rover stereo data.