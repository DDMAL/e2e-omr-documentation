---
layout: default
title: Obtaining Images from a IIIF Manifest
parent: OMR Tutorial
nav_order: 0
---

# What is a IIIF Manifest anyway?

[IIIF](https://iiif.io) describes numerous ways to work with images.
In this tutorial, "IIIF manifest" refers specifically to a IIIF Presentation API manifest.
This is, in short, a machine readable file that contains information on
where to get a set of images and how to display them to the user.

This is excellent if you're using a IIIF-aware application,
[of which there are many](https://iiif.io/apps-demos/),
but sadly Rodan isn't one of them (yet).
So, when this happens, you need to download the images.

# Downloading Images from a Manifest

IIIF documents are referenced by a URL.
For this example, we will be working with the CDN-Mlr 073 manuscript
which has a IIIF Presentation manifest at <https://iiif.archivelab.org/iiif/McGillLibrary-rbsc_ms-medieval-073-18802/manifest.json>.

The images from a IIIF manifest can be downloaded using a graphical program like the one at <https://github.com/JRegimbal/iiif-downloader> or a smaller command line program like <https://github.com/ryanfb/iiif-dl>.

![IIIF Downloader App]({{site.baseurl}}/assets/iiif-downloader-main.png)

Using the graphical program, a folder is selected and the URL to the IIIF manifest is entered.
After pressing the Start button, the program will create a folder with the name of the manuscript (in this case, "A 15th Century Italian antiphonal : manuscript").
Each image file is downloaded with a name combining the canvas number and the label for that canvas.

![IIIF Downloader Running]({{site.baseurl}}/assets/iiif-downloader-working.png)
