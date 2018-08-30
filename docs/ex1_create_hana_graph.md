# Exercise 1: Create a HANA Graph

1. Right-click on the `db/src` folder and select New -> File  

    ![](./img/Ex1_001_Create_File.png)

1. Call the file `route.hdbgraphworkspace` and press Ok

    ![](./img/Ex1_002_Create_File.png)

1. This file is pre-populated with a template graph declaration that we will modify.  But before we can do this, we  need to understand what a graph is and then how to build one.

    ***Background***  
    A graph is built from two types of information:
    
    1. A set of points
    1. A set of lines drawn between these points
    

    The technical word for a point is a *"vertex"*, and the technical word for a line drawn between two points is an *"edge"*.  Therefore, when we build a HANA graph, we must answer the following questions:

    1. Which database tables can be used to supply the required edge and vertex information?
    1. What keys fields do these tables use?
    1. What association/s is/are there between the data in these two tables?

    In our case, our graph is going to represent all the airports and the direct flights that can be taken between those airports.  Therefore, the database table generated from the `Airports` entity will define the vertices, and the database table generated from the `Earthroutes` entity will define the edges.
    
    ***IMPORTANT***  
    Only tables with a single key field may be selected for use in a HANA Graph!


1. Down the left side of Web IDE window is a column of icons.  Click on the Database Explorer icon  

    ![Database Explorer](./img/Ex1_003_Database_Explorer.png)

1. Web IDE needs to connect to the HANA database instance that you built in the prerequisite steps, so in the pop-up window, click on Connect  

     ![Database Explorer](./img/Ex1_004_Database_Explorer.png)

1. If you have never used this tool before, then the Database Explorer will show you an empty list.  Click on the plus sign to connect to a database instance.

    ![Database List](./img/Ex1_005_Database_List.png)

1. Select a database type of "HDI Container" and then select the `cloud-samples-spaceflight-hana-xxxxxxxx` instance (where `xxxxxxxx` is some randomly generated identifier).

    If you have only ever built the HANA database from this exercise, then you will only see one name listed here. 

    ![Add Database](./img/Ex1_006_Add_Database.png)

1. Once the Database Explorer has connected to your HANA instance, you will be able to select "Tables".

    ![DB Tables](./img/Ex1_007_DB_Tables.png)

1. Click on the table `TECHED_FLIGHT_TRIP_EARTHROUTES` and you will now see this table's metadata displayed on the right.

    ![Earthroutes](./img/Ex1_008_Table_Earthroutes.png)
    
    Here we can see that this table's key field is called `ID`
    
1. Now display the table `TECHED_FLIGHT_TRIP_AIRPORTS`.

    ![Airports](./img/Ex1_009_Table_Airports.png)
    
    Here we can see that this table's key field is called `IATA3`

1. We now have enough information to declare our HANA Graph

    * The name of our graph is `ROUTES`
    * Each entry in table `TECHED_FLIGHT_TRIP_EARTHROUTES` represents a direct flight between two airports.  Therefore, this data can be used to draw the lines (or edges) of our graph.  
      This table has the key field `ID`
    * The points of our graph (or vertices) are defined in table `TECHED_FLIGHT_TRIP_AIRPORTS` that has the key field `IATA3`
    * The each direct flight listed in table `TECHED_FLIGHT_TRIP_EARTHROUTES` has a starting airport and a destination airport; therefore, the data in association `STARTINGAIRPORT_IATA3` defines where an edge starts, and the data in association `DESTINATIONAIRPORT_IATA3` defines where an edge stops.

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

    ![Build Graph](./img/Ex1_010_Build_Graph.png)

1. Go back to the database explorer by clicking on the icon down the left-hand side of the Web IDE screen !
1. [Database Explorer](./img/Icon_Database_Explorer.png)

1. Open the graph workspaces and select the `ROUTES` graph

    ![Graph Workspace](./img/Ex1_011_Graph_Workspace.png)

1. Click on the glasses icon in the top right corner to display the `ROUTES` graph

    ![Graph Workspace](./img/Ex1_012_Graph_Workspace.png)
    
    You can now see a graphical representation of all the direct flights listed in the `TECHED_FLIGHT_TRIP_EARTHROUTES` table.  In this case the graph is centred on Amsterdam-Schipol Airport in the Netherlands.
    
    ![Earthroutes Graph](./img/Ex1_013_Earthroutes_Graph.png)


# \</exercise>
