---
layout: default
title: Home
nav_order: 0
---

# About This Site

The aim of this site is to provide information on the *Optical Music Recognition*
(OMR) process, specifically how to use it in the [Rodan workflow manager](https://ddmal.music.mcgill.ca/Rodan)
with square-notation manuscripts.
General information about OMR is available on the [main overview page]({{ "/overview" | prepend:site.baseurl}})
which has subpages on the different steps of the process.
Each subpage goes into detail on specific jobs that accomplish that task.
Additionally there is a [general overview of Rodan]({{ "/overview/rodan" | prepend:site.baseurl }}).

A brief tutorial of performing OMR using the [CDN-Mlr 073 manuscript](https://archive.org/details/McGillLibrary-rbsc_ms-medieval-073-18802)
as an example is present on the [tuorial pages]({{ "/tutorial" | prepend:site.baseurl }}).

# What Do I Need To Get Started?

This largely depends on your goals, but if you intend to create an encoding
of a page or a few pages in the [Music Encoding Initiative (MEI)](https://music-encoding.org)
format, you'll need the following materials:

1. High quality images of the pages;
    * If you need images, consult the [tutorial section on IIIF]({{ "/tutorial/iiif-manifest" | prepend:site.baseurl }}).
2. Models trained to detect staff lines, text, and music symbols in this
kind of manuscript;
    * If you need models, consult the [tutorial section on document analysis]({{ "/tutorial/document-analysis" | prepend:site.baseurl }}).
3. Sufficient training data for classifying glyphs in the format used by
[Gamera](https://gamera.infomatik.hsnr.de);
    * For generating training data for your manuscript, consult the
    [tutorial section on symbol classification]({{ "/tutorial/classification" | prepend:site.baseurl }}).
4. A CSV file mapping classes of glyphs to fragments of MEI; and
    * Consult [the MEI Mapping Tool](https://github.com/DDMAL/mei-mapping-tool)
    to create such a file or find an existing mapping that may work.
5. A plain-text transcript of the neumed text on the page and an OCR model
capable of reading that text.
    * If you need help getting a transcript, consult the
    [tutorial section on text alignment]({{ "/tutorial/music-reconstruction#text-alignment" | prepend:site.baseurl }}).
    * If you need to generate a model, read [the relevant section of the Text Alignment job's README](https://github.com/DDMAL/text_alignment#training-a-new-ocropus-model).

