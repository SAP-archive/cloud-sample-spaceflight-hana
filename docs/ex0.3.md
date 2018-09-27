<a name="top"></a>

# Prerequisite 3: Create a New Project

You have two options for how to create a new project.  You can either:

* Create the project manually, or you can
* Clone the Git repository containing the exercise template

However, irrespective of how you create the project, you will still need to build and deploy it to your HANA HDI Container in Cloud Foundry.  For both approaches, you will need to select the development perspective in Web IDE.

From the menu down the left side of the screen, select the Web IDE development perspective ![Development](./img/Icon_Development.png)

<a name="3.1"></a>

## 3.1: Create the Project by Cloning the Git Repository

1. Right-click on the top-level Workspace item and select Git -> Clone Repository

    ![Development](./img/Ex0_Git_Clone.png)

1. Enter the Git repository name <https://github.com/SAP/cloud-sample-spaceflight-hana.git> and press Clone

    ![Enter Repository Name](./img/Ex0_Repo_Name.png)

1. When the clone completes, you will see a success message in the top right corner of the screen and you will also have a project in your Web IDE workspace called `cloud-sample-spaceflight-hana`

    ![Clone Complete](./img/Ex0_Clone_Completed.png)


You have now finished this pre-requisite step.

<a href="#top">Top</a>



<a name="3.2"></a>

## 3.2: Create the Project Manually

1. Right-click on the top level `Workspace` and select New -> Project From Template

    ![New project from template](./img/Ex0_New_Project.png)

1. Select the SAP Cloud Platform Business Application template and press Next

    ![SAP Cloud Platform Business Application template](./img/Ex0_CP_Bus_App.png)

1. Enter the name `cloud-sample-spaceflight-hana` and press Next

1. Select a HANA Database and click Finish

    ![Application Id](./img/Ex0_Inc_Mods.png)

1. Edit the `mta.yaml` file.  Replace the contents of this file with the text below:

    ```yaml
    ID: cloud-sample-spaceflight-hana
    _schema-version: "3.1"
    version: 1.0.0
    
    modules:
      - name: spacetravel-db
        type: hdb
        path: db
        parameters:
          memory: 256M
          disk-quota: 256M
        requires:
          - name: spacetravel-hdi

    resources:
      - name: spacetravel-hdi
        type: com.sap.xs.hdi-container
        properties:
          hdi-container-name: ${service-name}
    ```

    ***Warning***  
    When editing a `.yaml` file, remember that the properties are indentation sensitive.  Removing or changing the indentation shown above can introduce syntax errors!

1. Edit `package.json` and replace the entire file with the following:

    ```json
    {
      "name": "spaceflight-model-hana"
    , "version": "0.1.0"
    , "dependencies": {
        "@sap/cds": "2.x"
      , "spaceflight-model": "https://github.com/SAP/cloud-sample-spaceflight"
      }
    , "scripts": {
        "build": "cds build --clean && reuseTableData"
      }
    , "license": "ISC"
    }
    ```

    Notice that an NPM dependency called `spaceflight-model` has been added.  This is how we can reference a CDS data model held in a separate Git repository.

    For a full explanation of what these instructions do, please read [How to Consume a CDS Data Model from a Different Git Repository](./consumeRemoteDataModel.md)

1. Finally, edit `db/data-model.cds`.

    Delete the only line in this file and replace it with:

    ```using teched.flight.trip from 'spaceflight-model/db';```
    
    At this point however, the syntax checker will inform you that this file contains a syntax error.
    
    This is a false error message and you don't have to worry about it!
    
    ![False syntax error](./img/Ex0_Syntax_Error.png)
    
    The error comes from the fact that the syntax checker has assumed the reference to `spaceflight-model` refers to some local CDS file in the current project, when in fact, it refers to the name of the NPM dependency we added into the `package.json` file above.  This name will not exist until ***after*** we have run the CDS Compiler.


<a href="#top">Top</a>
   
# \</prerequisite>
