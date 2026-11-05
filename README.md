# HSTL Stereoscope Images

*Last updated: 2026-11-05-0703*

Processing historic stereo photographs of Harry S. Truman and his contemporaries, and presenting them to the public the way NASA/JPL present Mars rover stereo imagery.

## Status

Planning stage. There is no code yet. The repository holds the project plan, background research, and one sample image.

Phase 1, a reference pipeline built on public-domain Keystone View Co. stereographs, can start now. This is a Truman Library project, limited to Truman Library collections, with Library staff doing the scanning. Depth products are deferred until the pilot scans are in. See [Decisions](PLAN_HSTL-Stereoscope-Images.md#decisions).

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
| [PLAN_HSTL-Stereoscope-Images.md](PLAN_HSTL-Stereoscope-Images.md) | Project plan: sources, feasibility test, data model, phases, risks and decisions |
| [VIEWING_MODES.md](VIEWING_MODES.md) | How each viewing mode works and how to build it: wiggle, anaglyph, cross-eyed, parallel and headset |
| [Questions-for-planning.txt](Questions-for-planning.txt) | Open questions and items not yet verified |
| [holograms_from_stereoscopic_images.md](holograms_from_stereoscopic_images.md) | Background: creating holograms and light field displays from stereo pairs |
| [jpl_mars_rover_3d_image_processing.md](jpl_mars_rover_3d_image_processing.md) | Background: JPL's rover stereo pipeline, with links to software, data archives and papers |
| [Images/](Images) | Sample image `2008-175.jpg`, the public copy of one eye of a Truman stereo card |

---

*Last updated: 2026-11-05-0703*
