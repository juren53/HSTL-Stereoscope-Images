# HSTL Stereoscope Images

Processing historic stereo photographs of Harry S. Truman and his contemporaries, and presenting them to the public the way NASA/JPL present Mars rover stereo imagery.

## Status

Planning stage. There is no code yet. The repository holds the project plan, background research, and one sample image.

Phase 1, a reference pipeline built on public-domain Keystone View Co. stereographs, can start now. Four decisions are still open: scope, the project's standing with the Library, who does the scanning, and whether depth products are in scope. See [Decisions needed](PLAN.md#decisions-needed).

## Approach

The JPL model transfers well, with two changes:

- **Source images are the main problem, not the processing.** The online copies of the Truman stereo portraits show only one eye, so both halves must be scanned before they can be used.
- **Metric 3D does not transfer.** Rover cameras are calibrated, and these cameras were not. Anaglyphs, stereo pairs and viewers are fully achievable, but depth products must be labeled as approximate reconstructions.

An early test on Keystone card `67-4227` showed that alignment and anaglyph generation work automatically at public resolution. It also showed that resolution is the limit for depth maps. It covers one card, so it is not yet a general result.

## Product levels

Each level can be reproduced from the one below, and nothing overwrites a master.

| Level | Product |
|---|---|
| L0 | Master scan of the whole object, front and back |
| L1 | Left and right eye crops |
| L2 | Aligned pair |
| L3 | Public products: anaglyph, side-by-side, wiggle GIF/video, MPO, spatial photo |
| L4 | Depth products, labeled as reconstructions |

## Phases

| Phase | Work |
|---|---|
| 0 | Inventory and permission |
| 1 | Reference pipeline on the Keystone cards |
| 2 | Master scans of a pilot set |
| 3 | Public viewer prototype |
| 4 | Scale and publish |
| 5 | Depth products (optional) |

## Contents

| Path | Description |
|---|---|
| [PLAN.md](PLAN.md) | Project plan: sources, feasibility test, data model, phases, risks and decisions |
| [VIEWING_MODES.md](VIEWING_MODES.md) | How each viewing mode works and how to build it: wiggle, anaglyph, cross-eyed, parallel and headset |
| [Questions-for-planning.txt](Questions-for-planning.txt) | Open questions and items not yet verified |
| [holograms_from_stereoscopic_images.md](holograms_from_stereoscopic_images.md) | Background: creating holograms and light field displays from stereo pairs |
| [jpl_mars_rover_3d_image_processing.md](jpl_mars_rover_3d_image_processing.md) | Background: JPL's rover stereo pipeline |
| [jpl_mars_rover_3d_image_processing-1.md](jpl_mars_rover_3d_image_processing-1.md) | Near-duplicate of the above, with a VR section and more links |
| [Images/](Images) | Sample image `2008-175.jpg`, the public copy of one eye of a Truman stereo card |
