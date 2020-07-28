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
So when this happens, you need to download the images.

# Downloading Images from a Manifest

IIIF documents are referenced by a URL.
For this example, we will be working with the Salzinnes, CDN-Hsmu M2149.L4
manuscript which has a IIIF Presentation manifest at <https://images.simssa.ca/iiif/manuscripts/cdn-hsmu-m2149l4/manifest.json>.

This can be downloaded using a small Python program at <https://github.com/JRegimbal/iiif-downloader> or a Ruby program at <https://github.com/ryanfb/iiif-dl>[^1].
To download the images with the Python program to a folder called `example-images`, we would run
```bash
python downloader.py https://images/simssa.ca/iiif/manuscripts/cdn-hsmu-m2149l4/manifest.json --path example-images
```
This would generate an output like this:
```
Downloading 479 images of manifest "Salzinnes, CDN-Hsmu M2149.L4" to example-images.
Downloading Folio 001r...
Downloading Folio 001v...
Downloading Folio 002r...
Downloading Folio 002v...
Downloading Folio 003r...
Downloading Folio 003v...
Downloading Folio 004r...
Downloading Folio 004v...
Downloading Folio 005r...
Downloading Folio 005v...
Downloading Folio 006r...
```
This isn't a particularly fast way to get the images, so if you have a
different way to access the source files that would likely be better.

[^1]: iiif-dl works very similarly to the iiif-downloader, however note that it requires the manifest file to be locally saved.
