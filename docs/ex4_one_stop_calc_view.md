# Exercise 4: Calculation View Showing Routes Requiring One Stop

## Before you Start...

Before starting this exercise, you must first have:

1. Compiled and deployed your Spaceflight data model to HANA.  See pre-requisite exercises [0.4](https://github.com/SAP/cloud-sample-spaceflight-hana/blob/master/docs/ex0.4.md) and [0.5](https://github.com/SAP/cloud-sample-spaceflight-hana/blob/master/docs/ex0.5.md) for details.
1. Completed [exercise 1](./ex1_create_hana_graph.md) in which you create a HANA Graph.  We will consume this graph during this exercise.

## Exercise Steps

1. Create a new Calculation View

    ![New Calculation View 1](./img/Ex3_New_Calc_View.png)
    
    Call this new Calculation View `route1stop` and make sure that the Data Category is `DEFAULT`.
    
1. Add a new Graph

    ![Add Graph](./img/Ex4_Add_Graph.png)

1. Add the graph we created in [exercise 1](./ex1_create_hana_graph.md) as the data source

    ![Add Data Source](./img/Ex4_Choose_Data_Src.png)

1. Select the details pane by clicking on the "Expand Details" icon ![Icon Detail Pane](./img/Icon_Detail_Pane.png) then add two new parameters

    ![Add Graph Parameter 1](./img/Ex4_Add_Graph_Param1.png)

1. For each item in the table below, repeat the following steps:

    | Name | Is Mandatory | Data Type | Length |
    |---|:-:|---|:-:|
    | `airportFrom` | ![Tick](./img/Icon_Tick.png) | `NVARCHAR` | 3
    | `airportTo` | ![Tick](./img/Icon_Tick.png) | `NVARCHAR` | 3

    ![Add Graph Parameter 2](./img/Ex4_Add_Graph_Param2.png)
    
    ![Add Graph Parameter 3](./img/Ex4_Add_Graph_Param3.png)
    

1. Now add an action.  From the drop-down menu on the right select "Pattern Matching" -> "Graphical Pattern Editor"

    ![Add Action](./img/Ex4_Add_Action.png)

    You will be asked to confirm the adjustment of the following fields

    ![Remove Fields](./img/Ex4_Remove_Fields.png)

1. If you remember from the first exercise, a graph as a visual representation of the connections (or edges) that exist between a set of points (or vertices).  Since we are planning air travel involving one stop, the vertices of our graph will be airports and the edges will be direct flights taken between these airports.

    Add the following three vertices, `Start`, `ChangeAt` and `Destination`

    Click on the `+` icon and drop a vertex on the left side of the editor area.

    ![Add Vertex 1](./img/Ex4_Add_Vertex1.png)

    Each vertex you add will be automatically named `V1`, `V2` or `V3` etc.  You will need to rename these vertices by clicking on the name and entering the new value.
    
    After you've added and renamed three vertices, your editor will now look like this:

    ![Add Vertex 2](./img/Ex4_Add_Vertex2.png)


1. The `Start` and `Destination` vertices must now have input parameters assigned to the following table fields:

    | Vertex Name | Attribute | Value |
    |---|:-:|---|
    | `Start` | `IATA3` | Input Parameter `airportFrom` 
    | `Destination` | `IATA3` | Input Parameter `airportTo` 

    For the `Start` and `Destination` vertices, click on the link in the Attribute column
    
    ![Add Vertex Attribute 1](./img/Ex4_Add_Vertex_Attr1.png)

    Select attribute `IATA3` and change the Value to "Input Parameter" and either `airportFrom` or `airportTo` as per the table above

    ![Add Vertex Attribute 2](./img/Ex4_Add_Vertex_Attr2.png)

1. Now create the graph edges by dragging the arrow from the `Start` vertex to the `ChangeAt` vertex.

    ![Add Vertex 3](./img/Ex4_Add_Vertex3.png)
    
    Repeat this process and connect the `ChangeAt` vertex to the `Destination` vertex
    
    These edges will automatically be named `E1`, `E2` etc.; however, for the sake of clarity, we will rename these edges `Leg1` and `Leg2` respectively.
    
    Your graph should now look like this:

    ![Add Edges](./img/Ex4_Add_Edges.png)
    
1. Now select Mapping from the Graph Detail Pane

    ![Graph Mapping 1](./img/Ex4_Graph_Mapping1.png)
    
    For each Data Source field shown in the table below, drag that field onto the Output Column area on the right

    | Data Source Type | Data Source Name| Field Name |
    |---|---|---|
    | Vertex | `Start` | `IATA3`
    | Vertex | `ChangeAt` | `IATA3`
    | Vertex | `Destination` | `IATA3`
    | Edge | `Leg1` | `ID`
    | Edge | `Leg1` | `DISTANCE`
    | Edge | `Leg1` | `AIRLINE_IATA2`
    | Edge | `Leg2` | `ID`
    | Edge | `Leg2` | `DISTANCE`
    | Edge | `Leg2` | `AIRLINE_IATA2`

    After this mapping is complete, if you collapse the individual data sources on the left of the Mapping screen, your mapping will look like this:
    
    ![Graph Mapping 2](./img/Ex4_Graph_Mapping2.png)


1. Insert a new projection in between `Projection` and `Graph_1` then connect `Graph_1` to this new projection

    ![Add Projection](./img/Ex4_Add_Projection.png)

    ![Connect Graph](./img/Ex4_Connect_Graph.png)


1. Map all the fields in the graph to the output of the projection.

    ![Projection Mapping 1](./img/Ex4_Projection_Mapping1.png)

    The projection mapping should now look like this:
    
    ![Projection Mapping 2](./img/Ex4_Projection_Mapping2.png)


1. In `Projection_1`, we now need to create three calculated columns.

    The purpose of these columns is to take the values from individual fields and merge them together in some way to form a more user-friendly value.  For instance, it would be helpful to have fields that display things like:
    
    1. A text string describing all the legs of the journey
    1. A text string showing which airline companies will be used on this journey
    1. The total journey distance
    
    
1. Add a new calculated column
    
    ![Calculated Column 1](./img/Ex4_Calc_Col1.png)

    Select the newly created column

    ![Calculated Column 2](./img/Ex4_Calc_Col2.png)
    
    Call this column `routeText`, change its type to be `NVARCHAR` and give it a length of `50`.
    
    ![Calculated Column 3](./img/Ex4_Calc_Col3.png)
    
    The purpose of this calculated column is to concatenate the airport location codes together.
    
    For example, we know that there is no direct flight from Madrid, Spain (`MAD`) to Las Vegas (`LAS`); therefore, we would need to change at somewhere like Chicago's O'Hare International Airport (`ORD`).  Therefore, we want such a route to be displayed as:
    
    `MAD -> ORD -> LAS`
    
    Since the SQL function `CONCAT` only takes two parameters, we need to use nested `CONCAT` calls to achieve the required result.  Now add the following expression:
    
    ```
    CONCAT(
      CONCAT(
        CONCAT(
          CONCAT(
            "IATA3"
          , ' -> '
          )
        , "IATA3_1"
        )
      , ' -> '
      )
    , "IATA3_2"
    )
    ```
    
    Press "Validate Syntax" to make sure there are no typos.
        
    ![Validate Syntax](./img/Ex3_Valid_Syntax.png)

1. Repeat the above steps for another calculated column called `airlineText`

    | Property | Value |
    |---|---|
    | Name | `airlineText`
    | Type | `NVARCHAR`
    | Length | 40
    
    The expression value creates a comma separated list of airline codes:
    
    ```
    CONCAT(
      CONCAT(
        "AIRLINE_IATA2"
      , ', '
      )
    , "AIRLINE_IATA2_1"
    )
    ```

1. Repeat the above steps for another calculated column called `totalDistance`

    | Property | Value |
    |---|---|
    | Name | `totalDistance`
    | Type | `INTEGER`
    
    The expression value simply adds up the distances of the two legs of the journey:
    
    ```
    "DISTANCE" + "DISTANCE_1"
    ```

1. Now connect `Projection_1` to the main Projection

    ![Connect Projection_1](./img/Ex4_Connect_Projection.png)

1. Select the main Projection and, following the order shown below, map the following fields from `Projection_1` through to the Output columns.

    Some of the output columns will need to be renamed.
    
    | Data Source | Output Column | Renamed to |
    |---|---|---|
    | `IATA3` | `IATA3` | `startingAirport` 
    | `IATA3_2` | `IATA3_2` | `destinationAirport` 
    | `routeText` | `routeText` | 
    | `airlineText` | `airlineText` | 
    | `totalDistance` | `totalDistance` | 
    | `ID` | `ID` | `leg1` 
    | `ID_1` | `ID_1` | `leg2` 
    
    Once complete, your mapping will look like this:

    ![Projection Mapping 3](./img/Ex4_Projection_Mapping3.png)

1. Now we are ready to build the calculation view

    ![Build View](./img/Ex4_Build_View.png)

1. After the view has built successfully, select Data Preview

    ![Data preview 1](./img/Ex4_Data_Preview1.png)

1. Enter two airports between which we know no direct flight exists.  For instance from Madrid (`MAD`) to Las Vegas (`LAS`), then press "Open Content" in the toolbar above.

    ![Data preview 2](./img/Ex4_Data_Preview2.png)

1. In the case of flying from Madrid to Las Vegas, you will see that 243 possible routes have been found.

    ![Data Preview 3](./img/Ex4_Data_Preview3.png)

1. Sort the Distance column into ascending order, and you will see that the shortest route from Madrid to Las Vegas is to fly via Chicago's O'Hare International airport (`ORD`)

    ![Sort Ascending](./img/Ex4_Sort_Ascending.png)

1. Multiple occurrences of the same route are shown because each leg of the journey is operated by a different combination of the airline companies.

    ![Data Preview 4](./img/Ex4_Data_Preview4.png)


[Previous Exercise](./ex3_no_stops_calc_view.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Next Exercise](./ex5_calc_view_as_odata_srv.md)


# \</exercise>
