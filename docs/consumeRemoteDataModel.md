# How to Consume a CDS Data Model from a Different Git Repository

When you wish to consume a CDS data model defined in some other repository, then the following steps provide an outline of the approach.

## 1) Referencing an External Git Repository

The Web IDE project stored in the public SAP repository [cloud-sample-spaceflight](https://github.com/SAP/cloud-sample-spaceflight) is packaged as an NPM module.  This means we can use NPM's normal module dependency mechanism to reference it. 

1. Ensure that the repository you wish to access is available on a public Git server.  In this particular example, we wish to consume the CDS data model found in the public SAP repository [cloud-sample-spaceflight](https://github.com/SAP/cloud-sample-spaceflight).

1. Look into the definition of the CDS data model you wish to consume and make a note of its namespace.  In our case, the namespace is `teched.flight.trip` (see line 6 of [flight-model.cds](https://github.com/SAP/cloud-sample-spaceflight/blob/master/db/flight-model.cds))

1. Edit the top-level `package.json` file of your project to include an NPM dependency to the external repository.

    ```json
    {
      "name": "spaceflight-model-hana"
    , "version": "0.1.0"
    , "dependencies": {
        "@sap/cds": "2.x"
      , "spaceflight-model": "https://github.com/SAP/cloud-sample-spaceflight"
      }
    , "scripts": {
        "build": "cds build --clean && reuseTableData"
      }
    , "license": "ISC"
    }
    ```

    You are free to choose any dependency name that is appropriate.  Here, we are using the name `spaceflight-model` followed by the URL of the public Git repository.

## 2) Copying the Instructions to Load Tables with Data

One important detail here concerns the fact that in the original repository, the data model contains additional instructions to populate the tables with data at the time the database is deployed.

These instructions are stored in the file [`db/src/csv/Data.hdbtabledata`](https://github.com/SAP/cloud-sample-spaceflight/blob/master/db/src/csv/Data.hdbtabledata) of the external repository, and the actual data to be loaded into the database is stored in the various `.csv` files found in [the same folder](https://github.com/SAP/cloud-sample-spaceflight/blob/master/db/src/csv).

At the moment however, when a CDS data model from a remote Git repository is referenced, the instructions to populate the database tables (in any `.hdbtabledata` files) and the corresponding `.csv` files are not copied automatically.

To solve this problem, in the `scripts` object of `package.json` shown above, you can see that the `build` property contains not only the normal instruction to start the CDS compiler (`cds build --clean`), but also a further instruction to run a script called `reuseTableData`.

The name `reuseTableData` is declared in the `bin` object of the top-level [`package.json`](https://github.com/SAP/cloud-sample-spaceflight/blob/master/package.json) file of the external repository.  This means that the name `reuseTableData` is available in the path used by `npm install` and can now be referenced by any other dependant NPM module.

The result of running this script is that the `.hdbtabledata` and `.csv` files are copied from the `node_modules/spaceflight-model/db` folder into our `db/src/gen` folder.


## 3) Using the Referenced Data Model

We have now done three things:

1. We have declared an NPM dependency on an external repository that contains a CDS Data Model
1. We have given this dependency the name `spaceflight-model`
1. We have invoked a workaround script called `reuseTableData` to copy the instructions for loading our database tables into our local `db/src/gen` folder

What remains is now to reference the `spaceflight-model` in our own `.cds` file.

In the `db` folder of our project, there is a single [`index.cds`](../db/index.cds) file that contains the following single line:

```
using teched.flight.trip from 'spaceflight-model/db';
```

This is all that is needed to import the data model from the `db` folder inside the NPM dependency called `spaceflight-model`.

## 4) So What Happens When We Run the CDS Compiler?

When we now run the CDS compiler on our project, the following things will happen:

1. Selecting Build -> Build CDS from the project's top-level context menu invokes the `npm install` command
1. `npm` reads the top-level `package.json` file and discovers some dependencies
1. These dependencies are imported into the local project and appear as sub-folders underneath the `node_modules` folder.  This is now where the `spaceflight-model` module appears
1. `npm` then invokes the `scripts.build` command found in `package.json`.
    1. Firstly, this invokes the CDS compiler that transforms all the copied `.cds` files in the `db` folder into files suitable for building HANA database tables. (These files then appear in the `db/src/gen` folder)
    1. Secondly, the `reuseTableData` script is invoked to copy all the `.hdbtabledata` and `.csv` files from the `node_modules/spaceflight-model/db` directory into the local `db/src/gen` folder.

The data model has now been fully copied into our project and is ready to be deployed to HANA.

Deployment to HANA is achieved by right-clicking on the `db/` folder name, and selecting Build -> Build

## 5) By the Way...

As an aside, since our particular data model is contained within a single namespace, and we wish to consume everything in this data model, it is actually unnecessary to provide the name of the namespace in the `index.cds` file.

Instead, the following instruction will work equally well because this means ***use the entire data model*** found in the `spaceflight-model/db` folder:

```
using from 'spaceflight-model/db';
```
