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

* **zip** - A zip file containing **Image** layers generated in Pixel.
Note that if you are classifying *n* layers, you should add output ports for
*n+2* layers as a background layer and selected regions layer will be automatically generated as the zeroth and last outputs respectively.

## Background Removal
* Job Name: `Background Removal`
* Category: `Background removal - remove image background`

[This job](https://github.com/DDMAL/background_removal/tree/release) classifies pixels of a manuscript into two categories: foreground and background, and removes the background. Background includes objects such as text, neumes, and staff, and background contains all pixels that do not belong to the foreground.

### Settings
* `Background Removal Method`: A drop-down menu to select the method to do background removal. Support using [Sauvola Threshold](https://scikit-image.org/docs/stable/auto_examples/segmentation/plot_niblack_sauvola.html) (no neural network) and [Selectional Auto-Encoder (SAE) binarize](https://github.com/ajgallego/document-image-binarization) (with neural network)
* `window_size`: The `window_size` for Sauvola Threshold, see docs for [`skimage`](https://scikit-image.org/docs/stable/api/skimage.filters.html#skimage.filters.threshold_sauvola)
* `k`: The `k` for Sauvola Threshold, see docs for [`skimage`](https://scikit-image.org/docs/stable/api/skimage.filters.html#skimage.filters.threshold_sauvola)
* `contrast`: adjust contrast before using Sauvola Threshold with `img = img * (contrast/127+1) - contrast + brightness`
* `brightness`:  adjust brightness before using Sauvola Threshold with `img = img * (contrast/127+1) - contrast + brightness`

### Input Ports
* `Image` of type `rgb+png`, `rgb+jpg`, `rgba+png`, `greyscale+png`, `onebit+png`: The manuscript that you want to remove its background.

### Output Ports
* `RGBA PNG image` of type `rgba+png`: the input `Image` with its background removed. 
  * The reuslt looks like this: ![](https://i.imgur.com/sTnVwaL.png)
* (Optional) `Empty Layer`: An empty layer ready to be used as the `PNG-Layer<i> Input` of the `Pixel_js` job.

## SAE binarization
* Job Name: `SAE Binarization`
* Category: `SAE Binarization - remove image background`

[SAE binarization](https://github.com/DDMAL/SAE_binarization/tree/release) uses a neural network to classify pixels of a manuscript into foreground and background. This job uses a pre-trained model, and you do not need to train a model from scratch.

### Settings
* This job does not contain settings.

### Input Ports
* `Image`: The manuscript that you want to remove its background.

### Output Ports
* `RGB PNG image` of type `rgba+png`: The input `Image` with its background removed.

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
3. Patch height;
4. Patch width; and
5. Batch size.

Note that these settings and the dimensions of the source image will change the memory requirements of the job.
As of the time of writing (July 2020), the job and GPU container itself can only use 45 GB on staging and 100 GB
on production.

Changing the batch size might effect the convergence of the training process. It is not recommended to change this value from the default (16) unless you know what you are doing.
Note that the number of samples per class also affects memory utilization.

### Input Ports

* **Image** - The image to use for training and whose layers will be provided.
* **Background layer** - The mask for the background layer.
* **Selected regions** - The mask for regions of the image that were manually classified into layers.
* **Layer 0** - The mask for the foreground layer. This layer is usually used for the symbols to be classified in the [classification step]({{site.baseurl}}/overview/classification.html#interactive-classifier). In the case of OMR, this layer tends to be the 'music symbol' layer.
* *Layer Inputs* - Some number of layers (layer 1–9) can be added as inputs. 

### Output Ports

* **Background Model** - The model for the background.
* **Model 0** - The model corresponding to layer 0.
* *Model Outputs* - Some number of models (model 1–9) can be added as outputs. These nine optional output models correspond to the nine optional input layers (layer 1–9).

*Example: When segmenting the document into music-symbol, staff-lines, and text layers, the required input layer 0 can be used for the music symbol layer, and layers 1 and 2 can be added for the staff-lines and text layers, respectively. In this case, the output model 0 would be the classification model for music symbols; model 1 would correspond to the staff-line classification model; and model 2 would be the text classification model.*

## HPC Patchwise Trainer

[This job](https://github.com/DDMAL/hpc-fast-trainer) runs the trainer discussed above on the Compute Canada Cedar
cluster. It provides access to more resources than the ones available on the cloud.

### Settings

Since resource limits must be specified a priori on the clusters, the following additional settings are present along with the Patchwise Trainer's settings:

1. Maximum time to run in days, hours, and minutes;
2. The number of CPUs to allocate to the job (one GPU is allocated regardless of this value);
3. The email to send notifications to; and
4. The maximum memory the job will use in megabytes.

### Input Ports

* **Image** - The image to use for training and whose layers will be provided.
* **Background layer** - The mask for the background layer.
* **Selected regions** - The mask for regions of the image that were manually classified into layers.
* **Layer 0** - The mask for the foreground layer. This layer is usually used for the symbols to be classified in the [classification step]({{site.baseurl}}/overview/classification.html#interactive-classifier). In the case of OMR, this layer tends to be the 'music symbol' layer.
* *Layer Inputs* - Some number of layers (layer 1–9) can be added as inputs. 

### Output Ports

* **Background Model** - The model for the background.
* **Model 0** - The model corresponding to layer 0.
* *Model Outputs* - Some number of models (model 1–9) can be added as outputs. These nine optional output models correspond to the nine optional input layers (layer 1–9).

*Example: When segmenting the document into music-symbol, staff-lines, and text layers, the required input layer 0 can be used for the music symbol layer, and layers 1 and 2 can be added for the staff-lines and text layers, respectively. In this case, the output model 0 would be the classification model for music symbols; model 1 would correspond to the staff-line classification model; and model 2 would be the text classification model.*

## Fast Pixelwise Classifier

[This job](https://github.com/DDMAL/Calvo-classifier) uses models generated by the previously
mentioned trainers to classify pixels in an image as either music symbols, staff lines, text,
or background.

### Settings

* **Height** - The patch height in pixels. Defaults to 256.
* **Width** - The patch width in pixels. Defaults to 256.
* **Threshold** - The confidence threshold to classify a pixel, as a percent. Defaults to 50.

### Input Ports

* **Image** - An image whose pixels are to be classified.
* **Background Model** - The model for the background.
* **Model 0** - The model corresponding to layer 0.
* *Model Inputs* - Some number of models (model 1–9) can be added as inputs. These nine optional input models correspond to the nine optional output models of the [trainer part]({{site.baseurl}}/overview/document-analysis.html#patchwise-trainer).

### Output Ports

* **Background** - The pixels classified as background.
* **Layer 0** - The pixels classified as layer 0.
* *Layer Outputs* - Some number of layers (layer 1–9) can be added as outputs. The output of *layer n* are the pixels classified as *layer n* according to *model n*.
* *Log File* - A log file of classification.

## Paco Trainer
* Job Name: `Training model for Patchwise Analysis of Music Document, Training`
* Category: `OMR - Layout analysis`

[Paco Trainer](https://github.com/DDMAL/Paco_classifier/tree/v1.0.0) It generates neural network models to classify pixels into OMR-relevant layers. It uses source images (manuscripts) and annotated layers (staff, neumes, background, text, ...) as the input and generates models to automatically classify pixels of a manuscript into one of the annotated layers.

### Settings
* `Batch Size`: The number of samples per step. `step = Maximum number of samples per label / Batch Size`
* `Maximum number of training epochs`: The number of epochs to train a model.
* `Maximum number of samples per label`: The number of training samples per epoch.
* `Patch height`: In each annotated layer, a model is trained to classify `Patch height x Patch width` region.
* `Patch width`: In each annotated layer, a model is trained to classify `Patch height x Patch width` region.

### Input Ports
* `Sample <1, 2, ...>`: The zip file from the `Pixel_js` job. A `Sample` represent a set of training data including image and annotated layers.

### Output Ports
* `Log File`: A log file of the training process.
* `Model <0, 1, ...>`: The trained model to classify pixels into layer.

## Paco Classifier
* Job Name: `Training model for Patchwise Analysis of Music Document, Classifying`
* Category: `OMR - Layout analysis`

[Paco Classifier](https://github.com/DDMAL/Paco_classifier/tree/v1.0.0) uses model generated by [Paco Trainer](#paco-trainer) to classify a manuscript into layers.

### Settings
* `Height`: Classify `Patch height x Patch width` region.
* `Width`: Classify `Patch height x Patch width` region.
* `Threshold`

### Input Ports
* `Image`: The image that you want to classify.
* `Background model`: The model generated by [Paco Trainer](#paco-trainer) that classify pixels into the background layer.
* `Model <0, 1, ...>`: The model generated by [Paco Trainer](#paco-trainer) that classify pixels into layers.

### Output Ports
* `Log File`: A log file of the training process.
* `Background`: A `rgba` image containing the background.
* `Layer <0, 1, ...>`: `rgba` images containing each layer.