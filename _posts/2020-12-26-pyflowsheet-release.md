---
layout: post
title: "Pyflowsheet - Part 1: Initial Release"
author: "Jochen Steimel"
categories: journal
tags: [pyflowsheet python softwarengineering]
image: pyflowsheet_image.png
---


With a few days of quiet before christmas I began working on a new open-source project. Unlike my other projects, this is not focused on process simulation (but still related to process engineering). Initially I only wanted to test out how publishing Python packages on PyPI works, but after discussions with my colleagues I understood that the project could actually be worthwhile to pursue.

The new project, pyflowsheet, is concerned with the programmatic definition and rendering of Process Flow Diagrams (PFD) for process engineering documentation purposes. Typically these diagrams are manually created based on the Piping and Instrumentation Diagrams (P&ID) using CAD programs and are used for the documentation of the mass & energy balance of a process.

![Picture of a Block Flow Diagram of a simple process](https://nukleon84.github.io/ChemicalCode/assets/img/pyflowsheet_blockflowdiagram.png)

This Python package aims at automating this process as much as possible. The entire PFD can be created using the object model defined in the package, and the diagram can be annotated with matplotlib plots, tables and free text annotations. A few unit operation icons are already implemented and the user can build simple process flowsheets for thermal fluid separation processes (i.e. processes with columns, heat exchangers, vessels). Alternatively the Black-Box unit operation can be used to create Block-Flow-Diagrams, an even more abstract way of process documentation.
A block flow diagram for a basic chemical process

This project does not aim to completely replace CAD programs for the drawing of diagrams, but it is concerned with the rapid visual presentation of the results of simulation studies. So if anyone is tired of copying Excel Tables into Visio sheets, this package might be helpful. The user is able to change the color and dash-pattern of the streams and the colors of the unit operations to give visual hints to process conditions (material of construction, composition, temperature, pH taken from a simulation). In addition the package already supports basic geometric transformations like flipping and rotating the icons. Some unit operations support optional internals (like structured packing, stages/plates, tubes or a reactive bed)

![Picture of a flowsheet with optional styling](https://nukleon84.github.io/ChemicalCode/assets/img/pyflowsheet_styling.png)

One of my dreams is to one day be able to parse [DEXPI](https://dexpi.org/) documents and extract the major pieces of process equipment automatically to speed up the tiresome process of PFD generation.

The project is still in a very early alpha stage and will undergo some testing in the beginning of the next year. But I am already happy that I can share it with everybody who is willing to play around with it on Github and PyPI (pip install pyflowsheet). The API will probably change quite a bit depending on the initial feedback, so don't expect a stable product yet.

You can find some code examples on the github repository of the project!

https://github.com/Nukleon84/pyflowsheet

https://pypi.org/project/pyflowsheet/0.1.1/