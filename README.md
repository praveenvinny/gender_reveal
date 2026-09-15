# gender_reveal

A one-page gender reveal: a white cake that says *"It's a.."*, a knife you drag
through it, blue buttercream flowing out of the cut, and the answer.

## Opening it

Double-click `index.html`, or serve the folder:

```sh
python3 -m http.server 8000
# then open http://localhost:8000
```

Works on phones and desktops. On a phone, swipe the knife; on a desktop, drag it
with the mouse. A "tap here to cut" link appears after 8 seconds for anyone who
doesn't find the drag, and "Do it again" resets everything — handy if you're
passing a phone around a room.

## Files

- `index.html` — the whole thing. No build step, no dependencies.
- `cake.png` — the photograph the animation is built on.

## How it works

The page is one SVG whose `viewBox` is the photograph's own coordinate space,
cropped in on the cake (centre `508,288`, radius ~`214`). Everything animated is
positioned in those coordinates, so it stays glued to the cake at any screen
size. `preserveAspectRatio="slice"` crops the sides on narrow screens, which is
why the cake grows on a phone instead of shrinking.

- **The cut** runs from the cake's centre to its bottom edge. Dragging reveals a
  seam progressively, tracking the pointer — stop halfway and the cut is halfway.
- **The opening** rotates the two halves a couple of degrees about the cake's
  centre, so the cut yawns open at the rim and stays shut at the pivot, like a
  slice being eased out. Only the cake rotates; the tabletop underneath is a
  separate static copy of the photo, so nothing shows through.
- **The cream** is a few hundred circles melted together by an SVG gooey filter
  (`feGaussianBlur` + a high-contrast `feColorMatrix`), growing on staggered
  delays so it spreads outward from the cut over about a second.

### Things worth knowing before you edit

- `#knife` carries its position as a `transform` **attribute**. Presentation
  attributes lose to CSS rules, so a CSS `transform` on `#knife` silently
  overrides the cut position — the idle bob lives on `#knifeBob` for that reason.
- Cream blobs smaller than ~`r=11` vanish: the gooey filter's alpha threshold
  wipes them out once blurred.
- The headroom above the cake is fake. The photo has almost none, so the top two
  rows are stretched upward to extend the tabletop seamlessly.

## Changing it

- **Names / captions** — the `.kicker` and `.tagline` in the markup.
- **"It's a boy.."** — the `.reveal` div. It's gold to match the cake's own
  lettering; for blue, swap the gradient in `.reveal` for the cream blues.
- **A different cake photo** — replace `cake.png` and update the cake's centre
  and radius (the `clipCake` / `clipCakeWide` circles, the `viewBox`, and
  `CUT_START` / `CUT_END` in the script).
