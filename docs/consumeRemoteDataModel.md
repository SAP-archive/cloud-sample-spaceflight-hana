# How to Consume a CDS Data Model from a Different Repository

When you wish to consume a CDS data model defined in some other repository, then the following steps provide an outline of the approach.

## 1) Referencing an External Repository

The Web IDE project stored in the public SAP repository [cloud-sample-spaceflight](https://Git.com/SAP/cloud-sample-spaceflight) is packaged as an NPM module.  This means we can use NPM's normal module dependency mechanism to reference it. 

1. Ensure that the repository you wish to access is available on a public Git server.  In this particular example, we wish to consume the CDS data model found in the public SAP repository [cloud-sample-spaceflight](https://Git.com/SAP/cloud-sample-spaceflight).

1. Look into the definition of the CDS data model you wish to consume and make a note of its namespace.  In our case, the namespace is `teched.flight.trip` (see line 6 of [flight-model.cds](https://Git.com/SAP/cloud-sample-spaceflight/blob/master/db/flight-model.cds))

1. Edit the top-level `package.json` file to include an NPM dependency to the public repository.

    ```
    {
      "name": "spaceflight-model-hana",
      "version": "0.1.0",
      "dependencies": {
        "@sap/cds": "2.x"
      , "spaceflight-model" : "https://Git.com/SAP/cloud-sample-spaceflight"
      },
      "scripts": {
        "build": "cds build --clean && reuseTableData"
      },
      "license": "ISC"
    }
    ```

    You are free to choose any dependency name that is appropriate.  Here, we are using the name `spaceflight-model` followed by the public Git repository name.

## 2) Copying the Instructions to Load Tables with Data

One further detail here concerns the fact that in the original repository, the data model contains additional instructions to populate the tables with data at the time the database is built.

These instructions are stored in the file [`db/src/csv/Data.hdbtabledata`](https://github.com/SAP/cloud-sample-spaceflight/blob/master/db/src/csv/Data.hdbtabledata) of the original repository, and the actual data to be loaded into the database is stored in the various `.csv` files found in [the same folder](https://github.com/SAP/cloud-sample-spaceflight/blob/master/db/src/csv).

At the moment however, when a CDS data model from a remote Git repository is referenced, the instructions to populate the database tables (in any `.hdbtabledata` files) and the corresponding `.csv` files are not copied automatically.

To solve this problem, in the `scripts` object of `package.json` shown above, you can see that the `build` property contains not only the normal instruction to start the CDS compiler (`cds build --clean`), but also a further instruction to run a script called `reuseTableData`.

The `reuseTableData` command is a simply a JavaScript program in the referenced repository that has been defined as a binary (this is normal NPM configuration at work here).

The result of running this scripts is that the `.hdbtabledata` and `.csv` files are copied into our `db/src/gen` folder after the CDS compiler has run.

The name `reuseTableData` is declared in the `bin` object of the top-level [`package.json`](https://github.com/SAP/cloud-sample-spaceflight/blob/master/package.json) file of the repository we're referencing, and can then be used by any other NPM module that has declared a dependency to this module.

## 3) Using the Referenced Data Model

We have now done three things:

1. We have declared an NPM dependency for an external repository that contains a CDS Data Model
1. We have given this dependency the name `spaceflight-model`
1. We have invoked a workaround script called `reuseTableData` to copy the instructions for loading our database tables

What remains is now to reference the `spaceflight-model` in our own `.cds` file.

In the `db` folder of our project, there is a single [`index.cds`](../db/index.cds) file that contains the following single line:

```
using teched.flight.trip from 'spaceflight-model/db';
```

This is all that is needed to import the data model from the NPM dependency called `spaceflight-model`.

Notice that the information about the data model is referenced by pointing to the `/db` folder within the dependency.

As an aside, since the data model is contained within a single namespace, and we wish to consume everything in this data model, in this case it is unnecessary to provide the name of the namespace.  The following instruction will work equally well because this means ***use the entire data model*** found in the `spaceflight-model/db` folder:

```
using from 'spaceflight-model/db';
```
