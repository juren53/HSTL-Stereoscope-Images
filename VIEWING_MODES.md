# Viewing Modes

*Last updated: 2026-11-04-1527*

Detail for the viewing mode switch in Phase 3 of [PLAN_HSTL-Stereoscope-Images.md](PLAN_HSTL-Stereoscope-Images.md). It covers how each mode produces depth, what the viewer has to do, and how to build it.

## The modes at a glance

| Mode | Needs | True stereo? | Best for | Main weakness |
|---|---|---|---|---|
| Wiggle | Nothing | No, motion only | First impression, sharing | Constant movement, limited depth |
| Anaglyph | Red/cyan glasses | Yes | Classrooms, exhibits, print | Poor color, some ghosting |
| Cross-eyed | Practice | Yes | Enthusiasts, any screen size | Eye strain, a learned skill |
| Parallel | Practice or a small viewer | Yes | Enthusiasts, closest to a period stereoscope | Image must stay small |
| Headset | VR headset | Yes | Best overall quality | Few visitors own one |

## How stereo depth works

Your eyes sit about 65 mm apart, so each sees a scene from a slightly different position. A near object shifts more between the two views than a far one. That shift is called **parallax**, and the brain turns it into depth.

A stereo camera records the same two views. Every true stereo mode does one job: **deliver the left image only to the left eye and the right image only to the right eye.** The modes differ only in how they keep the two images separate. Wiggle is the exception, as described below.

### What all modes share

All five modes draw on the same aligned L2 pair, so an item is processed once. Each item also carries three stored values:

- **Stereo window offset:** the horizontal shift between the eyes that decides what appears at the screen surface. Nearest objects sit at the surface and everything else falls behind it, as if seen through a window. Objects that poke through the frame edge are uncomfortable, so this is the default.
- **Pivot point:** the spot that stays still in wiggle mode, usually the subject's face.
- **Usable crop:** the area both eyes share. Material visible to only one eye, such as a frame edge, is trimmed.

## Wiggle

```
 time ->   [ L ]  [ R ]  [ L ]  [ R ]  [ L ] ...
           both eyes see the same frame
```

**How it works.** The left and right images alternate in one spot, about 5 to 8 times per second. Near and far objects shift by different amounts between frames, and the brain reads that relative motion as depth, much as it does when you move your head from side to side.

This is motion parallax, not binocular stereo. Both eyes see the same thing, so it also works with one eye and for people who cannot fuse stereo images.

**What the viewer does.** Nothing except look. This is why it is the default: the depth is visible at once, with no equipment or instructions.

**The pivot point matters most.** Whatever has zero shift between the frames appears still, and everything else rocks around it. A pivot on the subject's face holds the person steady while the room moves behind them. A poorly chosen pivot makes the whole picture jitter.

**Build notes**
- Stack two images and swap visibility, or draw to a canvas. Start at about 150 ms per frame and tune by eye.
- Offer a pivot default per item, and let users click to move it. The Stereogranimator lets users choose the focal point and speed.
- Exact alignment and matched brightness are critical here. Any mismatch shows up as flicker, and one-eye dust specks will blink, so use cleaned L2 images.
- Export as MP4 or animated WebP as well as GIF. GIF is easy to share, but its 256-color limit hurts color slides.

**Limits.** The movement can distract from the content and may be unpleasant for some viewers. See [Accessibility](#accessibility).

## Anaglyph

```
 left image  -> red channel    ┐
                               ├─> one combined image
 right image -> green + blue   ┘

 red lens (left eye):   passes the red channel,  so it sees the left image
 cyan lens (right eye): passes green and blue,   so it sees the right image
```

**How it works.** Both images are printed on top of each other in different colors. Colored filters then give each eye only its own image. This is the JPL rover method, and the red lens goes on the left eye by convention.

**What the viewer does.** Wears red/cyan glasses and looks at the picture normally.

**Black and white versus color.** Most of the difference comes down to the source:

| Source | Method | Result |
|---|---|---|
| Black and white, such as the Keystone and Mason cards | Left to red, right to green and blue | Clean, close to ideal |
| Color slide (full color) | Red channel from left, green and blue from right | Keeps some color, but saturated reds flicker between the eyes (retinal rivalry) |
| Color slide (Dubois) | Least-squares color matrices | Best compromise for color, as it reduces rivalry and ghosting |
| Color slide (gray) | Convert to grayscale first | Most comfortable, but the color is lost |

A saturated red tie or flag is bright through one lens and dark through the other, which is what causes the rivalry. Offer the Dubois method for color slides, and fall back to gray where it is still uncomfortable.

**Build notes**
- The core operation is a single channel merge, and the feasibility test on `67-4227` used it:

  ```python
  anaglyph = np.dstack([right, right, left])  # OpenCV order is BGR
  ```

- Render in the browser with canvas or WebGL for the live viewer, and pre-render files at L3 for download and print.
- **Save as PNG, or JPEG without chroma subsampling.** Standard JPEG blurs color detail, and color detail is the depth information in an anaglyph.
- Do not place a red-tinted frame or colored background around the image, as it confuses the filters.

**Limits.** Ghosting appears when a screen's colors leak through the wrong lens, and it varies by display. High-contrast edges are the worst case. Printed anaglyphs need ink and glasses tested together, because print colors differ from screen colors.

## Cross-eyed

```
   [ RIGHT image ]   [ LEFT image ]
            \             /
             \           /
              \         /
               \       /
                \     /
                 \   /
                  \ /
                  / \      eyes cross at a point in front of the screen
                 /   \
            left eye  right eye
```

**How it works.** The images sit side by side with **the right-eye image on the left**. The viewer crosses their eyes so the left eye looks at the right-hand image and the right eye looks at the left-hand image. Each eye therefore lands on its correct view.

**What the viewer does.**
1. Hold a fingertip midway between the eyes and the screen, then look at the fingertip.
2. Notice the screen behind it now shows three blurry images.
3. Adjust the finger's distance until the middle image is solid, then hold that eye position and let it come into focus.

Eye convergence and focus normally work together, and this skill separates them. Some people get it in seconds, while others never do.

**Build notes**
- Show a small alignment mark above each image. When the viewer sees the marks merge into one, the images are lined up.
- Add a size slider, as a smaller image is easier to start with.
- A wrong left/right order reverses the depth, so near appears far. Add a one-click swap for testing, and keep it out of the public interface.

**Limits.** Cross-eyed viewing works at any image size, which is its advantage over parallel. It tires the eyes quickly, and it does not suit everyone.

## Parallel

```
   [ LEFT image ]   [ RIGHT image ]
         |                |
         |                |     eyes look straight ahead,
         |                |     as if at something far away
     left eye         right eye
```

**How it works.** The left image sits on the left, the way the original stereo cards are laid out. The viewer relaxes their eyes as if looking through the screen, so each eye looks straight ahead at its own image. A Holmes stereoscope or Realist viewer does the same thing with lenses, which is why this mode is closest to how these images were first seen.

**What the viewer does.** Either of these:
- **Free viewing:** look through the screen into the distance, then let the two images drift together until a third, solid one appears in the middle.
- **With a viewer:** use a small lens stereoscope held to the screen, or a phone in a cardboard-style holder. This is far easier.

**The size limit.** Eyes cannot comfortably turn outward. The centers of the two images must therefore be no further apart than the viewer's eyes, about 65 mm. On a desktop monitor that makes the picture quite small.

**Build notes**
- Browsers do not reliably know the physical size of a screen. Provide a size slider with an on-screen calibration: hold a credit card to the screen and match a box to it, then calculate 65 mm from that.
- Add a full-screen side-by-side option for phone holders.
- Show the same alignment marks as in cross-eyed mode.

**Limits.** The picture is small without a viewer, and free viewing is a difficult skill for many people.

## Headset

```
   headset display
   ┌─────────────┬─────────────┐
   │  left eye   │  right eye  │    each eye has its own screen or
   │  sees L     │  sees R     │    region, so nothing can leak across
   └─────────────┴─────────────┘
```

**How it works.** A headset gives each eye a separate image, so there is no color loss, no ghosting and no eye training. The photo appears as a floating print or window in front of the viewer. Unlike a rover mesh, the viewer cannot walk around inside it, because the depth is fixed from one camera position.

**Two delivery routes.** Build the first and offer the second as a download.

1. **In the browser (WebXR).** The visitor presses an "Enter VR" button on the item page. Nothing needs to be installed.
2. **File download.** The visitor saves a side-by-side file or Apple spatial photo and opens it in the headset's own gallery. This is simpler and may look better on Vision Pro.

**Build notes**
- In Three.js, the left eye camera sees layers 0 and 1 and the right eye sees layers 0 and 2. Put one flat mesh with the left image on layer 1, and a second with the right image on layer 2, in the same position.
- The WebXR Layers API has a `stereo-left-right` layout for content that is already stereo, which is cleaner. It is still a W3C working draft, so treat it as an upgrade rather than a dependency.
- Show the Enter VR button only when `navigator.xr.isSessionSupported('immersive-vr')` confirms support.

**Natural scale.** A Realist's 35mm lens across a 24mm frame covers a horizontal angle of about 38 degrees. If the headset shows the print at that same angle, for example about 1.37 m wide at 2 m distance, the scene appears close to life size. Lens separation of about 70 mm is near human eye spacing, which helps too. This applies only to Realist images. Other cameras are unknown, so use a comfortable default size for those.

**Device support**

| Device | WebXR status | Note |
|---|---|---|
| Meta Quest Browser | Supported | Secondary sources. Primary test device. |
| Vision Pro (Safari) | `immersive-vr` supported | Secondary sources. A developer forum reports problems with a stereo video sample, so test it directly. |
| iPhone, iPad, Mac Safari | Not supported | Reported by one 2026 source. These users get the other four modes. |

**Limits.** Few visitors own a headset. Low-resolution scans will look soft at this size, which makes this the mode that most needs the master scans. Exhibit use also needs staff, cleaning and supervision.

## The switch itself

**Behavior**
- Open in wiggle, unless reduced motion is requested.
- Show all modes in one control. Hide only the headset mode where it is not supported.
- Remember the visitor's choice across items.
- Put the mode in the URL, such as `?mode=anaglyph`, so a teacher can share a link that opens in anaglyph.
- Keep zoom and position when switching modes.
- Add a short "How to view" panel for each mode, with the steps above.
- Always offer a plain 2D view of one eye. Some visitors want the image and caption without any effect, and that is also the fallback for failures.

**Data per item**

```json
{
  "id": "67-4227",
  "left": "67-4227_L2_left.jpg",
  "right": "67-4227_L2_right.jpg",
  "window_offset_px": -12,
  "pivot": [0.62, 0.55],
  "crop": [14, 12, 369, 386],
  "color": false,
  "camera": "unknown"
}
```

The values here are illustrative. `window_offset_px` and the crop come from the feasibility test, and `pivot` is a made-up example.

**Zoom.** Both eyes must zoom and pan together. If deep zoom is used, drive both images from one shared viewport so they cannot fall out of step.

## Accessibility

- **Motion.** Automatic wiggle must have a visible pause control. When the browser reports `prefers-reduced-motion`, open in the plain 2D view and let the visitor choose a mode.
- **Flashing.** Keep the swap rate moderate. Wiggle changes only small areas, but check high-contrast images against the WCAG flash limit of three per second.
- **Stereo blindness.** Some people cannot perceive stereo depth at all, and they lose nothing if the page works without it. Wiggle helps here, as it does not need two eyes.
- **Screen readers.** Alt text and captions should describe the photograph. They should not depend on the depth effect.
- **Eye comfort.** Add a short note: stop if your eyes feel strained. Do not ask children to practice cross-eyed viewing for long periods.

## Display size and exhibits

Parallax scales with the picture. In the test card, depth spans about 2% of the eye width. If the picture is shown larger than about 3.25 m wide, the farthest objects would be more than 65 mm apart on the screen, and the viewer's eyes would have to turn outward. Each item's depth range should therefore set a maximum display width for projection and large prints.

## Test checklist

Before Phase 3 exits, try each of these with a black-and-white card and a color slide:

- [ ] Wiggle holds steady on the subject, with no flicker
- [ ] Anaglyph shows correct depth with real glasses on a phone, a laptop and a print
- [ ] Depth direction is correct in cross-eyed and parallel, not reversed
- [ ] Parallel size calibration works on at least two screens
- [ ] WebXR works on a Quest and on Vision Pro, if one is available
- [ ] Reduced motion setting opens the 2D view
- [ ] Zoom stays in step between the eyes
- [ ] Staff who have never used the viewer can reach depth in each mode

## Prior work to review

- **[SLV LAB Stereoscope Viewer](https://slvlab-prod.coolify.slv.vic.gov.au/resources/stereoscope-viewer-prototype)**: State Library Victoria's prototype produces anaglyphs and wiggle from vintage stereographs using IIIF and Three.js. It is the closest match to this project. Its [code](https://github.com/StateLibraryVictoria-SLVLAB/stereograph-tool) is on GitHub, but the page states no license, so check that before reuse.
- **[NYPL Stereogranimator](https://www.libraryjournal.com/story/nypl-animator-recreates-19th-century-stereograph-effect)**: wiggle and anaglyph creation with user-chosen focal points, from 2012.

## Sources

Timing, sizing and scale figures not linked here are starting values or calculations of mine, and need testing.

- [Dubois anaglyph method](https://www.site.uottawa.ca/~edubois/anaglyph)
- [WebXR Layers API Level 1 (W3C working draft)](https://www.w3.org/TR/2025/WD-webxrlayers-1-20251203/)
- [A-Frame layers component](https://awesome.ecosyste.ms/projects/github.com%2Fbryik%2Faframe-layers-component), per-eye layer behavior
- [WebXR on visionOS (Brown VR wiki)](https://www.vrwiki.cs.brown.edu/hardware/vr-hardware/apple-vision-pro/development-approaches-for-visionos/webxr-on-visionos), [Apple developer forums: WebXR](https://developers.apple.com/forums/tags/webxr), [WebXR browser support overview](https://www.issoh.co.jp/tech/details/7076/)
- [Realist 3.5 specifications](https://stereoscopy.com/cameras/realist-35.html)

---

*Last updated: 2026-11-04-1527*
