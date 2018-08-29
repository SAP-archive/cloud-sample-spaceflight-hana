# Exercise 0: Prerequisite Steps

1. It is assumed that you already have at least a trial account on SAP's Cloud Platform.  If this is not the case, please follow the instructions for creating an [SCP trial account](https://cloudplatform.sap.com/try.html)
1. You must clone the base spaceflight data model found in the Git repository <https://github.com/SAP/cloud-sample-spaceflight-hana.git> into your Web IDE workspace.
    1. Log on to your SCP account, open Web IDE and select the development view  
        ![Development](./img/Icon_Development.png)
    1. Right-click on the top level `Workspace` and select Git -> Clone Repository
    1. Enter the link to the Git repo shown above and press Clone
    1. You now have a project in your Web IDE workspace called `cloud-sample-spaceflight-hana`
1. Right click on the `cloud-sample-spaceflight-hana` project name and select Build -. Build CDS
1. In the bottom right-hand corner of the Web IDE screen a column of three icons.  

    Click on the console ![Console](./img/Icon_Console.png) icon to display the console output of the CDS build tool.  

    You should output similar to the following:

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

    Make a note of the highlighted table names: `TECHED_FLIGHT_TRIP_AIRPORTS` and `TECHED_FLIGHT_TRIP_EARTHROUTES` as we will need to reference these later.

   
# \</exercise>
