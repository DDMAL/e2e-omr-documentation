---
layout: default
title: Hints
nav_order: 3
---

# Tips & Tricks for OMR in Rodan

## Staff Size Height and Training

The Calvo classifier and trainer jobs perform differently based on the size of images.
For music with staves, the distance between staff lines in pixels (*staff size height*)
tends to be a predictor of how well it will perform. For instance with the original
CDN-Hsmu M2149.L4 images the staff size height is 64 px.
Values around this point may result in better classification and training results, but
note that this is not an optimized measure.

## Labelling Resources

Due to the number of resources the OMR process can produce (and the fact that over multiple
iterations many will have the same or similar names) resource labels can be used to keep
track of important resources. Labels can be added when resources are uploaded by using the
tag box next to the upload button, or after uploading by selecting the resource(s) and then
entering the labels in the detailed view to the right of the screen.

![Label input next to the upload button]({{site.baseurl}}/assets/label.png){:width="75%"}

Labels are separated by commas and can contain alphanumeric characters and many special characters.
The Labeler job can be used to add a label to resources produced in a workflow.
