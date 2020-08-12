---
layout: default
title: Document Analysis Tutorial
parent: OMR Tutorial
nav_order: 1
---

*Note: This tutorial uses [McGill MS-73](https://archive.org/details/McGillLibrary-rbsc_ms-medieval-073-18802).*

# Image Layering

It's usually necessary to create a new model to split images into layers.
This can be done using [Pixel.js](/overview/document-analysis#pixeljs) or a desktop program like [Pixelmator](http://www.pixelmator.com/).
This process is time consuming and can be sped up if you have a model already
trained on a similar manuscript.
In this case, [models from the Salzinnes manuscript](https://github.com/DDMAL/Calvo-classifier/tree/develop/Models/01-square-notation/02-three-pages-val-acc-20-epochs/models) can be used.

In this example we are using pages 40, 230, and 176 from MS-73. Layers were created
using Pixelmator and then loaded into Pixel.js to create the background and
selected regions layers and ensure all layers were masks by setting the
generate masks setting to "true".

The files were then combined using [Image Magick](https://imagemagick.org).
The dimensions of the files differed slightly, so background values were
specified as the default of white was not suitable. The full images were
taken with a black background, and so "black" was used there. For the masks,
the keyword "none" was used to fill any gaps with transparent pixels.
So, for example with the full images:
```bash
convert -background black _A_15th_century_Italian_antiphonal___manuscript__0_40_0.jpg \
_A_15th_century_Italian_antiphonal___manuscript__0_230_0.jpg \
_A_15th_century_Italian_antiphonal___manuscript__0_176_0.jpg -append 40-230-176.jpg
```
<figure markdown="1">
![Combined Image of Neume Layers](/assets/40-230_Neumes.png){:width="275"}
<figcaption>
Combined Image of Neume Layers
</figcaption>
</figure>

After testing with the training, it was determined the images needed to be
resized. They were resized to 52.2% by using the following command:
```bash
convert 40-230-176.jpg -resize 52.2% 40-230-176-Resized.jpg
```

These combined files were uploaded back to Rodan for the next step.

# Model Training

Once the images and layers to use have been combined, the [Patchwise Trainer](/overview/document-analysis#hpc-patchwise-trainer) can be used
to generate new models.
For larger images (especially when combined), it may
be necessary to use
the HPC (High Perfomance Computing) job which dispatches the file to
the Cedar cluster of Compute Canada.

We will use this for our example with the following settings:

* Maximum Time: 0-12:00
* Maximum number of samples per label: 20000
* CPUs: 6
* Maximum number of training epochs: 15
* Maximum memory (MB): 250000
* Patch width: 256
* Patch height: 256

# Layer Extraction of Another Page

After testing that the models work on a page that it was trained on (e.g., page 176)
it is time to use them on another page from the same manuscript. Here,
page 40 of MS-73 is used. The "Fast Pixelwise Classifier" job is run on the page scaled down to 52.2% of the original size with the default settings.
Three of the layers generated &mdash; music symbols, staff lines, and text &mdash; are used in future steps.
