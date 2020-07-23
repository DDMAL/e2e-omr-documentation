---
layout: default
title: Symbol Classification
parent: Overview
nav_order: 2
---

# Biollante

*TODO*

# Interactive Classifier

The [Interactive Classifier](https://github.com/DDMAL/Interactive-Classifier) is based on the Gamera job of the same
name and is used to iteratively classify glyphs using a training model.
The job iterates between an automatic classification stage of glyphs and a manual correction stage where the user is able to specify more glyphs as training data.

There is also a [basic usage tutorial](https://github.com/DDMAL/Interactive-Classifier/wiki/How-to-Use) on the project's wiki page.

## Interactive Classifier Ports

The job has the following input and output ports. Optional input ports are in *italics*.

### Input Ports

* **Preview Image** - An image to be used to in the preview window to show glyphs in context.
* **Connected Components** - A file containing the glyphs to be classified.
* *Training Data* - A file containing existing training data, for example from a previous Interactive Classifier job.
* *Feature Selection/Weights* - A file specifying the features to use or which weights to assign to features. This can be produced by [Biollante](#biollante).
* *Class Names* - A file that defines preexisting classes and subclasses for classification.

### Output Ports

* **Training Data** - The training data generated during the manual classification stage.
* **Class Names** - The class and subclass names used during classification.
* **Classified Glyphs** - The glyphs classified as part of the job.

# Classifier

*TODO*
