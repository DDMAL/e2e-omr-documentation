---
layout: default
title: Using Rodan
---

# What is Rodan?

Rodan is a workflow manager with various OMR and analysis jobs.
There are two main parts to it: the backend that runs the jobs and the client that the user interacts with.
As a user of Rodan, the most important aspect is the client, however there are parts of the backend that are useful to understand to help make using Rodan easier.

## Basic Concepts

* *Resource* - A resouce is a a file with an associated file type and possibly associated tags. The file may or may not exist depending on if the resource is a placeholder for a resource that should be created by a job.
* *Project* - A project in Rodan is a named group of resources, workflows, workflow runs, and run jobs on the Rodan server.
* *Job* - A job represents a Rodan job or task that takes resources as inputs and produces resources as outputs. It may or may not be interactive (requiring user input).
    * Note that a *run job* is an instance of a job with specific assigned inputs, outputs, and settings.
* *Workflow* - A workflow is a directed acyclic graph with jobs as nodes and connections between output and input ports as edges.
    * A *workflow run* is an instance of a workflow with assigned resources to input ports


## Normal Steps to Use Rodan

1. Upload resources. Ensure that the correct file type is detected for each resource (this is important!).
2. Tag resources if you want.
3. Create a workflow. Make sure to add/remove ports as necessary and update job settings. Give the workflow a useful name.
4. Assign resources to input ports. Use the filters to easily find resources.
    * If more than one resource is assigned to *one* input port, then the workflow will run twice, one run with each input.
    * If more than one resource is assigned to *many* input ports, then the workflow will run multiple times using the n-th input in the list for each input port.
5. Start the workflow.
6. Provide user input for interactive jobs. This is necessary when a run job's status is "waiting for user input."
7. Check and download output resources.
