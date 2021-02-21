---
layout: page
title: Portfolio
---

## My Narrative Arc

Since the start of my industrial career in simulation, I was driven by the desire to understand the tools, algorithms and models that I was working with on a daily basis on a much deeper level. From this desire arose several medium-scale programming projects that I published on [Github](https://github.com/Nukleon84) to document my path and to share my experiences with others.

The journey started with steady-state process simulation, touched flowsheet drawing (and path routing) and concluded (so far) with a simple dynamic simulator capable of translating simple Modelica models into DAE problems that can be solved with a self-developed BDF2 solver with step-size adaptation. 

At this point I have touched a lot of the topics used in modern process simulation tools including, but not limited to:
* Automatic differentiation
* Equation-based modeling
* Block-Lower-Triangular form and Dulmage-Mendelsohn Decomposition
* Integration of thermodynamic calculations into a process simulator
* Flash algorithms
* Object-oriented hierarchical modeling
* Parsing of domain specific languages
* Instantiation of problem instances from abstract syntax trees


## OpenFMSL / MiniSim

[OpenFMSL](https://github.com/Nukleon84/OpenFMSL) (Open Flowsheet Modeling and Simulation Library) and [MiniSim](https://github.com/Nukleon84/MiniSim) were my attempt of developing an open-source process simulation engine. Based on a self-developed automatic differentiation engine, and using CSparse-net as the (linear) numerical solver, this tool allowed me to understand the architecture of an equation-oriented process simulator and how thermodynamics and balance equations could be implemented in an open-equation framework.

The first iteration of this project, OpenFMSL, had a simple IDE that allowed the user to use IronPython as the scripting language to assemble the process models.

![Writing Models in OpenFMSL](https://nukleon84.github.io/ChemicalCode/assets/img/IDE_Model.PNG)

![Viewing Results in OpenFMSL](https://nukleon84.github.io/ChemicalCode/assets/img/azeotropic_column.PNG)


In the second iteration of this project, MiniSim, the IDE was ditched in favor of Python bindings that allowed the user to directly use Jupyter Notebook as the frontend for setting up simulation models.

![Using MiniSim within Jupyter Notebooks](https://nukleon84.github.io/ChemicalCode/assets/img/MiniSim_jupyter.PNG)

## PyFlowsheet

The [pyflowsheet](https://github.com/Nukleon84/pyflowsheet) project is concerned with the programmatic definition and rendering of Process Flow Diagrams (PFD) for process engineering documentation purposes. Typically these diagrams are manually created based on the Piping and Instrumentation Diagrams (P&ID) using CAD programs and are used for the documentation of the mass & energy balance of a process.

With this project I wanted to learn how to publish Python packages on PyPI and focused on a smaller task, that could reasonably be finished in a finite amount of time. With Pyflowsheet, simple PFD drawings can be created from code and combined with different data sources (for example process simulations). But pyflowsheet itself contains no numeric calculations (except from the pathfinding needed for drawing the stream connections).

![Picture of a flowsheet with optional styling](https://nukleon84.github.io/ChemicalCode/assets/img/pyflowsheet_styling.png)



## Dynamic Modeling Framework

This project started as an offshot from MiniSim. Since I spend most of my academic time at the chair of Process Dynamics and Operation, I always had a latent interest in dynamic simulation. 

At the current state of development, the [DynamicModeling](https://github.com/Nukleon84/dynamicmodeling) project provides a minimal IDE that allows the user to input (simple) Modelica models, which are parsed and translated into a general DAE problem. The DAE problem is then solved with an implicit Backwards Euler Method (simpler alternatives are also available) after it has been partioned into block-lower-triangular form by Dulmage-Mendelsohn Decomposition. 

In this project, I learned a lot about the index problem, stiffness of DAEs, variable-step-size solvers and the numerical integration of implicit equation systems.

![Modeling a simple DAE](https://nukleon84.github.io/ChemicalCode/assets/img/modelingframework.PNG)


