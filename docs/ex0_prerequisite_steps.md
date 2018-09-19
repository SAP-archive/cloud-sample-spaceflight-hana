# Prerequisite Steps

There are 5 pre-requisite steps that need to be completed.  Please ensure that you have successfully completed all of these steps ***before*** proceeding with exercise 1.

1. [Log On To Your SAP Cloud Platform Account](./ex0.1.md)  
1. [Log On To Web IDE](./ex0.2.md)
1. [Configure Web IDE](./ex0.3.md)
1. [Create a New Project](./ex0.4.md)
1. [Compile and Deploy the Data Model to HANA](./ex0.5.md)

    
## Summary

After completing these steps you will have achieved the following:

1. You have logged on to your SAP Cloud Platform account 
1. You have logged on to and configured your Web IDE editor
1. Within Web IDE, you have created a new project (either manually or by cloning a Git repository)
1. Used the Core Data Services (CDS) tool in two stages to achieve three things:
    1. "Build CDS" invokes the CDS compiler to generate the `.hdbcds` found in the `db/src/gen/` folder.  
        (The CDS compiler actually does much more than this, but here we are only interested in the compilation of the data model)
    1. The second Build process (selected from the context menu of the `db` folder) then does two things:
        * Using the name of your Cloud Foundry Space configured in your Web IDE preferences, it sends the `.hdbcds` table information to the database builder tool installed in your CF Space.  The database Builder tool then generates the database tables in your HDI container
        * Using the configuration found in the JSON file `db/src/csv/Data.hdbtabledata`, the database builder then populates the database tables from the various `.csv` files in the `db/src/csv` folder
   
# \</prerequisite steps>
