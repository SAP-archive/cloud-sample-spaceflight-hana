# Exercise 1: Create a HANA Graph

Before we can build a HANA Graph, we need to understand what graphs are in general.

## What is a Graph?

A "graph" is a mathematical term for particular type of data visualisation, and is built from two types of information:
    
1. A set of points
1. A set of connections between these points.  These connections usually have some property that describes the strength (or "weight") of the connection

The technical word for a point is a *"vertex"*, and the technical word for the connection between two points is an *"edge"*.  Therefore, when we build a HANA graph, we must answer the following questions:

1. Which database table will supply the vertex information?
1. Which database table will supply the edge information?
1. What keys fields do these tables use?
1. What associations are there between these two tables?
1. What property (or properties) of the edge table can be used to describe the strength (or "weight") of connection?

In our case, our graph is going to visualise all the direct flights that can be taken between airports. Therefore, we can answer the first two questions:

> The edge information will be obtained from the database table generated from the `Earthroutes` entity  
> The vertex information will be obtained from the database table generated from the `Airports` entity

Notice here that the data sources are not the `Airports` or `Earthroutes` entities themselves, but rather the database tables that are generated from these entities.  

Do you remember from section 0.5.1 of the [prerequisites steps](./ex0_prerequisite_steps.md) you were asked to make a note of the table names `TECHED_FLIGHT_TRIP_AIRPORTS` and `TECHED_FLIGHT_TRIP_EARTHROUTES`?  In order to define the HANA graph, we need to know these two table names.

This is an important distinction because we need to understand how the CDS compiler transforms an entity name in a `.cds` file into a database table name (defined in a `.hdbcds` file).

In order to answer the remaining questions, we need to look inside the SQL table definitions.

***IMPORTANT***  
Only tables with a single key field may be selected for use in a HANA Graph!


## Exercise Steps

1. Right-click on the `db/src` folder and select New -> File  

    ![](./img/Ex1_Create_File.png)

1. Call the file `route.hdbgraphworkspace` and press Ok

    ![](./img/Ex1_Filename.png)

1. This file is pre-populated with a template graph declaration that we will modify; however, before we can modify this file, we need to know which fields from which tables will be used

1. Right-click on the `db` folder and select "Open HDI Container".  In order for this step to work correctly, you must first have already compiled and deployed your data model to HANA (as detailed in the [prerequisites steps](./ex0_prerequisite_steps.md))

    ![Database Explorer](./img/Ex1_Open_HDI_Container.png)

1. The Database Explorer tool now opens and connects to your HDI Container.  Now select "Tables".

    ![DB Tables](./img/Ex1_DB_Tables.png)

1. Click on the table `TECHED_FLIGHT_TRIP_EARTHROUTES` and you will now see this table's metadata displayed on the right.

    ![Earthroutes](./img/Ex1_Table_Earthroutes.png)
    
    Here we can see that this table's key field is called `ID`.  There are also two other fields that contain the IATA location codes of the starting and destination airports.
    
1. Now display the table `TECHED_FLIGHT_TRIP_AIRPORTS`.

    ![Airports](./img/Ex1_Table_Airports.png)
    
    Here we can see that this table's key field is called `IATA3`

1. We now have enough information to declare our HANA Graph

    * The name of our graph is `ROUTES`
    * The edges of our graph correspond to the entries in table `TECHED_FLIGHT_TRIP_EARTHROUTES`
    * The vertices of our graph correspond to the entries in table `TECHED_FLIGHT_TRIP_AIRPORTS`
    * Table `TECHED_FLIGHT_TRIP_EARTHROUTES` has a key field called `ID` and table `TECHED_FLIGHT_TRIP_AIRPORTS` has a key field called `IATA3`
    * The each direct flight listed in table `TECHED_FLIGHT_TRIP_EARTHROUTES` has a starting airport and a destination airport; therefore, the data in association `STARTINGAIRPORT_IATA3` defines where an edge starts, and the data in association `DESTINATIONAIRPORT_IATA3` defines where an edge stops
    * The `DISTANCE` field in table `TECHED_FLIGHT_TRIP_EARTHROUTES` can be used to define the weight of the connection between two vertices

    <pre>
    GRAPH WORKSPACE "<span style="background-color: yellow">ROUTES</span>"
    
    EDGE TABLE "<span style="background-color: yellow">TECHED_FLIGHT_TRIP_EARTHROUTES</span>"
      SOURCE COLUMN "<span style="background-color: yellow">STARTINGAIRPORT_IATA3</span>"
      TARGET COLUMN "<span style="background-color: yellow">DESTINATIONAIRPORT_IATA3</span>"
      KEY COLUMN "<span style="background-color: yellow">ID</span>"
    
    VERTEX TABLE "<span style="background-color: yellow">TECHED_FLIGHT_TRIP_AIRPORTS</span>"
      KEY COLUMN "<span style="background-color: yellow">IATA3</span>"
    </pre>

1. Build the `.hdbgraphworkspace` file by right-clicking on the file name and selecting Build -> Build selected file

    ![Build Graph](./img/Ex1_Build_Graph.png)

1. Go back to the database explorer by clicking on the icon down the left-hand side of the Web IDE screen ![Database Explorer](./img/Icon_Database_Explorer.png)

1. Open the graph workspaces and select the `ROUTES` graph

    ![Graph Workspace](./img/Ex1_Graph_Workspace.png)

1. Click on the glasses icon in the top right corner to display the `ROUTES` graph

    ![Graph Workspace](./img/Ex1_Display_Graph.png)
    
    You can now see a graphical representation of all the direct flights listed in the `TECHED_FLIGHT_TRIP_EARTHROUTES` table.  In this case the graph is centred on Amsterdam-Schipol Airport in the Netherlands.
    
    ![Earthroutes Graph](./img/Ex1_Earthroutes_Graph.png)


# \</exercise>
