---
layout: default
title: Score Generation and Correction
---

# Score Generation

# Score Correction

For square-notation scores, correction is performed using the [Neon](https://github.com/DDMAL/Neon/) (*Neume Editor Online*) job.
This works by displaying the source image in the background and rendering the contents determined by OMR in the foreground,
allowing errors to be easily perceived and corrected by the user[^1].
Detailed instructions on how to use Neon can be found on [the instructions page of its wiki](https://github.com/DDMAL/Neon/wiki/Instructions).
Note that only single-page mode is present on the Rodan job of Neon.

After finishing editing and saving the document, pressing the *Validate* button will send the corrected MEI file to Rodan.

# Footnotes

[^1]: [G. Burlet, A. Porter, A. Hankinson, and I. Fujinaga, “Neon.js: Neume Editor Online,” in Proceedings of the 13th International Society for Music Information Retrieval Conference (ISMIR), Porto, Portugal, 2012.](https://archives.ismir.net/ismir2012/paper/000121.pdf)
