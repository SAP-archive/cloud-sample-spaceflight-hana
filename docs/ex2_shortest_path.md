# Exercise 2: HANA Graph Showing the Shortest Path

1. Starting from the HANA graph, we can now find the shortest route between two airports.  Click on the "Algorithm" icon `<>` in the top left corner then select "Shortest Path".  Now select the location codes of your starting and destination airports.  In this case, we wish to travel from Madrid to Las Vegas.

    ![Shortest Path](./img/Ex2_001_Shortest_Path.png)


    The path that is chosen may vary from the one shown here, but in this case, HANA is proposing that we should fly from Madrid to Las Vegas via London Heathrow.

    ![Via Panama](./img/Ex2_002_Via_Heathrow.png)
    
1. Whilst this might be a popular route, it does not cover the shortest distance.  So now let's add the `DISTANCE` field as the weight parameter

    ![Shortest Path](./img/Ex2_003_Shortest_Path.png)

    Ok, so now we are being routed through Chicago's O'Hare International Airport.
   
    ![Shortest Path](./img/Ex2_004_Via_Chicago.png)

# \</exercise>
