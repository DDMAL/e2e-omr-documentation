---
layout: default
title: Home
nav_order: 0
---

# Optical Music Recognition with Rodan

This site describes the *Optical Music Recognition*
(OMR) process implemented by the [Distributed Digital Music Archives and Libraries](https://ddmal.music.mcgill.ca/) (DDMAL) lab
for encoding manuscripts in the [Music Encoding Initiative](https://music-encoding.org) format.
It demonstrates the [Rodan workflow builder and manager](https://ddmal.music.mcgill.ca/Rodan)
and stages of processing for interpreting and encoding square-notation music with machine learning.

General information about OMR is available on the [main overview page]({{ "/overview" | prepend:site.baseurl}}),
which also includes subpages on the different steps of the OMR process.
Each subpage goes into detail on specific jobs that accomplish a specific task.
Additionally, there is a [general overview of Rodan]({{ "/overview/rodan" | prepend:site.baseurl }}).

New users can follow a brief tutorial of performing OMR using the [CDN-Mlr 073 manuscript](https://archive.org/details/McGillLibrary-rbsc_ms-medieval-073-18802)
as an example, which is available in the [tutorial pages]({{ "/tutorial" | prepend:site.baseurl }}).

# What Do I Need To Get Started?

## Hardware and Software

Rodan is a web app and therefore doesn't require you to install anything, but for the best experience, it's recommended
that you use a recent version of Google Chrome or Firefox and use a computer with at least 8 GB of RAM.
However, everything should work on a modern computer using a recent version of a popular browser.

It may be necessary to generate some resources locally (e.g., like the Optical Character Recognition models.) Instructions to create these files are available with the jobs documentation.

## Digital Resources

This largely depends on your goals, but if you intend to create an encoding
of a page or a few pages in the [Music Encoding Initiative (MEI)](https://music-encoding.org)
format, you'll need the following materials:

1. High-quality images of the manuscript pages;
    * For instructions on obtaining images, consult the [tutorial section on IIIF]({{ "/tutorial/iiif-manifest" | prepend:site.baseurl }}).
2. Computational models trained to detect score elements in this kind of manuscript: staff lines, text, and music symbols.
    * Consult the [tutorial section on document analysis]({{ "/tutorial/document-analysis" | prepend:site.baseurl }})
    for instructions on developing these models.
3. Training data for classifying music symbol glyphs in the format used by
[Gamera](https://gamera.informatik.hsnr.de);
    * Consult the
    [tutorial section on symbol classification]({{ "/tutorial/classification" | prepend:site.baseurl }})
    for instructions on generating training data from a few pages of a manuscript.
4. A CSV file mapping classes of glyphs to fragments of MEI; and
    * Consult [the MEI Mapping Tool](https://github.com/DDMAL/mei-mapping-tool)
    to create such a file or find an existing mapping that may work.
5. A plain-text transcript of the neumed text or lyrics on each page of the manuscript and an OCR model
capable of reading that text.
    * For instructions on generating a transcript, consult the
    [tutorial section on text alignment]({{ "/tutorial/music-reconstruction#text-alignment" | prepend:site.baseurl }}).
    * Follow [the relevant section of the Text Alignment job's README](https://github.com/DDMAL/text_alignment#training-a-new-ocropus-model)
    for instructions on generating a suitable OCR model.

