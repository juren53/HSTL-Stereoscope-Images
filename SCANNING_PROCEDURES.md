# Scanning Procedures for Stereo Slides

*Last updated: 2026-11-05-0710*

Recommended process for scanning the Realist-format stereo slides and stereo cards. It supports Phase 2 of [PLAN_HSTL-Stereoscope-Images.md](PLAN_HSTL-Stereoscope-Images.md).

**Recommendation:** use a camera on a copy stand, capture the whole mount with both eyes in one shot, and meet at least FADGI 3-star.

The numbers below come from the [FADGI Technical Guidelines, Third Edition (2023)](https://zenodo.org/records/7929962). FADGI does not cover stereo slides, so the stereo-specific steps are recommendations to test in the pilot.

## Target specification

| | 3-star (recommended minimum) | 4-star |
|---|---|---|
| **Resolution** | 3000 ppi | 4000 ppi |
| **Bit depth** | 16-bit | 16-bit |
| **Color space** | Adobe RGB (1998), ProPhoto or eciRGB v2 | Same |
| **Format** | TIFF or JPEG 2000 | Same |
| **Whole mount (41x101 mm)** | About 58 MP, 350 MB uncompressed | About 103 MP, 615 MB uncompressed |
| **Each 24 mm eye** | About 2,800 px | About 3,800 px |

Even the 3-star level gives over five times the roughly 500 px per eye in the test slides in `Images/`. To reach 4-star, keep the single whole-mount shot and add a separate capture of each eye without moving the slide.

## Equipment

- **Capture:** FADGI accepts film scanners, planetary scanners, digital cameras and flatbeds, but not drum scanners. A camera avoids the mount-holder problem, and it is the only option for cards.
- **Cards:** FADGI says stereographs cannot be scanned on a flatbed because they curve. Use the copy stand, with lighting set up to avoid shadows and reflections.
- **Lens:** use a flat-field apochromatic lens made for close focusing, typically one to two stops down from maximum aperture.
- **Light:** use a 5000K light box in a dim room to limit flare, and mask the light around the slide.

## Procedure

1. **Prepare:** record each item's condition and mount type, and sort by mount. Glass-mounted slides may need their own setup.
2. **Handle:** wash and dry your hands, and do not use hand sanitizer. Remove loose dust with a soft, lint-free brush, and ask a conservator about gloves. If there is any risk to an original, stop and get help.
3. **Keep mounts intact:** do not remove chips or open mounts unless the archivist approves it.
4. **Calibrate each batch:** capture a transmissive target and check results with OpenDICE or similar software.
5. **Capture the transmitted image:** get the whole mount with both eyes, without moving the slide between eyes, so their relative geometry is preserved.
6. **Capture the mount:** photograph the front and back in reflected light to record labels and handwriting. FADGI says to scan the entire mount when it carries important information.
7. **Record orientation:** note which side faced the camera and which eye is on which side, because a wrong assumption swaps or mirrors the pair.
8. **Keep masters unaltered:** no sharpening beyond the FADGI limit, no auto-correct, and no dust removal. FADGI bans dust removal on masters, and the pipeline needs that dust to detect one-eye defects.
9. **Save and check:** apply checksums and the `2008-175_L0_...` naming, then inspect every item for focus, clipping and both eyes before re-housing.

## Likely problems

- **Kodachrome color:** FADGI warns that an Ektachrome IT-8 target cannot profile Kodachrome correctly. Kodachrome targets are rare, so a professionally made Kodachrome ICC profile may be the best solution. Identify the film type during preparation.
- **Glass mounts:** these can show Newton's rings and reflections, so run a test on them first.
- **Tight frames:** Realist frames can nearly touch or overlap on the film. Capture the full area and do not crop to the mount opening.
- **Fading:** match the transparency as it is now. Any correction between eyes belongs at L2, not in the master.

## Pilot

Try 5 to 10 items covering card mounts, glass mounts, Kodachrome, and one item with handwriting. Run them through alignment, then compare depth range and vertical error against the 800 px results before setting the final spec.

## Sources

- [FADGI Technical Guidelines, Third Edition (2023)](https://zenodo.org/records/7929962): sections 3.9 and 2.6 and the workflow notes
- [Stereo mount formats](https://rmm3d.com/3d.encyclopedia/formats/formats.html)
- [Realist frame overlap, cutting guide](https://rmm3d.com/3d.encyclopedia/mounting/section3.html)

---

*Last updated: 2026-11-05-0710*
