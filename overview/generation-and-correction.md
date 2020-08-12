---
layout: default
title: Score Generation and Correction
parent: Overview
nav_order: 4
---

# Score Generation &mdash; MEI Encoding

Score generation is performed with the [MEI encoding](https://github.com/DDMAL/MEI_encoding) job.
This job uses the information produced in the reconstruction and encoding steps to create a valid symbolic document.
Glyphs are matched to MEI snippets and pitch data is applied if applicable.

If text alignment was performed, textual information is also included and used to group neumes into syllables complete with text.

### Settings

* **Neume Component Spacing** - A multiplier controlling the spacing allowed between two neume components when grouping into neumes. 1.0 will use the median width of all glyphs on the page, 2.0 will use twice the median width, and so on. At 0, neume components will not be merged together, and each one will be treated as its own neume. Defaults to 0.5.

### Input Ports

* **JSOMR** - A JSON file containing OMR information on staves, glyphs, and the page.
* **MEI Mapping CSV** - A file mapping classes of glyphs to MEI snippets.
This can be produced by the [MEI Mapping Tool](https://github.com/DDMAL/mei-mapping-tool).
* *Text Alignment JSON* - A file containing the text alignment information
from the [Text Alignment job]({{site.baseurl}}/overview/reconstruction-and-encoding#text-alignment).

### Output Ports

* **MEI** - A file containing the information from the inputs in the [MEI](https://music-encoding.org) format.

# Score Correction &mdash; Neon

For square-notation scores, correction is performed using the [Neon](https://github.com/DDMAL/Neon/) (*Neume Editor Online*) job.
This works by displaying the source image in the background and rendering the contents determined by OMR in the foreground,
allowing errors to be easily perceived and corrected by the user[^1].
Detailed instructions on how to use Neon can be found on [the instructions page of its wiki](https://github.com/DDMAL/Neon/wiki/Instructions).
Note that only single-page mode is present on the Rodan job of Neon.

After finishing editing and saving the document, pressing the *Validate* button will send the corrected MEI file to Rodan.

### Input Ports

* **MEI File** - A valid MEI file to correct, such as the one produced
by the [MEI Encoding job](#score-generation--mei-encoding).
* **Source Image** - The source image used to generate the information.

### Output Ports

* **Corrected MEI File** - An MEI file incorporating the corrections performed in the job.

# Footnotes

[^1]: [G. Burlet, A. Porter, A. Hankinson, and I. Fujinaga, “Neon.js: Neume Editor Online,” in Proceedings of the 13th International Society for Music Information Retrieval Conference (ISMIR), Porto, Portugal, 2012.](https://archives.ismir.net/ismir2012/paper/000121.pdf)
