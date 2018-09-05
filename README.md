# Space Travel Sample for TechEd 2018

## Data Model

This repository references the data model found in the SAP Git repository [cloud-sample-spaceflight](https://github.com/SAP/cloud-sample-spaceflight).

Please read the [flight model](https://github.com/SAP/cloud-sample-spaceflight/blob/master/docs/flightModel.md) and [space model](https://github.com/SAP/cloud-sample-spaceflight/blob/master/docs/spaceModel.md) documentation in the referenced repository in order to understand the data model's structure and how it should be used.

## How to Consume a CDS Data Model from a Different Repository

The details of how a CDS data model held in one Git repository can be referenced from another repository is documented [here](./docs/consumeRemoteDataModel.md).

## Exercises

In this session, we perform the following exercises:

1. [Prerequisite Steps](./docs/ex0_prerequisite_steps.md)
1. [Create a HANA graph](./docs/ex1_create_hana_graph.md)
1. [HANA Graph Showing the Shortest Path](./docs/ex2_shortest_path.md)
1. [Create a calculation view](./docs/ex2_no_stops_calculation_view.md) to show direct flights
1. [Create a calculation view](./docs/ex3_two_stops_calculation_view.md) to show flights needing no more than 2 stops


### For SAP TechEd
SAP TechEd will provide you with a full environment to develop this sample application.  The instructions below are only needed if you wish to run the application in your own account on SAP cloud platform.

### Development in SAP Cloud Platform Web IDE

SAP Web IDE Full-Stack access is needed. For more information, see [Open SAP Web IDE](https://help.sap.com/viewer/825270ffffe74d9f988a0f0066ad59f0/CF/en-US/51321a804b1a4935b0ab7255447f5f84.html).

Read the [getting started tutorial](https://help.sap.com/viewer//65de2977205c403bbc107264b8eccf4b/Cloud/en-US/5ec8c983a0bf43b4a13186fcf59015fc.html) to learn more about working with SAP Cloud Platform Web IDE.

Now clone your fork of this repository (*File -> Git -> Clone Repository*).

#### Develop, Build, Deploy

Build and deploy the DB module by choosing *Build* from the context menu of the db folder.


## Known Issues
None

## Support
This project is provided "as-is": there is no guarantee that raised issues will be answered or addressed in future releases.


## License
Copyright (c) 2018 SAP SE or an SAP affiliate company. All rights reserved.
This project is licensed under the Apache Software License, Version 2.0 except as noted otherwise in the [LICENSE](LICENSE) file.
