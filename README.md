# SAP_RE_C4RE_DI_DWC_Content
SAP_RE_C4RE_DI_DWC_Content SAP Data Intelligence (DI) solutions for
1. pipelines for creating tables and loading data (bulk) from *SAP Cloud for Real Estate API* into *SAP Data Warehouse Cloud* (DWC) entry layer and
2. addionally, a custom operator for generic parsing data and bulk load them into *SAP Data Warehouse Cloud* entry layer.

If you want to use these pipelines the following steps will be prerequisite:

## Connection Management:

•	Steps 1. and 2. from my blog post https://blogs.sap.com/2022/01/25/how-to-connect-sap-cloud-for-real-estate-to-sap-data-warehouse-cloud/ are already done from your side: you have created an inbound (to the SAP Cloud for Real Estate API) and outbound (to the SAP Data Warehouse Cloud) connection in SAP Data Intelligence Cloud with the Connection Management.

• Just a *hint* which credential you need from the service keys in the inbound connection from the SAP Cloud for Real Estate API:
  
  - Host: 			            <the ***api_base_uri*** from the service keys **without** 'https://'>
  - Protocol: 		          **HTTPS**
  - Authentication Type:	  **OAuth2**
  - OAuth2 Grant Type:	    **client_credentials**
  - OAuth2 Token Endpoint:	https://<the ***url*** from the service key>***/oauth/token?grant_type=client_credentials***
  - OAuth2 Client ID:	      <***clientid*** from the service keys>
  - OAuth2 Client Secret:	  <***clientsecret*** from the service keys>

## System Management: 
https://help.sap.com/viewer/5ac15e8fccb447199fda4509e813bf9f/Cloud/en-US/eb8249323b7041d0ae7b670d064833ce.html

- Download and import the custom operator solution first: SAP RE C4RE custom python operator-1.0.0.tgz
- Download and import the pipelines solution: SAP RE C4RE pipelines-1.0.0.tgz

After importing, you will find the custom operator in the repository: 
- **/subengines/com/sap/python36/operators/sap/c4r/masterdata**.

The two meta pipelines you also will find them in the repository: 
- **/graphs/SAP/RE/C4RE/*CreateTables*/Meta** and **/graphs/SAP/RE/C4RE/*Masterdata*/Meta**.

- The first one is for creating the tables in the entry layer of SAP Data Warehouse Cloud.
- The second one is for loading data from the SAP Cloud for Real Estate API to of SAP Data Warehouse Cloud.

## Maintain connection seetings in sub-piplines:
Next step before running those pipelines is to maintain the connection settings in each sub-pipeline:
- Each of the two meta pipelines contains of a few sub-pipelines. Before you can run one of the meta pipeline you have to manage the inbound and outbound connection of the custom operator in each sub-pipeline:
- Just double click on each sub-pipeline, right click on the last operator and maintain the connections by selecting those from the drop down menu.

## Creating tables in entry layer:
If you have everything prepared, simply run the **/graphs/SAP/RE/C4RE/*CreateTables*/Meta** first and once. If you already done it, this pipeline will fail because the tables are already in place in the data warehouse. So all good from this site.

## Loading data into entry layer:
Afterwards, you can run the **/graphs/SAP/RE/C4RE/*Masterdata*/Meta** pipeline. You can run this pipeline as often you need.

**Hint 1**:
If the structrue of the SAP Cloud for Real Estate API changed, e.g. new fields are added then you can skip these attributes by simply adding them in the specififc sub-pipeline for this entity in the configuration part ***'Skip these attributes'*** but in whole word in ***lower-case***! The adavatge of this approach is, you don't have to manipulate/change the coding itself.

**Hint 2**: 
the **/graphs/SAP/RE/C4RE/*Masterdata*/Meta** pipeline is created to only load data from the source in full-mode. So, no delta is supported yet and deleted data in source will not be deleted in the target.

And voilà, now you can see the data in your SAP Data Warehouse Cloud instance!

**Enjoy your data!**

