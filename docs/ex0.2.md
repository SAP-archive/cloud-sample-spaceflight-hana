<a name="top"></a>

# Prerequisite : Log on to and Configure Web IDE

In the previous prerequisite step, you logged on the SAP Cloud Platform and found the link to your specific instance of SAP Web IDE.

<a name="1.1"></a>

## 1.1: Log on to Web IDE

1. Log on using same the credentials you used in prerequisite step 1

    ![Web IDE logon](./img/Ex0_Web_IDE_Logon.png)

1. Once logged on, you will see the Web IDE welcome screen

    ![Web IDE welcome screen](./img/Ex0_Web_IDE_Welcome.png)

<a href="#top">Top</a>



   
<a name="1.2"></a>

## 1.2: Configure Web IDE's Connection to Cloud Foundry

1. From the "Workspace Preferences", select "Cloud Foundry"
1. Select the name of your Cloud Foundry API endpoint from the drop-down list.  For TechEd 2018, this is EU10
1. The values for Organisation and Space names will be selected for you since your user id is allocated only to one space within one organisation
1. Press the "Save" button at the bottom of the screen

    ***IMPORTANT***  
    Please ***do not*** press the button saying "Reinstall Builder"!  
    Firstly, this action should not be necessary, and secondly, with multiple users sharing the same Cloud Foundry Space, this action should only be performed once per ***Cloud Foundry Space***, not once per ***user***.

<a href="#top">Top</a>


<a name="1.3"></a>

## 1.3: Configure Web IDE Features

1. From the "Workspace Preferences", select "Features"
1. Ensure that all of the following features have been switched on.  

    ![SAP Cloud Platform Business Application Development Tools](./img/Ex0_Feature_CP_Bus_App_Dev.png)  
    ![SAP HANA Database Development Tools](./img/Ex0_Feature_HANA_DB_Dev.png)  
    ![SAP HANA Database Explorer](./img/Ex0_Feature_HANA_DB_Exp.png)  
    ![Tools for NodeJS Development](./img/Ex0_Feature_NodeJS_Dev.png)  

    After switching all these features on, you ***must*** press the "Save" button located at the bottom of the screen.  

    You must then restart Web IDE in order to activate these new features.
   
<a href="#top">Top</a>

# \</prerequisite>
