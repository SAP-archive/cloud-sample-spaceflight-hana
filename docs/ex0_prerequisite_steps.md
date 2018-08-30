# Exercise 0: Prerequisite Steps


## 0.1 Log On To Your SAP Cloud Platform Account

1. The instructor will give you the logon details of the SCP account you can use for this exercise

1. In your SAP Cloud Platform Cockpit, display the details of your sub-account. This is done by using the horizontal navigation menu across the top of your screen.

    The general format of this information is:  
    `Region Name` / `Global Account Name` / `Sub-account Name` / `Space Name`

    1. Click on the sub-account name and you will see a screen showing the details of your sub-account
    1. Make sure the "Overview" option is selected from the menu down the left side of the screen, then to the right you will see a box labelled 'Cloud Foundry'
    1. In this box you will see the URL of your API Endpoint
    1. Make a note of this URL because you will need it to configure Web IDE

## 0.2 Configure Web IDE

1. From your SCP cockpit, select Services from the menu down the left side

1. Select "Full Stack Web IDE"

1. If this service is not yet enabled, press the Enable button and wait a few minutes

1. Click on "Go to service"

1. In Web IDE, select the preferences icon ![Preferences](./img/Icon_Preferences.png)
    1. From the "Workspace Preferences", select "Cloud Foundry"
    1. Enter the name of your Cloud Foundry API endpoint that you discovered in the previous section
    1. Select the Organization and Space names from the drop down lists
    1. Press the "Save" button at the bottom of the screen
    1. Under the heading "Builder", if the button says "Install Builder", then you must also click here to perform the installation.  The installation process could take a few minutes to complete.
    1. Once the installation has completed, press "Save" again

## 0.3 Clone the Git Repository

1. From the menu down the left side of the screen, select the Web IDE development view ![Development](./img/Icon_Development.png)

1. Right-click on the top level `Workspace` and select Git -> Clone Repository

1. Enter the Git repository name <https://github.com/SAP/cloud-sample-spaceflight-hana.git> and press Clone

1. You should now have a project in your Web IDE workspace called `cloud-sample-spaceflight-hana`

## 0.4 Build the Project

1. Right click on the `cloud-sample-spaceflight-hana` project name and select Build -> Build CDS

1. In the bottom right-hand corner of the Web IDE screen is a column of three icons.  

    Click on the console ![Console](./img/Icon_Console.png) icon to display the console output of the CDS build tool.  

    As the build tool runs, you should output similar to the following:

    <pre>
    10:55:20 (DIBuild) Build of "/cloud-samples-spaceflight-hana" in progress.  
    10:55:21 (DIBuild) [INFO] Injecting source code into builder...  
    [INFO] Source code injection finished[INFO] ------------------------------------------------------------------------
    npm install
    
    10:55:23 (DIBuild) up to date in 0.871s
    npm run build
    
    > spaceflight-model@0.1.0 build /home/vcap/app/.java-buildpack/tomcat/temp/builder/sap.cds.mta/builds/build-6976017343015870064/cloud-samples-spaceflight-hana
    > cds build --clean
    
    This is CDS 2.7.0, Compiler 1.0.32, Home: node_modules/@sap/cds
    
    10:55:25 (DIBuild) Compiled 'db/index.cds' to
      db/src/gen/.hdinamespace
      db/src/gen/BOOKINGSERVICE_AIRCRAFTCODES.hdbcds
      db/src/gen/BOOKINGSERVICE_AIRLINES.hdbcds
      db/src/gen/BOOKINGSERVICE_AIRPORTS.hdbcds
      db/src/gen/BOOKINGSERVICE_BOOKINGS.hdbcds
      db/src/gen/BOOKINGSERVICE_EARTHROUTES.hdbcds
      db/src/gen/BOOKINGSERVICE_ITINERARIES.hdbcds
      db/src/gen/BOOKINGSERVICE_PLANETS.hdbcds
      db/src/gen/BOOKINGSERVICE_SPACELINES.hdbcds
      db/src/gen/BOOKINGSERVICE_SPACEPORTS.hdbcds
      db/src/gen/BOOKINGSERVICE_SPACEROUTES.hdbcds
      db/src/gen/TECHED_FLIGHT_TRIP_AIRCRAFTCODES.hdbcds
      db/src/gen/TECHED_FLIGHT_TRIP_AIRLINES.hdbcds
      db/src/gen/<span style="background-color: yellow">TECHED_FLIGHT_TRIP_AIRPORTS</span>.hdbcds
      db/src/gen/TECHED_FLIGHT_TRIP_BOOKINGS.hdbcds
      db/src/gen/<span style="background-color: yellow">TECHED_FLIGHT_TRIP_EARTHROUTES</span>.hdbcds
      db/src/gen/TECHED_FLIGHT_TRIP_ITINERARIES.hdbcds
      db/src/gen/TECHED_SPACE_TRIP_ASTRONOMICALBODIES.hdbcds
      db/src/gen/TECHED_SPACE_TRIP_SPACEFLIGHTCOMPANIES.hdbcds
      db/src/gen/TECHED_SPACE_TRIP_SPACEPORTS.hdbcds
      db/src/gen/TECHED_SPACE_TRIP_SPACEROUTES.hdbcds
      
    10:55:28 (DIBuild) Compiled 'srv/index.cds' to
      srv/src/main/resources/edmx/BookingService.xml
      srv/src/main/resources/edmx/csn.json
      
      CDS return code: 0
      10:55:28 (DIBuild) ********** End of /cloud-samples-spaceflight-hana Build Log **********
    </pre>

    Make a note of the table names: `TECHED_FLIGHT_TRIP_AIRPORTS` and `TECHED_FLIGHT_TRIP_EARTHROUTES` as we will need to reference these tables later using their generated name, not the entity name seen in the `db/flight-model.cds` file.

1. Deploy the generated `.hdbcds` to HANA.  This is done by right-clicking on the `db` folder and selecting Build -> Build

1. As this deployment process runs, you will see several hundred lines of output in the console that will end with something similar to the following:

    <pre>
      Finalizing...
        Checking the uniqueness of the catalog objects in the schema "CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI2_1"...
        Checking the uniqueness of the catalog objects in the schema "CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI2_1"... ok
      Finalizing... ok
      Make succeeded (0 warnings): 33 files deployed (effective 62), 0 files undeployed (effective 0), 0 dependent files redeployed
     Making... ok
     Starting make in the container "CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI2_1" with 33 files to deploy, 0 files to undeploy... ok
    Deploying to the container "CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI2_1"... ok (9s 316ms)
    No default-access-role handling needed; global role "CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI2_1::access_role" will not be adapted
    Unlocking the container "CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI2_1"...
    Unlocking the container "CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI2_1"... ok (0s 0ms)
    Deployment to container CLOUD_SAMPLES_SPACEFLIGHT_HANA_SPACETRAVEL_HDI2_1 done [Deployment ID: none].
    (11s 483ms)<br>
    13:41:43 (DIBuild) ********** End of /cloud-samples-spaceflight-hana/db Build Log **********
    13:41:44 (Builder) Build of /cloud-samples-spaceflight-hana/db completed successfully.</pre>

1. You have now used the Core Data Services (CDS) tools to do three things:
    1. The "Build CDS" process compiles any `.cds` files found in the `db`, `srv` and `ui` folders into files suitable for building database tables in your chosen database - HANA in this case.  
       The result of this compilation process is the `.hdbcds` files found in the `db/src/gen/` folder
    1. The second Build process then deploys the `.hdbcds` files to HANA and builds the tables in your own database instance
    1. Using the instructions found in the JSON file `db/src/csv/Data.hdbtabledata`, the deploy process also pre-populates the HANA tables with data from the various CSV files found in the `db/src/csv` folder
   
# \</exercise>
