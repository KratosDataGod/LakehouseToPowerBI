# Lakehouse To PowerBI
Architectural design for incorporating a Data Lakehouse architecture with an Enterprise Power BI Deployment

Enterprise Data & Analytics Engineering
This article is part of the Power BI implementation planning series of articles. It describes using the medallion lakehouse architecture and the star schema design to create and manage data models optimized for performance and usability in Power BI. It also explains how to integrate these data models with an enterprise power bi data model that can serve as a single source of truth for your organization.



# What is the medallion Lakehouse architecture?


﻿The medallion lakehouse architecture is a data design pattern used to logically organize data in a lakehouse, aiming to incrementally and progressively improve the structure and quality of data as it flows through each layer of the architecture (from Bronze ⇒ Silver ⇒ Gold layer tables). The medallion architecture guarantees atomicity, consistency, isolation, and durability as data passes through multiple layers of validations and transformations before being stored in a layout optimized for efficient analytics.

A lakehouse is a new data platform architecture paradigm that combines the best features of data lakes and data warehouses. A modern lakehouse is a highly scalable and performant data platform hosting raw and prepared data sets for quick business consumption and to drive advanced business insights and decisions1. It breaks data silos and allows seamless, secure data access to authorized users across the enterprise on one platform.

The medallion architecture consists of three main layers: bronze, silver, and gold.

The bronze layer contains unvalidated data. Data ingested in the bronze layer typically maintains the raw state of the data source, is appended incrementally and grows over time, and can be any combination of streaming and batch transactions.
The silver layer contains validated, enriched data trusted for downstream analytics. Data in the silver layer is matched, merged, conformed, and cleansed ("just enough") so that it provides an "Enterprise view" of all its key business entities, concepts, and transactions.
The gold layer contains highly refined and aggregated data that powers analytics, machine learning, and production applications. Data in the gold layer is transformed into knowledge rather than just information.


# What is the star schema design?


The star schema design is a dimensional data model used to organize data in a database that is easy to understand and analyze. The star schema design can be applied to data warehouses, databases, data marts, and other tools. The star schema design is optimized for querying large data sets.

The star schema design requires modelers to classify their model tables as either dimension or fact. Dimension tables describe business entities—the things you model. Entities can include products, people, places, and concepts including time itself. Fact tables store observations or events, including sales orders, stock balances, exchange rates, temperatures, etc.

The star schema design uses a denormalized approach to data, adding redundant columns to some dimension tables to make querying and working with the data faster and easier. The purpose is to trade some redundancy (data duplication) in the data model for increased query speed by avoiding computationally expensive join operations.



The star schema design has several benefits, such as:

It is simple to understand and implement
It enables incremental ETL
It can recreate your tables from raw data at any time
It supports ACID transactions and time travel
It is excellent for simple queries because of its reduced dependency on joins when accessing the data
It adapts well to fit OLAP models
It improves query performance as compared to normalized data



# How does the star schema fit in the medallion lakehouse architecture?

The star schema design can be applied to any layer of the medallion lakehouse architecture. However, it is most commonly used for the gold layer tables that power analytics, machine learning, and production applications. The gold layer tables are often highly refined and aggregated, containing data that has been transformed into knowledge.



# How to integrate with an enterprise Power BI data model

An enterprise Power BI data model is a data model that can serve as a single source of truth for your organization. It can provide consistent and accurate data for various reports and dashboards across different departments and business units. An enterprise Power BI data model can also enable self-service analytics and user collaboration.



To integrate your medallion lakehouse architecture and star schema design with an enterprise Power BI data model, you can follow these steps:


Identify the key business entities, concepts, and transactions relevant to your organization and align them with your lakehouse's bronze, silver, and gold layers.

Define the granularity, dimensions, and measures of your fact tables and the attributes and hierarchies of your dimension tables.

Use Azure Data Factory to connect to your data sources and ingest raw data into the bronze layer of your lakehouse. You can use incremental refresh to append new data over time.

Use Spark notebooks (in either Synapse or Azure Databricks) to validate, conform, and cleanse your data from the bronze layer and load it into the silver layer of your lakehouse. You can use query folding to optimize the performance of your transformations.

Use Spark notebooks (in either Synapse or Azure Databricks) to transform, enrich, and aggregate your data from the silver layer and load it into the gold layer of your lakehouse. You can use variables to improve your formulas.

Use Spark notebooks (in either Synapse or Azure Databricks) to create a star schema design for your gold layer tables by creating one-to-many relationships between your dimension and fact tables. 

Use Tabular Editor to load tables directly into your analytical model from your gold layer. Enterprise solutions should contain no transformations between the gold and analytical layers. In TE3, you can use the waterfall technique or the star schema approach to organize your tables in the model view.

Use Tabular Editor to create measures using DAX to calculate business metrics based on your gold layer tables. You can use appropriate error functions, avoid converting blanks to values, use the divide function instead of the divide operator, use countrows instead of count, use selectedvalue instead of values, etc.

Use Power BI Desktop to create a separate report from your model using a live connection or direct query mode. You can use report page tooltips, report page drill-through, etc., to enhance your report design.

Use Power BI service to publish your model from Tabular Editor to a workspace dedicated to enterprise data models. While you can use deployment pipelines to manage development, test, and production environments, many enterprises find it worthwhile to build a CICD pipeline for model management, including data validation and regression testing. 

Use the Power BI service to share your model and report with other workspace users. You can also enable row-level security (RLS) to restrict data access based on user roles.

The Power BI service includes increased controls over who is allowed to Read, Build, and Share reports that have access to your analytical models. 

Use Power BI service to monitor and troubleshoot your model's performance and report. You can use query diagnostics, performance analyzer, etc., to identify and resolve issues.
﻿

# Scenario diagram

The following diagram depicts a high-level overview of the most common user actions and relevant Data Engineering and Power BI components that support enterprise Data and Analytics. The focus is on the use of Azure DevOps to manage and publish content programmatically at scale through development, test, and production workspaces in the Azure service.

The scenario diagram depicts the following user actions, processes, and features.



Azure Data Factory is a cloud-based data integration service that allows you to create data-driven workflows (called pipelines) for ingesting data from various sources and orchestrating data movement and transforming data at scale. 

The bronze layer is the first layer of the medallion lakehouse architecture that contains raw and unvalidated data from various sources, both streaming and batch.

The silver layer is the second layer of the medallion lakehouse architecture that contains validated and enriched data that can be trusted for downstream analytics. 

The gold layer is the third and final layer of the medallion lakehouse architecture that contains highly refined and aggregated data that powers analytics, machine learning, and production applications.

Data in the silver layer is matched, merged, conformed and cleansed ("just-enough") so that it provides an "Enterprise view" of all its key business entities, concepts and transactions

The Gold layer also houses departmental and data science sandboxes to enable self-service analytics and data science across the enterprise. Providing these sandboxes and their own separate compute clusters prevents the Business teams from creating their own copies of data outside of the Lakehouse.

No Power Query transformations are allowed in enterprise models ensuring full query folding to the source to support both direct query and import load patterns. The only exception on transformations is for incremental loads and for aggregations on top of a direct query table.

Power BI workspaces dedicated for model deployments are workspaces that contain only data models and no reports or dashboards. These workspaces are used to create a single source of truth for enterprise data products and to enable centralized datasets that can be shared and reused by other workspaces.

Power BI workspaces for thin reports are workspaces that contain only reports and no data models. These workspaces are used to create reports that connect to existing datasets in other workspaces using live connection model.

To connect to data sources that reside within a private organizational network, an on-premises data gateway is required. You can set up Azure Pipelines to perform gateway configuration activities by using the Power BI REST APIs (not shown in this scenario diagram).

Data Engineers, Analytics Engineers, and Content creators regularly commit and push their changes to a remote repository during development by using a Git client such as Visual Studio Code. In this diagram, the remote repository is Azure Repos.

Changes to content in the remote repository trigger Azure Pipelines. A validation pipeline is the first pipeline that is triggered. The validation pipeline performs automated tests to validate content before it's published.

Content that passes the validation pipeline triggers a subsequent build pipeline. The build pipeline prepares content for publishing to the Synapse/ Databricks environments and to the Power BI service. The process up to this point is typically referred to as continuous integration (CI).

Content is published and deployed by using release pipelines. The release pipelines use Synapse/Databricks API's, either the Power BI REST APIs or the workspace XMLA endpoint to programmatically import content to the Power BI service. Deployment by using release pipelines is typically referred to as continuous deployment (CD).

Data Engineers manage the Lakehouse code deployments and promotions through Dev / Test / Production keeping in close contact with the Analytics Engineers always monitoring for breaking changes (schema or cardinality changes). 

Analytics Engineers manage the tabular model deployments and data refreshes of the model through Dev / Test / Production keeping in close contact with the Data Engineers for up stream breaking changes, and with the Content Creators for down stream breaking changes.

Content Creators work with both the Data Engineers and Analytics engineers to monitor changes to the models, data quality, and refresh timing.

Data Teams may be all within IT, or they may be broken out by different domains and domain owners.

Data Engineers build and manage data pipelines, preparing for analytical consumption, and for ensuring data quality.

Analytics Engineers build and manage tabular models for consumption by Content Creators.

Content Creators focus primarily focused on delivering value to the business through the consumption of analytics.


# Next steps

In this article, you learned how to use the medallion lakehouse architecture and the star schema design to create and manage Power BI data models optimized for performance and usability. You also learned how to integrate these data models with an enterprise power bi data model that can serve as a single source of truth for your organization.



To learn more about Power BI guidance documentation, you can refer to this link: https://learn.microsoft.com/en-us/power-bi/guidance/

To learn more about Power BI implementation planning series, you can refer to this link: https://learn.microsoft.com/en-us/power-bi/guidance/implementation-planning

To learn more about Power BI adoption roadmap series, you can refer to this link: https://learn.microsoft.com/en-us/power-bi/guidance/adoption-roadmap

To learn more about Power BI migration series, you can refer to this link: https://learn.microsoft.com/en-us/power-bi/guidance/migration

To learn more about Power BI Center of Excellence (COE) series, you can refer to this link: https://learn.microsoft.com/en-us/power-bi/guidance/coe







# References

https://addendanalytics.com/blog/data-modelling-best-practices/

https://blog.enterprisedna.co/data-modelling-in-power-bi-tips-best-practices/

https://codingsight.com/power-bi-star-schema-the-easy-how-to-guide-for-starters/

https://community.powerbi.com/t5/Community-Blog/Data-Modelling-In-Power-BI-Helpful-Tips-amp-Best-Practices/ba-p/1977956

https://docs.databricks.com/lakehouse/medallion.html

https://learn.microsoft.com/en-us/azure/databricks/lakehouse/medallion 

https://learn.microsoft.com/en-us/azure/data-explorer/power-bi-best-practices

https://learn.microsoft.com/en-us/power-bi/guidance/

https://learn.microsoft.com/en-us/power-bi/guidance/star-schema

https://radacad.com/power-bi-basics-of-modeling-star-schema-and-how-to-build-it

https://www.databricks.com/glossary/medallion-architecture

https://www.mssqltips.com/sqlservertip/7597/star-schema-data-modeling-for-power-bi/

https://www.powercommunity.com/design-your-data-model-efficiently-with-star-schema-for-power-bi-must-know/


