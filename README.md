# Homework Portfolio

This repository is the home page and container for four homework projects. The
landing page is intentionally small: it introduces the collection and links to
four separate folders. Each folder is a standalone web app placeholder that can
be replaced by a finished assignment without redesigning the portfolio shell.

## What is here

The project is a static site, so there is no build step or framework dependency.

| Path | Purpose |
| --- | --- |
| [`index.html`](index.html) | Portfolio landing page and assignment directory |
| [`styles.css`](styles.css) | Landing page layout, responsive rules, and theme tokens |
| [`assignment-shared.css`](assignment-shared.css) | Shared baseline styles for standalone assignment pages |
| `assignment-01/` | Homework 01 standalone app placeholder |
| `assignment-02/` | Homework 02 standalone app placeholder |
| `assignment-03/` | Homework 03 standalone app placeholder |
| `assignment-04/` | Homework 04 standalone app placeholder |

Each assignment folder contains its own `index.html` and a local `styles.css`
entry point. The local stylesheet imports the shared baseline so the placeholders
look consistent today, while a future assignment branch can replace that import
with completely local app styles when it needs a different visual system.

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
should offer a `Back home` link.

There is no package manager setup or compilation step. Any static server that
serves the repository root will work.

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
