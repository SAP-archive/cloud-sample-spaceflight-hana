# CNA262: Developing SAP HANA Database Modules with an Application Programming Model

<!-- *********************************************************************** -->
<a name="description"></a>
## Description

The TechEd 2018 session CNA262 describes how to create a HANA Graph and Calculation views using the information supplied from the base data model found in the SAP Git repository [cloud-sample-spaceflight](https://github.com/SAP/cloud-sample-spaceflight).



Please read the [flight model](https://github.com/SAP/cloud-sample-spaceflight/blob/master/docs/flightModel.md) and [space model](https://github.com/SAP/cloud-sample-spaceflight/blob/master/docs/spaceModel.md) documentation in the referenced repository in order to understand the data model's structure and how it should be used.



<!-- *********************************************************************** -->
<a name="requirements"></a>
## Requirements

### For SAP TechEd 2018

For attendees of SAP TechEd 2018, the session instructors will provide you with access to the full environment in which to perform the exercises listed below.

### For General Use

Once SAP TechEd 2018 has finished, the exercises shown below will be available for use to anyone with the following accounts:

1. An SAP Cloud Platform (Cloud Foundry) account containing at least the services:

    * SAP HANA Database (Standard or Enterprise)
    * SAP HANA Schemas & HDI Containers (hdi-shared)

1. An SAP Cloud Platform (Neo) account that provides access to at least SAP Web IDE


### How to Consume a CDS Data Model from a Different Git Repository

The details of how a CDS data model held in one Git repository can be referenced from another repository are documented [here](./docs/consumeRemoteDataModel.md).



<!-- *********************************************************************** -->
<a name="exercises"></a>
## Exercises

Before starting any of the exercises, you must first complete [these pre-requisite steps](./docs/ex0_prerequisite_steps.md).

Once you have completed the pre-requisite steps, you are then ready to perform the following exercises:

1. [Create a HANA graph](./docs/ex1_create_hana_graph.md)
1. [Adapt the HANA Graph to show the shortest path between two airports](./docs/ex2_shortest_path.md)
1. [Create a calculation view showing direct flights](./docs/ex3_no_stops_calc_view.md)
1. [Create a calculation view showing routes requiring one stop](./docs/ex4_one_stop_calc_view.md)



<!-- *********************************************************************** -->
<a name="download"></a>
## Download and Installation

The exercises for this session are entirely Cloud-based.  Although you are at liberty to use any software editor you choose, the exercise instructions assume the use of SAP Web IDE; therefore based on this assumption, no software need be installed locally.



<!-- *********************************************************************** -->
<a name="configuration"></a>
## Configuration

None


<!-- *********************************************************************** -->
<a name="limitations"></a>
## Limitations

When using SAP Web IDE in the Firefox Quantum browser, a small number editor features are known not to function correctly; therefore it is recommended to use an up-to-date version of Google Chrome when working in SAP Web IDE.



<!-- *********************************************************************** -->
<a name="issues"></a>
## Known Issues

None so far...


<!-- *********************************************************************** -->
<a name="support"></a>
<a name="contributing"></a>
## Support and Contributing

This project is provided "as-is": there is no guarantee that raised issues will be answered or addressed in future releases.



<a name="license"></a>
## License

Copyright (c) 2018 SAP SE or an SAP affiliate company. All rights reserved.
This project is licensed under the Apache Software License, Version 2.0 except as noted otherwise in the [LICENSE](LICENSE) file.
