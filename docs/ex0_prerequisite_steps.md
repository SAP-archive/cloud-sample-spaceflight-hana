# Prerequisite Steps

The following prerequisite steps all relate to configuring SAP Web IDE in readiness for performing the first exercise.

Please ensure that you have successfully completed all of the following steps ***before*** proceeding with exercise 1.

1. [Log on to and Configure Web IDE](./ex0.1.md)
1. [Create a New Project](./ex0.2.md)
1. [Compile and Deploy the Data Model to HANA](./ex0.3.md)

    
## Summary

After completing these steps you will have achieved the following:

1. You have logged on to and configured your Web IDE editor
1. Within Web IDE, you have created a new project (either manually or by cloning a Git repository)
1. You have used the Core Data Services (CDS) compiler in two stages to achieve three things:
    1. "Build CDS" invokes the CDS compiler to generate all the `.hdbcds` files that now appear in the `db/src/gen/` folder.   (*The CDS compiler can actually do much more than this, but here, we are interested only in the compilation of the CDS data model*)
    1. The second Build process (selected from the context menu of the `db` folder) then does two things:
        * The CDS compiler connects to Web IDE Builder tool installed in your Cloud Foundry Space, and transfers over all the `.hdbcds` table information.  The Web IDE Builder tool then generates the database tables within an HDI container in the HANA DB
        * Using the configuration found in the JSON file [`db/src/csv/Data.hdbtabledata`](https://github.com/SAP/cloud-sample-spaceflight/blob/master/db/src/csv/Data.hdbtabledata) (found in the referenced repository), the Web IDE builder additionally populates the database tables using the data found in the various `.csv` files in the `db/src/csv` folder
   
# \</prerequisite steps>
