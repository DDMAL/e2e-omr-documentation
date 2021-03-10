---
layout: default
title: Overview
nav_order: 1
has_children: true
---

# Optical Music Recognition

Optical Music Recognition (OMR) is the transformation of images of music notation into digital representations with limited direct human involvement.
The workflow present in Rodan and discussed here is segmented into four stages[^1]:

1. Document Analysis
2. Symbol Classification
3. Music Reconstruction and Encoding
4. Symbolic Score Generation and Correction

<figure markdown="1">
![]({{ "/assets/omr_stages.png" | prepend:site.baseurl }})
<figcaption>
Stages of the OMR Process (Vigliensoni et al. 2019)
</figcaption>
</figure>

OMR systems designed to process modern printed music notation can depend on general models
for interpreting music symbols and score structure. Rather than apply a single standard decoding
process, the Rodan workflow is tuned at each stage to facilitate the interpretation of handwritten
scores and manuscripts in less common notations.

There are different jobs in Rodan meant for each stage.

## Document Analysis

Document analysis involves certain generic image preprocessing jobs, such as those based on actions in [Pillow](https://python-pillow.org/) (e.g., image resizing) and [Gamera](https://gamera.informatik.hsnr.de/) (e.g., de-speckling).
The Pixel.js[^2] job, allows a user to separate regions of a manuscript page into different layers (e.g., background, staff lines, text, and music elements).
The Pixelwise and Patchwise trainer jobs[^3] take the layers produced by Pixel and use them to train a model for
automatically separating layers in other images, specifically other pages from the same manuscript.

## Symbol Classification

The music symbol layers found using the models in the previous step are broken
into a collection of connected components or glyphs. Each of these connected components
are then classified as score elements (e.g., C clef, custos, punctum).
This requires training data for each of the different classes (i.e., a set of
sample glyphs for each type of element to be identified). If prepared training data already
exists, it can be used to classify glyphs non-interactively. Else, training data can be
created from a manuscript page using the Interactive Classifier.

As classification occurs using k-nearest neighbor classification across
different features, there are some image features that may be more salient
than others. Feature selection can be performed using the Biollante job,
which performs optimization using a genetic algorithm.

## Music Reconstruction and Encoding

Once music symbols are classified by shape (C clefs, custodes, punctums, etc.), their meaning
can be further interpreted from their relative positions in the source image.
The jobs for this involve finding staves ([Miyao Staff Finding](https://github.com/DDMAL/gamera_rodan)), finding the pitches of musical elements ([Heuristic Pitch Finding](https://github.com/DDMAL/heuristic-pitch-finding)), and aligning a plaintext version of text on the page to the actual text glyphs ([Text Alignment](https://github.com/DDMAL/text_alignment)).

## Symbolic Score Generation and Correction

The [MEI Encoding](https://github.com/DDMAL/MEI_encoding) job takes the outputs of this music
encoding process and translates them into the [MEI](https://music-encoding.org) format.

Following MEI generation, the browser-based graphical interface [Neon](https://github.com/DDMAL/Neon) can be used to correct errors in the OMR process for square-notation manuscripts.

## Footnotes

[^1]: [G. Vigliensoni et al., “From image to encoding: Full optical music recognition of Medieval and Renaissance music,” presented at the Music Encoding Conference, University of Vienna, Vienna, Austria, May 2019.](https://github.com/music-encoding/music-encoding.github.io/raw/master/_conferences/2019/abstracts_mec2019/vigliensoni19from%20camera%20ready.pdf)
[^2]: [Z. Saleh, K. Zhang, J. Calvo-Zaragoza, G. Vigliensoni, and I. Fujinaga, “Pixel.js: Web-based Pixel Classification Correction Platform for Ground Truth Creation,” in Proceedings of the Twelfth IAPR International Workshop on Graphics Recognition, Kyoto, Japan, 2017.](https://doi.org/10.1109/ICDAR.2017.267)
[^3]: [J. Calvo-Zaragoza, G. Vigliensoni, and I. Fujinaga, “Pixelwise classification for music document analysis,” in 2017 Seventh International Conference on Image Processing Theory, Tools and Applications (IPTA), Nov. 2017.](https://doi.org/10.3390/app8050654)
