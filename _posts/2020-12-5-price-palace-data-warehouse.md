---
layout: post
title: Software Engineering Internship at IndusIntel
tags: 
excerpt_separator: <!--more-->
---

For my class project for CS6400 at GeorgiaTech (Database Systems and Design), my team and I designed a PostgreSQL Database to serve queries on an example database of sales data and store information for a company (Price Palace). For this class project, we worked through the entire design process:

<!--more-->

Analyzing the Requirements
    
The first step before we consider the design of the database is to establish what the client’s (Price Palace, in this case) requirements are. We first understand the reports that the client needs us to generate. For this project, we needed to generate reports on sales data on a city-by-city and store-by-store basis, alongside analyses of sales data and the efficacy of price reductions on certain products. We compile these into a set of constraints and outputs.

Defining the Information Flow Diagram & Entity Relationship Model
    
We can model these constraints and outputs in an Information Flow Diagram (IFD), which breaks down the various tasks that the database needs to be able to perform on the dataset to provide the appropriate output for each report. Given the information flow diagram and reports to be generated, we can create an Entity Relationship Model (ER), which compartmentalizes the data in the dataset to a group of entities, which contain a set of values and relationships to other entities. This allows us to understand the structure of how the data should be stored in the database.

Defining the Relational Model + Normalization

After the Entity Relationship Model is defined, we want to then define these as normalized schemas, so that they may be translated to SQL definitions of tables in the implementation. To ensure data integrity and avoid redundancies of the data, we normalize these schemas according to Boyce-Codd Normal Form. 

Implementing The Database and Queries

Finally, we implement the database and design the SQL queries accordingly. Each report can be sectioned into a single or sequence of queries that provide the necessary information for the report. After the queries were defined, each set of queries associated with a report was attached to a web interface to visualize the data necessary for the client.
This process was a great introduction into the fundamentals of database design and I hope to explore more of the complexities of database design in the future.
