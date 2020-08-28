---
layout: default
title: Document Analysis
parent: Overview
nav_order: 1
---

# Document Analysis

## Pixel.js

Pixel.js is a web browser-based graphical interface for separating pixels of a music score image into layers for OMR (including an automatically generated background layer).
It can output either the original image split into different layers or masks of those layers.
Different shapes or brushes can be used to classify pixels and patches into different layers.

The output format of the layers (mask or not) can be set through the Rodan job's settings.
The number of output ports for this job is the number of layers generated plus two for
the automatically generated background layer and selected regions layer.

Optionally, existing layers can be loaded through input ports. One useful way to use this feature
is to use existing classifier models to generate layers of the source image to use in Pixel.js.
Errors would be made, but these can be corrected in Pixel.js and saves time compared to working
from scratch.

<figure markdown="1">
![]({{site.baseurl}}/assets/classifier-pixel-workflow.png)
<figcaption>
Classifier to Pixel Workflow in Rodan. A PNG resource representing the image of a manuscript page is distributed to the
Fast Pixelwise Analysis job and the Pixel.js job. The Fast Pixelwise Analysis job also has pre-existing models as its other
inputs to separate the layers of the manuscript page. The job produces imperfect separated layers that are transferred to
Pixel.js to form a starting point for verified separated layers the user can produce.
</figcaption>
</figure>

For more information on using Pixel.js, consult [the project's wiki](https://github.com/DDMAL/Pixel.js/wiki/).
For information on the version of Pixel.js in Rodan, including how to properly set up input and output ports, [look at the project's README](https://github.com/DDMAL/pixel_wrapper/blob/master/README.md).

**Note: When loading pre-existing layers into Pixel, the layers must finish loading before starting the Pixel plug-in in Diva. If not, the layers will not appear.**

### Settings

* **Output Mask** - Whether Pixel.js should output layers with the actual pixels from the
source image or output them as an image mask.
Defaults to "false" (actual pixels).

### Input Ports

* **Image** - The image to classify into layers.
* *Layer Inputs* - Some number of layers can be added as inputs.
The layer data will be preloaded into Pixel upon loading.
*Do not upload a background layer as this is generated automatically.*

### Output Ports

* **Layer Outputs** - The layers generated in Pixel.
Note that if you are classifying *n* layers, you should add output ports for
*n+2* layers as a background layer and selected regions layer will be automatically generated as the zeroth and last outputs respectively.

## Patchwise Trainer

The [Patchwise Trainer](https://github.com/DDMAL/Calvo-classifier) job (actually named *Training model for
Patchwise Analysis of Music Document* and part of the set of Calvo classifier jobs) generates the models for
classifying image pixels into OMR-relevant layers automatically. It uses a source image and its handcrafted layer
masks generated with Pixel.js as inputs.
Its outputs are the models for detecting the layers of background, music symbols, staff lines, and text.
There are four settings for this job:

### Settings

1. Maximum number of samples per label;
2. Maximum number of training epochs;
3. Patch height; and
4. Patch width.

Note that these settings and the dimensions of the source image will change the memory requirements of the job.
As of the time of writing (July 2020), the job and GPU container itself can only use 45 GB on staging and 100 GB
on production.

### Input Ports

* **Image** - The image to use for training and whose layers will be provided.
* **Background layer** - The mask for the background layer.
* **Music symbol layer** - The mask for the music symbol layer.
* **Staff lines layer** - The mask for the staff lines layer.
* **Text layer** - The mask for the text layer.
* **Selected regions** - The mask for regions of the image that were manually classified into layers.

### Output Ports

* **Background Model** - The model for the background.
* **Music Symbol Model** - The model for music symbols (e.g., neumes, custodes, clefs).
* **Staff Lines Model** - The model for staff lines.
* **Text Model** - The model for text.
* *Log File* - A file containing the information logged by the job.

## HPC Patchwise Trainer

[This job](https://github.com/DDMAL/hpc-fast-trainer) runs the trainer discussed above on the Compute Canada Cedar
cluster. It can only be run from staging as of now, but it provides access to more resources than available on the cloud.

### Settings

Since resource limits must be specified a priori on the clusters, the following additional settings are present along with the Patchwise Trainer's settings:

1. Maximum time to run in days, hours, and minutes;
2. The number of CPUs to allocate to the job (one GPU is allocated regardless of this value);
3. The email to send notifications to; and
4. The maximum memory the job will use in megabytes.

### Input Ports

* **Image** - The image to use for training and whose layers will be provided.
* **Background layer** - The mask for the background layer.
* **Music symbol layer** - The mask for the music symbol layer.
* **Staff lines layer** - The mask for the staff lines layer.
* **Text layer** - The mask for the text layer.
* **Selected regions** - The mask for regions of the image that were manually classified into layers.

### Output Ports

* **Background Model** - The model for the background.
* **Music Symbol Model** - The model for music symbols (e.g., neumes, custodes, clefs).
* **Staff Lines Model** - The model for staff lines.
* **Text Model** - The model for text.

## Fast Pixelwise Classifier

[This job](https://github.com/DDMAL/Calvo-classifier) uses models generated by the previously
mentioned trainers to classify pixes in an image as either music symbols, staff lines, text,
or background.

### Settings

* **Height** - The patch height in pixels. Defaults to 256.
* **Width** - The patch width in pixels. Defaults to 256.
* **Threshold** - The confidence threshold to classify a pixel, as a percent. Defaults to 50.

### Input Ports

* **Image** - An image whose pixels are to be classified.
* **Background Model** - The model for the background.
* **Music Symbol Model** - The model for music symbols.
* **Staff Lines Model** - The model for staff lines.
* **Text Model** - The model for text.

### Output Ports

* *Background* - The pixels classified as background.
* *Music symbol* - The pixels classified as music symbols.
* *Staff lines* - The pixels classified as staff lines.
* *Text* - The pixels classified as text.
* *Log File* - A log file of classification.
