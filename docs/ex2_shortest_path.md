# Exercise 2: HANA Graph Showing the Shortest Path

Starting from the HANA graph, we can now find the shortest journey between two airports for which we know there are no direct flights.

For instance, we happen to know that in our `Earthroutes` entity, there is no direct flight from Madrid to Las Vegas.  So lets use the HANA Graph to calculate firstly ***a*** route, and secondly, the ***shortest*** route.

1. Click on the "Algorithm" icon `<>` in the top left corner then select "Shortest Path".

1. Now select the location codes of your starting and destination airports.  In this case, we wish to travel from Madrid to Las Vegas.

    ***IMPORTANT***
    The values you enter in the Start and End Vertex fields are case-sensitive!

    ![Shortest Path](./img/Ex2_001_Shortest_Path.png)


1.  Here, HANA is proposing that we fly from Madrid to Las Vegas via London Heathrow.

    ![Via Panama](./img/Ex2_002_Via_Heathrow.png)
    
1. This is certainly a popular route, but it does not cover the shortest distance.  So now let's add the `DISTANCE` field as the weight column and press Apply again

    ![Shortest Path](./img/Ex2_003_Shortest_Path.png)

    Ok, so now we are being routed through Chicago's O'Hare International Airport (which is approximately 500Km shorter than flying via London Heathrow)

    ![Shortest Path](./img/Ex2_004_Via_Chicago.png)

# \</exercise>
