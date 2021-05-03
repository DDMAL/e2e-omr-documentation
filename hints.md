---
layout: default
title: Hints
nav_order: 3
---

# Tips & Tricks for OMR in Rodan

## Staff Size Height and Training

The Calvo classifier and trainer jobs are sensitive to the size of images.
For music with staves, the distance between staff lines in pixels (*staff size height*)
tends to be a predictor of how well it will perform. For instance, with the original
CDN-Hsmu M2149.L4 images, the staff size height is 64 px.
Values around this point may result in better classification and training results, but
note that this is not an optimized measure.

For better classification results, the **Resize Image job** can be used to modify the image size to have a *staff size height* closer to 64 px. Of course, at the end of the OMR workflow, the MEI Encoding job would output an MEI file with coordinates pointing to this smaller image. Therefore, the [**MEI Resizing job**](https://github.com/DDMAL/MEI_resizing) should get an MEI file with the original image coordinates. The following figure shows the complete OMR workflow, including the *Resize Image* and *MEI Resizing* jobs.

![OMR workflow incuding resizing jobs (Resize Image and MEI Resizing)]({{site.baseurl}}/assets/workflow-with-resize.png)

## Labelling Resources

Due to the number of resources the OMR process can produce (and the fact that over multiple
iterations many will have the same or similar names) resource labels can be used to keep
track of important resources. Labels can be added when resources are uploaded by using the
tag box next to the upload button, or after uploading by selecting the resource(s) and then
entering the labels in the detailed view to the right of the screen.

![Label input next to the upload button]({{site.baseurl}}/assets/label.png){:width="75%"}

Labels are separated by commas and can contain alphanumeric characters and many special characters.
The Labeler job can be used to add a label to resources produced in a workflow.

## Selecting Multiple Resources

Rodan makes it possible to select and manage multiple resources at a time. Hold the `Control` key
(`Command` on macOS) and click multiple resources to select them. It's also possible to select
a resource, hold `Shift`, and click another resource to select all the resources in the list between
those two resources and the resources themselves.

When multiple resources are selected it's possible to change attributes that already have the
same value (i.e., file type and labels) and perform actions available on all the resources
(e.g., delete, download). It's not perfect, but it helps save time when going through resources
in Rodan.

## Processing Multiple Folios
While one can use the OMR workflow to process individual folios, it can also be used to process a complete manuscript. Check [Running the Same Workflow with Different Resources]({{site.baseurl}}/overview/rodan.html#running-the-same-workflow-with-different-resources) for more information.

When processing multiple folios in the OMR workflow, if having the transcript text, you are changing the resources of two input ports---the one for the image at the beginning of the workflow, and the one for the transcript of the lyrics in the Text Alignment job---between workflow runs (this is **_scenario 2_** of the [Running the Same Workflow with Different Resources]({{site.baseurl}}/overview/rodan.html#running-the-same-workflow-with-different-resources)). Then, the resources for each run must be assigned to the input ports taking care that *the same number of resources must be assigned to each input port that has more than one resource assigned (here the input port of the PNG job at the beginning of the workflow and the input port for the transcript of the neumed text of the Text Alignment job) and the n<sup>th</sup> resource of each port will be in the same workflow run.* To guarantee the latter, making sure that both images and text files are processed in the same workflow run, one needs to provide the list of images and text files in the same order in their respective input ports. To achieve this, it is recommended to follow these conventions:

* Use three digits for the folio numbers (e.g., use *001r* and *010v* rather than *1r* and *10v*, respectively).
* Remove any images with no chant content (e.g., front and back book covers, blank pages, and pages containing only staves or illuminations with no music content) since these are not supposed to be processed by the OMR workflow, and you won't have a transcript text corresponding to them.

The previous conventions would allow to correctly sort the resources by their filename within Rodan, guaranteeing the same order for both images and transcript files and, thus, the processing of the images with their corresponding text.

![Ordering transcript resources by filename]({{site.baseurl}}/assets/ordering-transcript-by-filename.png)

In addition to the images and transcript texts, there are other resources needed to run this OMR end-to-end workflow (the list of all resources is outlined in [Digital Resources]({{site.baseurl}}/#digital-resources)):

* *Document-segmentation models* generated with the [patchwise trainer]({{site.baseurl}}/overview/document-analysis.html#patchwise-trainer).
* *Training data* (and *feature selection/weights* if needed) to use for the [non-interactive classifier]({{site.baseurl}}/overview/classification.html#non-interactive-classifier).
* *OCR model* for the [Text Alignment job]({{site.baseurl}}/overview/reconstruction-and-encoding.html#text-alignment).
* *MEI Mapping CSV file* for the [MEI Encoding job]({{site.baseurl}}/overview/generation-and-correction.html#score-generation--mei-encoding) generated with the [MEI Mapping Tool](https://github.com/DDMAL/mei-mapping-tool).

Unlike the images and transcript text files, these resources are constant between workflow runs. Therefore, the user just needs to assign one resource to the corresponding input port, and that resource will be used in all workflow runs generated.

![Annotated OMR workflow for digital resources]({{site.baseurl}}/assets/workflow-with-resize-annotated-for-digital-resources.png)
