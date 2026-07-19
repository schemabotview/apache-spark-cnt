# CLAUDE.md — apache-spark-cnt

A **content repo** (`<concept>-ct` in the workspace map), not an app. It holds the **Apache
Spark** concept, authored **video-first** for the UI/render app, which loads it **at
runtime** over the network (`manifest.json` + `md/` + `slides/` + `audio/`). No render
engine and no scenes live here. Read alongside the workspace map + design contract in
[`../README.md`](../README.md).

Authored **fresh**. The prior repos — `../../Products/apache-spark-ct/` (the `.ipynb`-based
video-target Spark repo) and `../../Products/apache-spark-content/` (the graphl-ux-era
curriculum) — are **reference material**, not something to copy wholesale. We refine
structure, not port it.

There is **nothing to build, run, or test** here. The one manual step (later) is the Colab
tool that turns `tts/` scripts into `audio/` `.wav`s (owner-run).

## Working agreement

**Step by step, one small slice, review gate between each.** Settle the shape first (module
spine → per-module sections → per-section content), then fill deliberately. **No batch
generation** — list files and get a yes before writing a set.

## The core contract (do not break)

1. **`md/NN-SS-slug.md` is the single source of truth** for a section's prose and code — one
   `## ` section per file, plain Markdown (not `.ipynb`: the app strips images and only needs
   the heading + prose/code, so notebook JSON is overhead and `.md` diffs cleanly for change
   detection). `manifest.json` only *wires* — it must never duplicate `.md` content.
2. **One file per section** (not per module) across all four artifacts — `.md` + `.slide` +
   `.tts` + `.wav` share the same `NN-SS-slug` stem — so a section is authored and reviewed on
   its own. The `.md`'s single `## ` heading is the section title (mirror it in the manifest
   `heading`).
3. **`.md`, `.slide`, and `.tts` are one editorial unit** — authored together in the same
   reviewed pass. They are siblings, not source + auto-derived outputs: the narration is a
   *spoken* re-expression and the slide is *re-fit* to the frame, so neither is a mechanical
   function of the `.md`. When a section changes, all three are re-authored together. There is
   **no change-detector that regenerates slides/tts.**
4. A section's diagram images (`![]()`) are **stripped** by the app — the React Flow **scene**
   replaces them.
5. **Scenes live in the UI app** (`src/scenes/*`), app-owned TypeScript. Here you only
   reference a scene **by id**, plus `highlight` (node ids that get the spotlight) and `focus`
   (a node id the camera frames) per section.

## Folder layout

```
apache-spark-cnt/
  md/            # NN-SS-slug.md      — one ## section each; SOURCE OF TRUTH for prose/code
  slides/        # NN-SS-slug.slide   — authored one-screen right-pane (title + bullets)
  tts/           # NN-SS-slug.tts     — plain spoken narration script
  audio/         # NN-SS-slug.wav     — generated from tts/ via Colab (owner-run)
  manifest.json  # wires sections → md / slide / scene / highlight / focus / audio / spine / role
  CLAUDE.md · README.md
```

Naming: every artifact for a section shares the stem `<NN>-<SS>-<slug>` (`NN` = module number,
`SS` = section position, so a sorted glob stays in reading order).

## The `.slide` format

A one-screen, scannable Markdown subset — a `# Title`, then `## ` sub-labels, short prose,
fenced ` ```code``` ` blocks, `- `/numbered lists, and inline **`**bold**`** key terms
(rendered bright white, the rest a softer gray). **Keep the whole slide inside the fixed
1920×1080 frame:** the right pane does not scroll or auto-shrink, so an over-long slide clips
top and bottom — trim it to fit (drop connective prose the narration already carries). Title
may be punchier than the `.md` `## ` heading. Every layout decision is judged at **1080p after
re-encoding** — legibility surviving compression is the bar.

## The `.tts` format

Plain spoken narration: **no code, no markdown**; symbols spelled out (`dockerd` → "docker
daemon", `-p 8080:80` → "publish host port…"). Generated to `.wav` via Colab, owner-run —
never hand-edited, never reused across a re-author.

## manifest.json

Wires only; never duplicates `.md`. Per section: `scene` (id → app registry), `highlight`
(node ids brightened, rest dimmed), `focus` (node the camera frames), `slide`, `audio`,
`spine`, `role`, `heading`. Prefer generating it from the files + a small config over
hand-writing it.

## Scenes (app-side)

Three Spark scenes live in the UI repo: `spark-architecture` (whole-system execution map),
`spark-batch-api` (the shared batch / DataFrame / RDD / SQL / perf master map), and
`spark-streaming`. Here you reference them by id only. **Keep node ids identical to the source
scenes** so `highlight`/`focus` wiring transfers.

## Curriculum

The course outline (module spine + per-module sections) lives in [`README.md`](./README.md) —
the single human-facing source for structure while we plan; `manifest.json` becomes the
machine source of truth once authored. Don't duplicate the module/section list here.

**Target:** ~10 modules × ~10 sections, each authored as one standalone narrated video (one
dense scene + a linear section walkthrough). The exam-questions module and the hands-on
project are parked.

## Status

Scaffolded. `README.md` holds the proposed 10-module × ~10-section spine (**under review** —
this is the current working step). No sections authored yet; folders (`md/`, `slides/`,
`tts/`, `audio/`) not yet populated. Next: settle the module/section categorization in
`README.md`, then author section by section under the review gate.
