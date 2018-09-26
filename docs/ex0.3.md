<a name="top"></a>

# Prerequisite 3: Compile and Deploy the CDS Data Model to HANA

The data model used by this project is imported from a different Git repository: <https://github.com/SAP/cloud-sample-spaceflight.git>.

***WARNING***  
The information imported from the remote Git repository does ***not*** immediately give us a working HANA database.  Instead, we are importing the various CDS files that define the what the database tables will look like when deployed to HANA.  Therefore, after importing the data model, we must perform two steps:

1. Compile the data model
1. Deploy the compiled database definition to an HDI Container.  This step has been configured so that it will additionally populate the database tables

> ***IMPORTANT***  
> Currently, the CDS compiler can only create files suitable for deployment to a HANA database.  
> Compiling `.cds` files for other target databases is planned for the future.



<a name="3.1"></a>

## 3.1 Compile the Data Model

If you have not changed the default configuration for SAP Web IDE, then this step will already have been performed when you saved your changes to the `db/data-model/.cds` file at the end of [prerequisite step 2.2](./ex0.2.md#2.2).

The purpose of the CDS compiler is firstly to transform the database agnostic `.cds` files into a form suitable for deployment to HANA. For more information on the behaviour of the CDS compiler, please read this [brief overview](https://github.com/SAP/cloud-sample-spaceflight/blob/master/docs/cdsCompile.md)


1. If you wish to recompile all the CDS files in your project, right-click on the `cloud-sample-spaceflight-hana` project name and select Build -> Build CDS.
    
1. In the bottom right-hand corner of the Web IDE screen is a vertical menu.  

    Click on the console ![Console](./img/Icon_Console.png) icon to display the console output of the CDS build tool.  

    As the build tool runs, you will see output similar to the following:

    ```
    16:15:04 (DIBuild) Build of "/cloud-sample-spaceflight-hana" in progress.
    16:15:04 (DIBuild) [INFO] Injecting source code into builder...
    [INFO] Source code injection finished
    [INFO] ------------------------------------------------------------------------
    npm install
    16:15:18 (DIBuild) added 42 packages from 27 contributors in 13.528s
    npm run build
    
    > spaceflight-model-hana@0.1.0 build /home/vcap/app/.java-buildpack/tomcat/temp/builder/sap.cds.mta/builds/build-6961006870400751194/cloud-sample-spaceflight-hana
    > cds build --clean && reuseTableData
    
    This is CDS 2.8.0, Compiler 1.1.1, Home: node_modules/@sap/cds
    
    16:15:20 (DIBuild) Compiled 'db/data-model.cds' to
      db/src/gen/.hdinamespace
      db/src/gen/TECHED_FLIGHT_TRIP_AIRCRAFTCODES.hdbcds
      db/src/gen/TECHED_FLIGHT_TRIP_AIRLINES.hdbcds
      db/src/gen/TECHED_FLIGHT_TRIP_AIRPORTS.hdbcds
      db/src/gen/TECHED_FLIGHT_TRIP_BOOKINGS.hdbcds
      db/src/gen/TECHED_FLIGHT_TRIP_EARTHROUTES.hdbcds
      db/src/gen/TECHED_FLIGHT_TRIP_ITINERARIES.hdbcds
      db/src/gen/TECHED_SPACE_TRIP_ASTRONOMICALBODIES.hdbcds
      db/src/gen/TECHED_SPACE_TRIP_SPACEFLIGHTCOMPANIES.hdbcds
      db/src/gen/TECHED_SPACE_TRIP_SPACEPORTS.hdbcds
      db/src/gen/TECHED_SPACE_TRIP_SPACEROUTES.hdbcds
      
    ---------------------------------------------------------------------------------
    
    Found 0 locally declared tables
    
    Importing CSV files based on table data from 'node_modules/spaceflight-model/db/src/csv/Data.hdbtabledata'
      db/src/gen/spaceflight-model/db/src/csv/airlines.csv
      db/src/gen/spaceflight-model/db/src/csv/aircraftcodes.csv
      db/src/gen/spaceflight-model/db/src/csv/airports.csv
      db/src/gen/spaceflight-model/db/src/csv/earthroutes.csv
      db/src/gen/spaceflight-model/db/src/csv/spaceports.csv
      db/src/gen/spaceflight-model/db/src/csv/astrobodies.csv
      db/src/gen/spaceflight-model/db/src/csv/spaceflightcompanies.csv
      db/src/gen/spaceflight-model/db/src/csv/spaceroutes.csv
      db/src/gen/spaceflight-model/db/src/csv/itineraries.csv
      db/src/gen/spaceflight-model/db/src/csv/Data.hdbtabledata
    
    CDS return code: 0
    
    16:15:20 (DIBuild) ********** End of /cloud-sample-spaceflight-hana Build Log **********
    ```

    Make a note of the table names `TECHED_FLIGHT_TRIP_AIRPORTS` and `TECHED_FLIGHT_TRIP_EARTHROUTES`.  In a later exercise, we will need to reference these tables by their generated name and not by the entity names used in the referenced [`flight-model.cds`](https://github.com/SAP/cloud-sample-spaceflight/blob/master/db/flight-model.cds) file.

1. As a result of running the CDS Compiler, you will now find that your `db/src/gen` folder has been populated.  These are the files that now need to be deployed to your HANA HDI Container.

    ![Generated files](./img/Ex0_Gen_Folder.png)

    Notice also that there is a subdirectory called `spaceflight-model`.  This folder was created by running the additional build script `reuseTableData` (see line 9 in [package.json](https://github.com/SAP/cloud-sample-spaceflight-hana/blob/master/package.json)) and contains all the `.csv` files needed to populate the database tables after they have been deployed.

<a href="#top">Top</a>



<a name="3.2"></a>

## 3.2 Deploy the Compiled Data Model to HANA

Now that all the `.cds` files have been transformed into `.hdbcds` files, we can deploy this definition to HANA.

1. Before starting the deploy process, it is worth first clearing the console output.  To do this, Select View -> "Clear Console" from the Web IDE menu running across the top of the screen

1. In order to deploy the generated `.hdbcds` to your HDI Container, right-click on the `db` folder and select Build -> Build

1. As this deployment process runs, you will see several hundred lines of output in the console that will end with something similar to the following:

    ```plain_text
      Finalizing...
        Checking the uniqueness of the catalog objects in the schema "CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI2_1"...
        Checking the uniqueness of the catalog objects in the schema "CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI2_1"... ok
      Finalizing... ok
      Make succeeded (0 warnings): 33 files deployed (effective 62), 0 files undeployed (effective 0), 0 dependent files redeployed
     Making... ok
     Starting make in the container "CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI_1" with 33 files to deploy, 0 files to undeploy... ok
    Deploying to the container "CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI_1"... ok (9s 316ms)
    No default-access-role handling needed; global role "CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI_1::access_role" will not be adapted
    Unlocking the container "CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI_1"...
    Unlocking the container "CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI_1"... ok (0s 0ms)
    Deployment to container CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI_1 done [Deployment ID: none].
    (11s 483ms)<br>
    13:41:43 (DIBuild) ********** End of /cloud-samples-spaceflight-hana/db Build Log **********
    13:41:44 (Builder) Build of /cloud-samples-spaceflight-hana/db completed successfully.
    ```

<a href="#top">Top</a>



<a name="3.3"></a>

## 3.3 Remember Your HDI Container Name

Although it is not essential, it is useful to make a note of the HDI Container name that is created when you deploy your CDS Data Model to the shared HANA DB.  You might need to know this when you later connect using the Database Explorer tool in Web IDE.

Scroll to the very top of the console and you will see output similar to the following:

```plain_text
14:29:44 (DIBuild) Build of "/cloud-sample-spaceflight-hana/db" in progress.
14:29:44 (DIBuild) Service provisioning for module: '/db'
Created the 'cloud-sample-spaceflight-hana-spacIqaQmbX9T166/dki' instance of the 'hana' service type for the 'spacetravel-hdi' resource.
[INFO] Injecting source code into builder...
[INFO] Source code injection finished
[INFO] ------------------------------------------------------------------------
Your module contains a package.json file, it will be used for the build.
14:29:46 (DIBuild)
> deploy@ postinstall /home/vcap/app/.java-buildpack/tomcat/temp/builder/hdi-builder/builds/build-2330334266323447403/cloud-sample-spaceflight-hana/db
> node conditionalBuild.js
```

Look at the third line that starts with `Created the 'cloud-samples-spaceflight-hana-xxxxxxxx' instance` (where `xxxxxxxx` is some randomly generated identifier).

This is the name of your HDI Container within the shared HANA database and into which your database tables have been deployed.


<a href="#top">Top</a>
   
# \</prerequisite>
