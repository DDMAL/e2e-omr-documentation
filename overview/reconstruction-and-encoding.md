---
layout: default
title: Music Reconstruction and Encoding
parent: Overview
nav_order: 3
---

# Text Alignment

The [Text Alignment](https://github.com/DDMAL/text_alignment) job aligns a provided transcript of
neumed text or lyrics on a page to the text that appears on the page itself by using optical
character recognition (OCR) and the [Needleman-Wunsch algorithm](https://en.wikipedia.org/wiki/Needleman%E2%80%93Wunsch_algorithm).

### Input Ports

* **Text Layer** - The layer of text in the source image.
* **Transcript** - A transcript of the text on the page.
* **OCR Model** - An OCRopus OCR model.

### Output Ports

* **Text Alignment JSON** - A JSON file containing text alignment information.

# Miyao Staff Finding

This job performs the Miyao staff finding algorithm.
It finds the staves in an image of staves.
Note that this is **not** the job called "Miyao Staff Find*er*" as that job has different settings and produces a different output.

### Settings

* **Number of lines** - The number of lines in a staff. The default is 4. Zero
automatically detects the number of lines.
* **Interpolate** - Interpolate found line points so all lines have the same number of points.
This must be set to true.

### Input Ports

* **Image containing staves** - The staff line layer.

### Output Ports

* **JSOMR** - A JSON file containing staff and staff line information.

# Heuristic Pitch Finding

This job finds the pitch of musical element connected components. This requires the output from the Miyao Staff Finding job to work properly.

### Settings

* **Discard Size** - The size of elements to discard. Default is 12.

### Input Ports

* **JSOMR of staves and page properties** - The output of the Miyao Staff Finding Job.
* **Classified Connected Components** - The classified connected components from a classifier job.

### Output Ports

* **JSOMR of glyphs, staves, and page properties** - A combined JSON file with data on
musical elements with pitches, staves, and the page as a whole.
