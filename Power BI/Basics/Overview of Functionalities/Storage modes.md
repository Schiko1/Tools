- import mode
	-  import small data sizes from various sources into powerbi
	- stores in memory
	- quick access
	- +:
		- small to medium
		- easly to work with
	- -:
		- Manual refresh
- DirectQuery mode
	- connect directly to data sources
	- data remains in source system
	- for large datasets with complex queries
	- +:
		- Data is always up to date
		- good for large datasets
	- -:
		- can impact performance if query is complex or data source is slow
- Cannot switch from one mode to the other
	- can only switch from directquery to import by importing all necessary data

- Dual Mode
	- Combines both methods
	- power BI services determine which mode is better for the query
	- goof for dimension table
	- small dataset can be set to import, big dataset to direct query

- Composite model
	- combine multiple import models into one
		- Enhance functionality and performance
	- specify storage mode for each table in your data model

## Configuring storage modes
- choose usually
- for each model
	- go to model view
	- if model has color -> direct query; else import
	- right click properties, change storage mode
- Changing from direct query/dual to import is irreversible operation

## Triggers and actions
- used by connecters
- automate workflow
	- refresh data
	- email the latest reports 
- Triggers
	- iniate a workflow and prompt it to run
- actions
	- enable interaction with data source through various functions 
	- automating tasks and processes with actions
	- time based intervals for these action

## Structured data
- quantitative, searchable, sortable, analyzed
- correct storage solution
	- SQL databasse can be cloudbased

## Unstructured data
- stored as blobs()


Data serialization
- turn semistructured into structured