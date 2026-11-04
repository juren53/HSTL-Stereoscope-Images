# HSTL Stereoscopic Images: Project Plan

Draft, 2026-11-04. Goal: process historic stereo photographs of Harry S. Truman and his contemporaries, and present them to the public the way NASA/JPL present Mars rover stereo imagery.

## Summary

The JPL model transfers well, with two changes:

1. **Getting the source images is the main problem, not the processing.** The online copies of the Truman stereo portraits show only one eye. Nothing can be done with those until both halves are scanned.
2. **JPL's metric 3D does not transfer.** Rover cameras are calibrated, and these were not. Anaglyphs, stereo pairs and viewers are fully achievable. Measured terrain models are not, so depth products must be labeled as approximate reconstructions.

Work can start now on public-domain stereographs that already have both halves online. Scanning and permissions can proceed alongside that work.

## What exists today

| Source | What it is | Status for this project |
|---|---|---|
| Truman Library slide collection | 2,700+ slides, "primarily" 35mm and 35mm stereo. Mostly Library exhibits and events, and mostly post-presidential. Also Truman travel and Margaret's early-1950s Europe trips. | **The stereo count is unknown.** The Library's page does not separate stereo from plain 35mm. |
| `2008-175`, `2008-176` | Stereo cards (3.5x7 in) of Truman and his sister Mary Jane at a White House mantel, made by Lowell Mason. Truman's handwritten note to Margaret survives with them. | **Best candidates, but only one half is online.** See below. |
| `67-4210`, `67-4223`, `67-4227` and likely others in the `67-42xx` series | Keystone View Co. WWI stereographs, 6x3 in, donated to Truman. | **Public domain, both halves online.** Ideal for development, but they date from WWI, not the 1950s. |
| Robert B. Landry Papers | Stereo slide of the White House, ca. 1955. | Worth checking. Found only in a search summary, so not yet confirmed against the record. |
| Eisenhower Library stereo slides | 1,154 slides, mostly 1948-1958, taken by Eisenhower and staff on a Realist camera. Covers the 1952 campaign, 1953 inauguration, 1953 Bermuda Conference and George VI's funeral. | **Strongest fit for "and others."** Different institution. Online availability and rights not established. |

Findings from inspecting the records:

- **`2008-175` and `2008-176` are not a left/right pair.** They look like a natural pair but show different poses: in one the subjects face the camera, in the other they face each other. Each is one eye of a separate card.
- **The rest of the card exists.** `2008-175` shows a sliver of the adjacent half at its right edge, and the record lists 3.5x7 in. The public JPEG was evidently cropped to one half.
- **Public copies are about 800 px.** That matches the 800x800 derivative step in the HST-Metadata Phase 1 workflow, so those files are not suitable as sources. Whether a master TIFF holds the whole card is the first thing to find out.
- **`Stereoscope` is already an HST Keyword.** It can drive an inventory query against the photo database.
- **The Truman items may not all be 1950s.** The Mason cards are undated, and show Truman as president, so they fall in 1945 to January 1953.

No stereo material turned up in the local `HST-Metadata` project.

## Where the rover analogy holds

| JPL stage | Historic equivalent | Transfers? |
|---|---|---|
| Telemetry unpacked to raw records | Scan the physical object | Yes, as the master scan |
| Radiometric correction, flat field | Density and color correction, matched between eyes | Yes. Fading differs by film stock. |
| CAHVOR calibration from lab measurements | None. Camera often unknown. | **No.** Realist specs are only approximate (35mm lenses, about 70mm separation). |
| Epipolar rectification | Feature-based alignment from the image itself | Yes, and tested below |
| Anaglyphs, stereo pairs | Same | Yes, fully |
| Disparity to metric range and DEM | Relative depth only | **Partly.** Must be labeled as approximate. |
| Access Mars 3D environments | Those were built from many overlapping views | **Mostly no.** One pair gives a shallow relief from one viewpoint, not a walkable room. |
| Raw gallery and captioned releases | Item pages from Library metadata | Yes, and this is the most valuable idea to borrow |

**Tooling change from the source docs:** Ames Stereo Pipeline and ISIS are aimed at planetary mapping and are the wrong fit here. Rover products came from JPL's MIPL team using VICAR, which requires CAHVOR-calibrated input that we do not have. Use OpenCV instead, with StereoPhoto Maker / StereoAutoAlign as a free comparison baseline.

## Feasibility test

A throwaway test ran on `67-4227` at public resolution (800x410) with OpenCV SIFT matching and a row-alignment fit.

| Measure | Result |
|---|---|
| Feature matches | 667, of which 654 were inliers |
| Vertical error before alignment | median 0.98 px |
| Vertical error after alignment | 0.33 px RMS |
| Rotation | about 0.03 degrees |
| Usable depth range | about 8 px across a 397 px wide eye (about 2%) |
| Left/right order check | correct (correlation 0.97) |
| Dense disparity coverage | 65% valid |

What this shows:

- **Alignment and anaglyph generation work automatically** on this card. It is one card, so this is not yet a general result.
- **Resolution is the limit.** Eight pixels of depth is too coarse for good depth maps, which is another reason to get masters.
- **Dense depth is unreliable with a classic matcher.** It was smooth on the textured ground and mostly noise in the sky and horizon. Depth products need masking and probably a neural matcher.
- **Scratches and dust appear in only one eye.** They show as colored specks in the anaglyph. That makes them detectable automatically, because a real feature appears in both eyes.
- **The left/right check only suits landscapes.** It assumes lower in the frame means nearer, so interiors and portraits need a person to check them.

## Data model

Borrow the PDS idea of numbered product levels, where each level is reproducible from the one below and nothing overwrites a master.

| Level | Product | Notes |
|---|---|---|
| L0 | Master scan of the **whole object**, front and back | Archival TIFF with checksum. Keeps mount labels and handwriting. |
| L1 | Left and right eye crops | Crop coordinates recorded. L/R order verified. |
| L2 | Aligned pair | Transform stored in a sidecar file, so it can be re-derived. |
| L3 | Public products | Anaglyph, side-by-side, wiggle GIF/video, MPO, spatial photo |
| L4 | Depth products | Disparity map and relief mesh, labeled as reconstructions |

File IDs follow the accession number, such as `2008-175_L2_left.tif`. A stereo pair is one catalog item with one record, and metadata comes from the existing CSV and ExifTool workflow in HST-Metadata.

**Restoration rules:** cleaning happens at L2 or above and is recorded. No AI inpainting of faces, no colorizing, and no synthesized second eye presented as an original stereo photograph.

## Phases

### Phase 0: Inventory and permission (no code)

- Query the photo database for `Stereoscope` and list the stereo slides.
- Establish for each item: format, date, photographer, rights, and whether a whole-object master exists.
- Confirm the project's standing with the Library, and contact the Eisenhower Library if it is in scope.
- Choose a pilot set of 10 to 12 items that mixes cards and slides.

**Exit:** a stereo inventory CSV, a chosen pilot set, and agreement on who may publish what.

### Phase 1: Reference pipeline (can start now)

- Build a Python package covering L1 to L3, run on the public-domain Keystone cards.
- Add gutter detection, alignment, stereo window placement and the export formats.
- Generate a QC report per item: vertical error, depth range, L/R check, one-eye defect map.
- Add a human QC step, since the automated checks do not cover everything.
- Compare results against StereoAutoAlign.

Proposed starting thresholds, to be tuned: vertical residual under 0.1% of eye width, and total depth range under about 3%.

**Exit:** the whole Keystone batch runs from one command, and every item passes or is flagged for review.

### Phase 2: Master scans of the pilot set

- Scan both halves in a single capture, which preserves their relative geometry.
- Scan 3.5x7 cards on a flatbed or copy stand.
- Realist mounts are about 41x101 mm and will likely not fit a 35mm film scanner holder, so use a flatbed with transparency support or a camera copy stand.
- Avoid vendors' default stereo service: at least one commercial service states that it scans only one side of each stereo slide, which would repeat the `2008-175` problem.
- Decide with the archivist whether chips stay in their mounts.
- Take the specifications from the FADGI Third Edition transparency tables. Forum figures of 3000-4000 ppi for 35mm are unconfirmed. At 4000 ppi a 24mm frame would be about 3,800 px per eye.
- Scan color and grayscale targets with each batch.

**Exit:** pilot masters in hand, processed through L3, with depth range and alignment compared against the 800 px results.

### Phase 3: Public viewer prototype

Follow the JPL pattern of a raw gallery plus captioned feature pages:

- **Item page:** like a NASA Photojournal entry, with a story caption, credit, accession number, rights, and downloads at each level.
- **Viewing mode switch:** wiggle (the default, as it needs no glasses), anaglyph, cross-eyed, parallel, and headset. See [VIEWING_MODES.md](VIEWING_MODES.md).
- **Raw gallery:** unprocessed L1 pairs for enthusiasts, as the rover raw image pages do.
- **Feature stories:** the note from Truman to Margaret makes the Mason cards an obvious first feature.

Build a static site, with deep zoom through OpenSeadragon or IIIF if needed. NYPL's Stereogranimator is the precedent: it turned 40,000 stereographs into GIFs and anaglyphs, and is an open-source project to review before building anything similar. WebXR and Apple spatial photo output still need testing. Conversion tools exist, but I have not validated either path.

**Exit:** the pilot set is viewable on a phone, a desktop, and one headset, and reviewed by Library staff.

### Phase 4: Scale and publish

- Run the full inventory through the pipeline.
- Publish wherever Phase 0 settles. This could be the Library's site, the NARA catalog, or both.
- Add exhibit options: printed anaglyphs with paper glasses, or a period Realist viewer display.

### Phase 5: Depth products (optional)

- Test FoundationStereo or similar on masters, against the classic matcher.
- Produce shallow relief scenes for selected images, labeled as reconstructions.
- This needs an NVIDIA GPU. None was detected on this machine, so plan for slow CPU runs or another machine.
- Check licenses. NVIDIA's NGC card lists the NVIDIA Open Model License, while the GitHub code license is unconfirmed.

## Risks

| Risk | Consequence | Response |
|---|---|---|
| Few 1950s Truman stereo images exist | Thin core content | Phase 0 answers this early. Broaden to Eisenhower or the WWI series. |
| Second halves are missing or masters are cropped | Rescans needed | Check `2008-175` first. |
| Rights unclear | Items cannot be published | The Mason cards read "unaware of any copyright claims; use at your own risk." Donated slides may carry restrictions. Check per item. |
| Sequential exposures on non-Realist cameras | Subjects moved between eyes, so ghosting | QC flags it. Keep such items as 2D. |
| Faded or mismatched color film | Eyes disagree, causing eye strain | Match eyes at L2. Use a color-optimized anaglyph method. |
| Depth output read as measurement | Misleads the public | Label L4 clearly. Do not publish scale claims. |

## Decisions needed

1. **Scope:** Truman Library only, or also Eisenhower and others? This sets the size of the project more than anything else.
2. **Standing:** is this a Library project, like HST-Metadata, or independent? That determines scan access, hosting and branding.
3. **Scanning:** who scans, and on what equipment?
4. **Depth products:** in scope, or a later stretch goal? I recommend deferring them until Phase 2 results are in.

## Notes on the source docs

- `jpl_mars_rover_3d_image_processing.md` and `-1.md` are near-duplicates. `-1` adds the VR section and more links, while the other adds CAHV model names and the literature list. They could be merged.
- Both credit ASP and ISIS for rover work without mentioning VICAR/MIPL, which is what the Pathfinder and MER sources describe.

## Sources

- [Truman Library: Audiovisual Materials Collection](https://www.trumanlibrary.gov/library/audiovisual-materials-collection)
- Truman records: [2008-175](https://www.trumanlibrary.gov/photograph-records/2008-175), [2008-176](https://trumanlibrary.gov/photograph-records/2008-176), [67-4210](https://trumanlibrary.gov/photograph-records/67-4210), [67-4223](https://trumanlibrary.gov/photograph-records/67-4223), [67-4227](https://trumanlibrary.gov/photograph-records/67-4227)
- [Eisenhower Library: Stereo Slide Collection](https://www.eisenhowerlibrary.gov/index%2Ephp/node/1318)
- [VICAR help: MARSJPLSTEREO](https://nasa-ammos.github.io/VICAR-DOCS/vicar/dev/html/vichelp/marsjplstereo.html)
- [Mars Pathfinder Rover Camera MIDR dataset](https://pds.nasa.gov/data/mpfr-m-apxs-2-edr-v1.0/mprv_0001/document/rvcmdrds.htm)
- [MIPL and MER imaging](https://scienceblog.com/imaging-masterminds-give-human-scale-view-of-mars)
- [StereoPhoto Maker](https://stereo.jpn.org/eng/stphmkr), [StereoAutoAlign](https://stereo.jpn.org/stereoautoalign/index_wine.html)
- [Stereo Realist](https://en.wikipedia.org/wiki/Stereo_Realist), [Realist 3.5 specifications](https://stereoscopy.com/cameras/realist-35.html), [stereo mount formats](https://rmm3d.com/3d.encyclopedia/formats/formats.html)
- [Commercial stereo slide scanning (one image only)](https://everpresent.com/understanding-photo-formats-stereo-slides/)
- [Library of Congress on FADGI Third Edition](https://blogs.loc.gov/thesignal/2023/05/fadgi-third-edition-still-image/)
- [NYPL Stereogranimator, Library Journal](https://www.libraryjournal.com/story/nypl-animator-recreates-19th-century-stereograph-effect), [2024 follow-up, PetaPixel](https://petapixel.com/2024/05/08/these-animated-stereograms-bring-19th-century-photos-to-life/)
- [FoundationStereo](https://github.com/NVlabs/FoundationStereo), [NGC model card](https://catalog.ngc.nvidia.com/orgs/nvidia/tao/models/foundationstereo)
