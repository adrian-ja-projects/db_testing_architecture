# Testing strategy 
**Overview**  
Fleshing out thoughts for a CI/CD with automated testing for the data platform

## Overall architecture
Placeholder for picture

**Zoom in**  

As seen in the picture above, each "zone" in the architecture runs a set of data quality checks. The rules and results are stored in the SQL db "Metadata DB". The purpose of this is so that downstream consumers - Data Scientist, Data Modellers, Data Analyst - can also read the data profile results before running their downstream batch processes. 

Also, if defined by the data owner, certain rules can make the entire pipeline fail before moving from zone to zone. 

Assumptions:
* Azure Data Factory Orchestrate the end to end pipelines

# High-level CI/CD Strategy
It is a rough idea that needs details to be perfectioned but could turn into a framework with further work

## Type of testing

* Unit testing
* Integration testing (end-to-end run)
* Quality testing

### Unit testing.

All modules created for any ETL in databricks should be tested with their respective testing tooling that should be triggered automatically as part of the Azure DevOps release pipeline. This can be done using Databricks API as an activity in DevOps. 

For example, pytest can be used as part of the release pipeline

### Integration testing



**Zoom in**  

As seen in the architecture picture, under the raw zone there is a zone called "Sample Data". This section contains only a representative sample of the production data . i.e. last week of data from source. This is with the purpose to be used to carry out integration testing. 
Further explanation on the use of sample data is below.

**Integration testing will consist of two types of testing:** 

* Data Quality Testing

This testing uses the sample data and run data quality checks to ensure all important KPIs are within the expected range and values. Sample data should be small enough to make the test efficient but big enough to provide meaningful quality results. 

As part of the release, a smoke test should be run using the sample data. This can be automated by having a Global Parameter in ADF that all pipelines use and that points at the sample ADLS for read and write. As in production, if any table returns data quality checks outside the expected values an error will be caught. 

* Logical testing

Logical testing is used with testing data. Testing data is a very small dataset which is used in the transformation and at the end, the result is compared with an expected dataframe. Both should be equal. This can be achieved as part of a pytest script. 
