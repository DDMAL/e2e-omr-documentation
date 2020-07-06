---
layout: default
title: Overview
---

# Optical Music Recognition

Optical Music Recognition (OMR) is the transformation of images of music notation into digital representations with limited direct human involvement.
The workflow present in Rodan and discussed here is segmented into four stages[^1]:

1. Document Analysis
2. Symbol Classification
3. Music Reconstruction and Encoding
4. Symbolic Score Generation and Correction

{% figure caption:"Stages of the OMR Process (Vigliensoni et al. 2019)"%}
![](assets/omr_stages.png)
{% endfigure %}

There are different jobs in Rodan meant for each job.

## Document Analysis

Document analysis involves certain generic image preprocessing jobs, such as those based on actions in [Pillow](https://python-pillow.org/) and [Gamera](https://gamera.informatik.hsnr.de/).
There is also the Pixel.js[^2] job, for separating regions of a source image into different layers (e.g., background, staff lines, text, and music elements), and
the Pixelwise and Patchwise trainer jobs[^3]. These jobs take the layers produced by Pixel and use them to train a model for
classifying other images.

## Symbol Classification

Classifier, Interactive Classifier, Biollante.

## Music Reconstruction and Encoding

Heuristic Pitch Finding, Staff Finding, Text Alignment.

## Symbolic Score Generation and Correction

MEI Encoding, Neon.

## Footnotes

[^1]: [G. Vigliensoni et al., “From image to encoding: Full optical music recognition of Medieval and Renaissance music,” presented at the Music Encoding Conference, University of Vienna, Vienna, Austria, May 2019.](https://music-encoding.org/conference/2019/abstracts_mec2019/vigliensoni19from%20camera%20ready.pdf)
[^2]: [Z. Saleh, K. Zhang, J. Calvo-Zaragoza, G. Vigliensoni, and I. Fujinaga, “Pixel.js: Web-based Pixel Classification Correction Platform for Ground Truth Creation,” in Proceedings of the Twelfth IAPR International Workshop on Graphics Recognition, Kyoto, Japan, 2017.](https://doi.org/10.1109/ICDAR.2017.267)
[^3]: [J. Calvo-Zaragoza, G. Vigliensoni, and I. Fujinaga, “Pixelwise classification for music document analysis,” in 2017 Seventh International Conference on Image Processing Theory, Tools and Applications (IPTA), Nov. 2017.](https://doi.org/10.3390/app8050654)
