# Homework Portfolio

This repository is the home page and container for four homework projects. The
landing page is intentionally small: it introduces the collection and links to
four separate folders. Each folder is a standalone web app, with unfinished
assignments kept as placeholders until their branch is ready.

## What is here

The project is a static site, so there is no build step or framework dependency.

| Path | Purpose |
| --- | --- |
| [`index.html`](index.html) | Portfolio landing page and assignment directory |
| [`styles.css`](styles.css) | Landing page layout, responsive rules, and theme tokens |
| [`assignment-shared.css`](assignment-shared.css) | Shared baseline styles for standalone assignment pages |
| `assignment-01/` | Homework 01: interactive Sol LeWitt wall drawings |
| `assignment-02/` | Homework 02: smooth Pollock gesture sketch |
| `assignment-03/` | Homework 03 standalone app placeholder |
| `assignment-04/` | Homework 04 standalone app placeholder |

Each assignment folder contains its own `index.html` entry point. The placeholder
pages use a local `styles.css` entry point that imports the shared baseline;
Homework 01 and Homework 02 are self-contained p5.js pages with inline styles and
scripts so their canvas experiences remain together.

## Run it locally

You can open `index.html` directly in a browser, but a tiny local server gives
the same relative-link behavior as a hosted static site. From the repository
root, run:

```bash
cd /Users/liuboto/dev/homework
python3 -m http.server 8000
```

Then visit <http://localhost:8000>. The four links on the landing page should
open `/assignment-01/` through `/assignment-04/`, and each assignment page
should offer a return link to the portfolio (Homework 01 and Homework 02 label it
`Homework portfolio`; the placeholders label it `Back home`).

There is no package manager setup or compilation step. Any static server that
serves the repository root will work.

## Homework 01: Sol LeWitt wall drawings

Homework 01 is a single-file p5.js study of Sol LeWitt's instruction-based wall
drawings. It includes five drawing systems (Wall Drawings 118, 17, 46, 122, and
273), parameter controls, procedural regeneration, pointer interactions, and PNG
export. The assignment keeps its own canvas and inspector layout inside
`assignment-01/index.html`, while the small header link returns to the portfolio.

The drawing engine loads p5.js 1.11.3 from cdnjs, so the first load needs network
access to that CDN. Once p5.js is available, the app has no build step and does
not send drawing data anywhere.

The assignment follows the portfolio's shared theme preference: it starts in
dark mode, offers a light-mode control, and stores the choice in the same
`homework-theme` local-storage key. The artwork canvas remains a paper-like white
surface so the generated line work stays legible in either surrounding theme.

## Homework 02: Smooth Pollock Gesture

Homework 02 is a full-window p5.js gesture sketch. Move the pointer to lay down
smooth tangent-matched curves; pressing starts a new randomized brush personality,
and Space clears the canvas. The sketch varies line width, color, and splatter
behavior to keep each gesture responsive and painterly.

The drawing engine loads p5.js 1.11.3 from jsDelivr and keeps the gesture entirely
in the browser. The small overlay link returns to the portfolio while leaving the
canvas full-screen.

## Theme behavior

The portfolio defaults to a dark theme inspired by the Scyllabus workbench:
near-black surfaces, light ink, hairline rules, and one muted warm accent. The
`Light mode` control in the header switches to the light palette. The choice is
stored in `localStorage` under `homework-theme`, so it persists between visits.

Assignment pages include the same dark-first palette and a theme control so they
remain usable when opened directly. If browser storage is unavailable, the site
keeps the dark default and the control still works for the current page.

The two palettes intentionally use very few colors:

| Role | Dark | Light |
| --- | --- | --- |
| Page background | `#131313` | `#f7f7f6` |
| Surface | `#1a1a1a` | `#ffffff` |
| Main ink | `#e9e9ec` | `#17171a` |
| Secondary text | `#83838e` | `#82828c` |
| Hairline | `#2b2b2b` | `#e3e3e0` |
| Accent | `#dd985f` | `#7c6450` |

## Replacing a placeholder

When an assignment is ready:

1. Keep the folder name and its `index.html` entry point so the landing-page
   link remains stable.
2. Replace the placeholder markup in that folder with the assignment app.
3. Keep the app's styles, scripts, and assets inside the same folder whenever
   possible.
4. Update the matching description in the root `index.html` so the directory
   describes the finished work.
5. Test both directions: open the assignment from the landing page, then use
   `Back home` to return to the directory.

The shell does not require an assignment to use a particular framework. A plain
HTML/CSS/JavaScript app, a compiled static bundle, or a small client-side app
can all live behind the same folder link as long as the folder still serves an
`index.html` entry point.

## Branch workflow

`main` owns the shared portfolio shell and the four folder entry points. Work on
each assignment in its matching branch:

- `hw1` for `assignment-01/`
- `hw2` for `assignment-02/`
- `hw3` for `assignment-03/`
- `hw4` for `assignment-04/`

Start new work from the latest `main`, keep assignment-specific edits inside the
matching folder, and merge the branch back into `main` when the app is ready.
For example:

```bash
git switch main
git pull --ff-only
git switch -c hw2
```

After testing the assignment locally:

```bash
git add assignment-02
git commit -m "Build homework 02"
git push -u origin hw2
```

Open a pull request for review, then merge it into `main`. The existing `hw1`
branch is the starting point for Homework 01 work.

If a branch needs the latest shell changes while it is in progress, update it
from `main` before continuing. Keep conflicts in the root landing page and the
assignment folder separate where possible so each assignment remains easy to
review.

## Accessibility and layout notes

The landing page uses semantic header, navigation, main, section, article, and
footer elements. Links have visible focus styles, arrows are inline SVGs with
`aria-hidden`, and the mobile navigation uses a native `details` disclosure.
The layout collapses from a two-column assignment grid to one column below the
mobile breakpoint, and the text sizes use responsive `clamp()` values so the
headings remain readable without horizontal scrolling.

The theme button has an accessible label that describes the next mode, and the
current choice is reflected through `aria-pressed`. A blocked or private
`localStorage` implementation does not prevent the page from rendering.

## Suggested review checklist

Before merging an assignment branch, verify:

- the assignment opens from the correct homepage link;
- the assignment's own stylesheet and scripts load from its folder;
- `Back home` returns to the root landing page;
- the dark default remains readable, including link and rule contrast;
- the light-mode control changes the page and persists after a reload;
- the layout works at both a desktop width and a narrow mobile width;
- the root landing page still has exactly four assignment destinations.
