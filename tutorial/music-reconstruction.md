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
