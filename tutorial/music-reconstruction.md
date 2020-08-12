---
layout: default
title: Music Reconstruction and Encoding Tutorial
parent: OMR Tutorial
nav_order: 3
---

The music reconstruction step is fairly straight forward. It does rely on
connected components from [the previous step](/overview/classification).

# Miyao Staff Finding

The Miyao Staff Finding job uses an image of staff lines and determines the characteristics of the staff/staves.
It requires a specific black-and-white cleaned-up input image, much like
the CC Analysis job previously.

Here, however, after despeckling the black-and-white image it should be dilated using the Dilate job to improve the results of the staff finding job.
The steps to run the Miyao Staff Finding job are present in the image below.

![Workflow in Rodan. A PNG job for the staff lines image leads to a convert to black and white PNG job, then to a despeckle job, then to a dilate job, then to the Miyao Staff Finding job.](/assets/workflow-miyao.png)

# Heuristic Pitch Finding

The Heuristic Pitch Finding job combines classified connected components
previously generated with the staff information produced by the Miyao Staff Finding job.
It then produces a JSON file containing information on glyphs (from the connected components), the staves, and the page itself.

A workflow showing the Heuristic Pitch Finding job using the results of the Interactive Classifier and Miyao Staff Finding workflows is below.

![The connected components results from the Interactive Classifier and the staff information from the Miyao Staff Finding job are inputs to the Heuristic Pitch Finding job in the workflow.](/assets/workflow-ic-miyao-pf.png)

# Text Alignment

The [Text Alignment](/overview/reconstruction-and-encoding#text-alignment)
job is an optional step that allows the user to include the text of the
source page into the encoded MEI. Specifically, this step takes a plain
text transcript of the neumed text on the page, breaks it into syllables,
and assigns these syllables to text on the page.

For many sources, the text can be obtained by using the
[Cantus Manuscript Database](http://cantus.uwaterloo.ca). The following
steps can be used to obtain the text for a page:

1. Find the [source in the database](http://cantus.uwaterloo.ca/sources).
    * The siglum is the most direct way to find it, but many other options
    (e.g., provenance, description, century, etc.) can be used to search.
2. Export as a CSV using the box in the upper right corner of the screen.
The button "CSV export" will download the necessary file.
![An example page of a source in Cantus. A box is drawn around the section containing download options.](/assets/cantus-source.png)
3. Open the CSV file in a program like Microsoft Excel or LibreOffice Calc.
4. Locate the row corresponding to the folio of the page.
5. Copy the text in that row under the heading `fulltext_ms` to a plain text (`.txt`) file. This is the input for the text alignment job.
