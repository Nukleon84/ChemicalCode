---
layout: post
title: "Process Simulation in the Cloud: Opportunity or a Castle in the Air?"
author: "Jochen Steimel"
categories: journal
tags: [process-engineering cloud container simulation]
image: simulation_in_the_cloud_title.png
---

# Introduction

Recently the [DECHEMA e.V.](https://dechema.de/) published the position paper [Process Simulation – Fit for the future?](https://dechema.de/en/Positionspapier_Prozesssimulation). This document was written by the ProcessNet working committee [Process Simulation, Process Synthesis and Knowledge Processing](https://processnet.org/Fachgemeinschaften/Prozess__+Apparate_+und+Anlagentechnik/Modellgest%C3%BCtzte+Prozessentwicklung+und+_optimierung.html) (part of the larger PAAT community), of which I am a member of. This group presents a cross-section of the German chemical industry and academia for the field of computer-aided-process engineering.

The paper is targeted at *software vendors*, *academic program designers* and *decision makers in EPC and owner-operator companies* in the chemical and pharmaceutical industry.

Although my contribution to the whitepaper was rather small, I am very happy with the result and would like to use this article to share my personal thoughts on some of the addressed topics.

The position paper is driven by the following question 
> Process simulation is already one of the most important tools in process development, operation and optimisation in the chemical, biotechnology and pharmaceutical industries. But are the existing tools sufficient to meet the demands of the digitalised industry?

If you know my writings, you will remember that this topic is very dear to my heart. There is a lot I could write about this position paper and I will publish smaller articles as soon as I can find time to work on them. Today, I will be talking about an important topic: cloud-native simulation.

# Cloud-native process simulation

One of the topics that in my opinion was not formulated explicitly enough is the need for cloud-native software. While several vendors already proclaim to sell cloud-ready software we are still far away from truly cloud-native process simulation software. Just being able to run the Windows fat client on a virtual desktop is not cloud computing in my book.

For me, cloud-native software offers the following features:
* **Platform-independence:** The backbone of the cloud runs on Linux, and we need process simulator runtimes that can work without a GUI, unattended and on different operating systems than Microsoft windows (i.e. Linux).
* **Web-based user interfaces:** The user needs to be able to connect to the remote instance of the simulation server to check the status, retrieve results or change the model. Fat-clients are okay for the actual building of the model, but for deployed use cases simple GUIs that just support the right number of features are needed.
* **Light-weight deployment:** When we talk about deployed models, users cannot be asked to install several gigabytes for the full simulator suite every time. They need smaller runtimes, preferably optimized for use in *containerized* environments. Also, the requirements for RAM need to be brought down, so the users can deploy models on smaller instances (promoting scalability). They need scripted installers that can reliably deploy the software on any target machine.
* **Attractive licensing schemes:** If companies want to truly leverage the power of deployed models, they need schemes for scaling: when the users want to deploy a model for each column in their many sites, they cannot be asked to pay for a whole engineering station license. Vendors need to find the right balance between rightful monetarization and a non-existent business case.
* **Open interfaces:** To communicate with the simulators running in containerized instances, users need standard protocols (HTTP, REST, gRPC, …) that allow them to send and receive large amounts of data quickly. Deployed models rely on a multitude of external data source (SAP, data historians, OPC servers), so a standardized connectivity layer is needed.
* **Powerful scripting:** To achieve a comfortable level of convergence for unattended simulations, users and model writers need powerful scripting interfaces that allow them to switch off parts of the flowsheet, reinitialize and solve partial problems, instead of hoping that the internal solver will make the jump from an arbitrary initial condition to whatever the external sources require at the moment. Many simulators offer COM servers, but for distributed systems this old technology will not be enough anymore. Internal REST APIs could be part of the solution.

# A Use-Case Idea
For use-cases I currently have the most hopes in steady-state soft-sensors, as the technology is mature enough to “survive” in the wild and give results reliably. These plant-wide soft-sensors would be used for calculating mass- and energy-balances and calculate equipment internals like temperature and pressure profiles in columns or reactors. Data reconciliation may be used to improve the plant-to-model-match. 

Of course steady-state detection would be needed for such soft-sensors, but generally accepted methods exist to scan a timeseries for stationarity, and allow only those datapoints to be sent to the simulation server.

The results of these models could be used to help plant decision makers improve operating conditions (manually, I am not talking about RTO here), or to provide virtual sensor inputs for predictive maintenance models. So far the business opportunities are not very clear to me, which makes it even more important that users can experiment without huge upfront costs or commitment.

Granted, some companies already offer model deployment in a way or another, but proprietary technology, data formats and restrictive licensing schemes are in my opinion the biggest hurdles to use those features on a wider scale.

# Summary

So, what are your thoughts on the position paper, and by extension, my thoughts? 
* What problems would you like to see tackled by the software vendors and academic researchers.
* What opportunities do you see for the development of process simulation in the future?
* What features would you like to see in modern simulators?
* Do you think cloud-computing will be a major factor in process simulation? 

Feel free to contact me on my [LinkedIn profile](https://www.linkedin.com/in/jochen-steimel/) to share your thoughts with me. I am always open for a chat about process simulation.
