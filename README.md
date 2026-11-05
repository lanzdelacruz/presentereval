# PresenterEval

A real-time web app for evaluating live classroom reports: presenter delivery, slide quality, time management and Q&A.

**Live app:** https://lanzdelacruz.github.io/presentereval/

Author: Lanz Dela Cruz

## Features

- **Dynamic evaluation matrix.** Four weighted criteria (Clarity & Tone, Visual Media Quality, Time Management, Q&A Handling) rated 1 to 5 with radio groups, adjustable weight sliders, and expandable feedback cards (`<details>`). Weighted score, per-criterion points, average rating and progress indicators update instantly with no page reload. DOM writes are batched into one `requestAnimationFrame` pass.
- **Media notes.** Audio and video commentary recorded in the browser with `navigator.mediaDevices.getUserMedia` and `MediaRecorder`, saved at the timestamp where recording started. If the camera or microphone is unavailable, the app falls back to attaching a recording file.
- **Timestamped bookmarks.** A report clock tracks elapsed time against the allotted time. "Flag moment" and five quick flags (e.g. "Pacing too fast", "Great slide transition") drop interactive bookmarks on the timeline; each bookmark jumps to an editable note.
- **Offline resilience.** Drafts autosave to IndexedDB (with a localStorage fallback) and survive refreshes, including a running clock. Submitted evaluations go into an outbox and re-sync automatically on the `online` event, with retry and backoff. A "Simulate network drop" toggle demonstrates this.
- **Accessibility (WCAG 2.1 AA).** Semantic HTML5 (`main`, `section`, `article`, `form`, `fieldset`, `legend`, `meter`, `progress`, `output`), explicit `<label>` bindings, visible `:focus-visible` rings, an `aria-live="polite"` region for score updates, light and dark themes, and reduced-motion support.
- **Export.** Evaluations export to CSV or JSON, serialized from stored data rather than scraped from the page.

## Keyboard shortcuts

| Keys | Action |
| --- | --- |
| `F`, or `Space` / `Enter` on the page | Flag the current moment |
| `Shift` + `1` to `5` | Quick flags |
| `1` to `5` | Rate the highlighted criterion |
| `J` / `K` | Next / previous criterion |
| `Tab` / `Shift` + `Tab` | Move between controls |
| `S` | Start, pause or resume the report clock |
| `A` / `V` | Start or stop an audio / video note |
| `Esc` | Leave a note field or close the shortcuts panel |
| `?` | Show shortcuts |

## Performance

Single file, no libraries or build step. Total HTML/CSS/JS is about 70 KB (about 21 KB gzipped), well under the 300 KB budget. The clock updates the DOM once per second, and the timeline uses native `<progress>`, so live recording and playback stay smooth.

## Running locally

Camera and microphone access require a secure context (https or localhost):

```bash
npx serve .
```

Then open http://localhost:3000.

## Sync endpoint (optional)

Set `SYNC_ENDPOINT` near the top of the script in `index.html` to a URL that accepts POSTed evaluation JSON. When it is empty, evaluations stay on the device.
