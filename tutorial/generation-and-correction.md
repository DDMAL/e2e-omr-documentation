---
layout: default
title: Score Generation and Correction Tutorial
parent: OMR Tutorial
nav_order: 4
---

# MEI Encoding

The data produced by the previous steps is still not in the MEI format.
The role of the MEI encoding job is to transform this data from JSON
into MEI. This obviously requires the data itself, but also requires
a CSV file mapping the class of a glyph in that manuscript (e.g., `neume.punctum`)
to an MEI fragment that represents it (e.g., `<neume><nc/></neume>`).
The easiest way to produce this kind of CSV file is through the
[MEI mapping tool](https://github.com/DDMAL/MEI-mapping-tool), although it
can also be manually created through a tool like Microsoft Excel or
LibreOffice Calc.

If text alignment data is provided, the MEI Encoding job will attempt to match
the text to neumes and group them as syllables. Otherwise there will be
one syllable in the entire staff.
For grouping neume components into neumes, a heuristic is used based on the
position of the glyphs.

# Correction with Neon

Neon renders the MEI produced in the previous step over the source image
by using [Verovio](https://www.verovio.org).
A [full description of its features are available on the project's wiki](https://github.com/DDMAL/Neon),
but there are a few key points for using it as part of the workflow.

1. Adjust staff size and rotation. This will help glyphs appear over the
correct place on the page.
2. Correct elements that were incorrectly classified. A typical case is
that a C clef is classified as two neume components, or an F clef as three.
3. Correct the pitch of elements. This is done by just dragging to the
correct position.
4. Correct the grouping of neumes and syllables. If text alignment data was
provided, this is often close to correct already.
5. When you're done, press the "Validate" button in the upper left of the
screen to send the new MEI file back to Rodan so it can be saved as a
resource.
