  ![](images/ml/001.png)

# Oracle Autonomous Data Warehouse Machine Learning Demo Lab

## Introduction
Estimated time to complete: ~45 minutes

In this lab you will assume the persona of Heather, the data scientist/ML expert for Alphaoffice. Heather has spent most of her time over the past couple of years extracting and preparing data for analysis. The large volumes of data need extracting and processing mean she spends most of her time waiting for jobs to finish and very little of her time analyzing the data. Demands from marketing are forcing a new approach whereby the data remains in the data warehouse and is processed there. The alternative cloud solution is more complex, and has no direct out of the box processes to analyze the data in place. She started taking a look at Oracle, and found the simple SQL commands in ADWC are familiar, and execute extremely fast, leveraging all the performance features of the platform. Further once she is done csan can apply the learning models to incoming data on the fly, and allow end user analysts to immediately see mining results. This drastically reduces the cycle of data preparation, analysis, and publishing. It also means there is no change to analysis/reporting Data Visualization toolset that users are familiar with.

An added benefit is the ability to use a new open source Apache Zeppelin based collaboration environment where she can work with others on the team in real time, annotating ML steps and combining the processing and documentation in one place. Since we are going to use Oracle ML interface, much of the lab will be done in that interface. For more information on which Machine Learning Algorithms are supported see Oracle Advanced Analytics documentation.

You will begin your machine learning development journey by importing an Apache Zeppelin Notebook into Oracle Machine Learning and then use ADWC's Machine Learning to predict customer good credit given detailed demographic information.

## Objectives

- Get hands-on with Oracle's Autonomous Data Warehouse Machine Learning
- Explore the Apache Zeppelin notebook interface
- Create an Attribute Importance Model using the DBMS_Predictive_Analytics.Expain procedure
- Identify customer attributes that are the best predictors of good credit
- Predict credit worthiness of new customers

## Instructions

### Download your workshop files.  

**Click to Download**

[install.zip](https://dgcameron.github.io/adwcml_oow/install.zip)

  ![](images/ml/001.1.png)

- Save and extract the files.

  ![](images/ml/001.2.png)

### Retrieve your login URL.

- To support this workshop we have pre-provisioned several Autonomous Data Warehouse instances, and have allocated userids user01 - user80.  Use your assigned login url and userid to sign into Machine Learning.  The password for everyone will be `xxxxxx`.  Locate your assigned Machine Learning URL and click on that.

|Machine Learning userid|Machine Learning Login|
|---|---|
|user01 - user05|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML1&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|
|user06 - user10|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML2&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|
|user11 - user15|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML3&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|
|user16 - user20|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML4&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|
|user21 - user25|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML5&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|
|user26 - user30|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML6&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|
|user31 - user35|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML7&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|
|user36 - user40|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML8&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|
|user41 - user45|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML9&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|
|user46 - user50|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML10&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|
|user51 - user55|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML11&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|
|user56 - user60|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML12&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|
|user61 - user65|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML13&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|
|user66 - user70|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML14&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|
|user71 - user75|<a href="https://adb.us-phoenix-1.oraclecloud.com/omlusers/login.html?tenant=OCID1.TENANCY.OC1..AAAAAAAANH7SZ33FSOTVGGJY7VAY5MDCUFXV5YHLVVLJU7WCLOJR5GWQXQ7Q&database=OML15&redirect_uri=https://adb.us-phoenix-1.oraclecloud.com/omlusers/api/oauth2/v1/login" target="_blank">Machine Learning Login URL</a>|

### Log into Oracle Machine Learning.

- Enter your userid with password `xxxxxxx`.

- Note that you can run sql statements, run sql scripts (pl/sql blocks), work with Apache Zeppelin Notebooks, schedule notebooks to run at specified times, and review examples of notebooks.  Note that the examples are review only.  To work with the examples you need to first export and then import them to be updatable.  Lets take a look at the examples.  Select `Examples` to review sample content.

  ![](images/ml/003.png)

- Select `Attribute Importance` and review the notebook content.  We will not do anything with this notebook, but will be using the `Attribute Importance` model later on.

  ![](images/ml/004.png)

- When you first open a notebook you need to set the interpreter binding and save this setting.  This is only done once.  Any of the binding settings will work, but be sure to select `OMLXX_low` setting.  This is setting the resources (memory, low, medium, high).  Then scroll down to review the example content.

  ![](images/ml/005.png)

  ![](images/ml/005.1.png)

- Now lets take a look at a pre-built notebook that has more meaningful predictive content.  Select the menu in the upper left and select `Notebooks`.

  ![](images/ml/006.png)

  ![](images/ml/007.png)

- We will be importing a pre-built notebook, and using this for the remainder of the lab.  Select `Import`.

  ![](images/ml/008.png)

- Go to where you extracted the `install.7z` file and select all the json files.  We will be primarily working with the `Credit Score Predictions` notebook, but others may be of interest.

  ![](images/ml/009.png)

- Select the `Credit Score Predictions` notebook.

  ![](images/ml/010.png)

- Before you start working with the `Credit Score Predictions` (or any of the others) you need to set the interpreter binding.  Click on the gear icon.

  ![](images/ml/011.png)

- Select the OMLXX_low interpreter binding and then Save.

  ![](images/ml/012.png)

The rest of this lab will be done interactively in the notebook.  The following area just screen shots for your convenience.

![](images/ml/013.png)

![](images/ml/014.png)

![](images/ml/015.png)

![](images/ml/016.png)

![](images/ml/017.png)

![](images/ml/018.png)

![](images/ml/019.png)

![](images/ml/020.png)

![](images/ml/021.png)

![](images/ml/022.png)

![](images/ml/023.png)

![](images/ml/024.png)

![](images/ml/025.png)

![](images/ml/026.png)