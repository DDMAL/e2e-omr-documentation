---
layout: default
title: Symbol Classification
parent: Overview
nav_order: 2
---

The jobs in this section focus on extracting and interpreting components from the image layers produced during the
[document analysis stage]({{site.baseurl}}/overview/docuemnt-analysis). They are based on jobs in and use features
from [Gamera](https://gamera.informatik.hsnr.de/), a Python framework for building document analysis applications.

# Biollante

Rodan's [Biollante](https://github.com/DDMAL/biollante-rodan) is based on the Gamera job of the same name.
It is used to select or weigh the features of connected components for glyph classification.
From [the Gamera documentation](https://gamera.informatik.hsnr.de/docs/gamera-docs/ga_optimization.html#introduction):

> Feature selection is the technique of selecting a subset of good features out of a larger
> feature set to obtain the features which are suitable for the best possible classification.
> Feature weighting is a generalization of feature selection with real-valued weights between
> [0,1] instead of the binary values 0/1.
>
> Due to the fact that feature selection and weighting are NP-hard optimization problems,
> a complete and correct solution is not easy to determine. One way to get an approximate
> solution is by using genetic algorithms. Even though feature weighting is a generalization
> of feature selection and should thus be able to produce better classification results,
> practice has shown that feature selection produces better recognition rates in most cases.
> It is thus recommended to use feature selection rather than feature weighting [[Bolten2012]](https://gamera.informatik.hsnr.de/docs/gamera-docs/ga_optimization.html#bolten2012).

Biollante allows the user to set parameters for optimization and requires the user to set a *stop condition*.
Biollante goes back and forth between an interactive component, where the user is able to
set the parameters and stop conditions from a menu, and a non-interactive component where
the genetic algorithm is run.

### Input Ports

* **Classifier Data** - A file of connected components to optimize for kNN classification.

### Output Ports

* **Feature Weights/Selection** - The results of the feature weighting or selection that can be used in classification.

# Interactive Classifier

The [Interactive Classifier](https://github.com/DDMAL/Interactive-Classifier) is a browser-based interactive
graphical interface based on the Gamera job of the same
name and is used to train a music symbol model.
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

# Non-Interactive Classifier

The Non-Interactive Classifier job is based on the non-interactive Gamera classifier.
Once a suitable classification is achieved with the Interactive Classifier, the
outputs and classification feature weights can be shared with the
Non-Interactive Classifier to automatically process the connected component
sets from images of the same or a similar manuscript.
Instead of iteratively classifying glyphs and generating training data
like the Interactive Classifier, this job requires training data as an
input and then classifies a provided set of connected components *without*
user input.

## Non-Interactive Classifier Ports

### Input Ports

* **Training Data** - A file containing existing training data, for example from the Interactive Classifier job.
* **Connected Components** - A file containing the glyphs to be classified.
* *Feature Selection/Weights* - A file specifying the features to use or which weights to assign to the features.
This can be produced by [Biollante](#biollante).

### Output Ports

* **Classified Glyphs** - The glyphs classified as part of the job.
