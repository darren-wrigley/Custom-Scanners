# JDBC based custom scanner

this scanner can be used where the standard EDC JDBC Scanner does not work either at all, or as expected.

known examples where the generic JDBC scanner does not work.

* Denodo - denodo does not have the concept of schemas - all tables in all databases were always extracted, causing duplicate table name errors

### How it works

* reuses the `com.infa.ldm.relational` model package - so the results look like out-of-box dbms scans
* profiling will not be possble (yet, until it is supported with custom scans)
* includes a completely generic scanner for JDBC - with parameters to control many options (more than MITI offers for JDBC) - this is the default (superclass)
* database specific sub-classes can over-ride any functions that operate differently - e.g. denodo for catalog/schema extracts + querying denodo for the internal lineage relationships + custom lineage to the original source objects
* view sql parsing is not currently included, but is something that will be possible


### Configuring the Custom JDBC Scanner

1 - (one time) create a resource-type for <your dbms> - e.g. Snowflake
  * from ldmadmin ui - select Manage | Custom Resource Types
  * click + to create a new Custom Resource type (if not already created)
  * Name=SnowflakeScanner  (changing the name is ok)
  * Model=com.infa.ldm.relational  (browse to select only this model)
  * Connection Types=Database (for linking, no need to select schema)
  
  Note:  you could create resource types for each databsae type (e.g. SnowFlakeScanner, AthenaScanner etc...)

2 - create a resource based on the custom resource type

3 - execute the scanner

  * edit <scannertype>.properties - set customMetadata.folder=<subfolder under current folder> to write scanner output files (folder will be created if not already existing)
      * a .zip file will be created that can be imported into your catalog resource

  * for linux
  `java -cp ".:customJdbcScanner.jar:lib/*" com.infa.edc.scanner.jdbc.GenericScanner snowflake.properties`
  
  