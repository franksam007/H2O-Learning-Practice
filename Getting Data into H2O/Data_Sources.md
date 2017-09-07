### Default Data Sources
* Local File System
* Remote File
* S3
* HDFS
* JDBC

## JDBC Databases   
Relational databases that include a JDBC (Java database connectivity) driver can be used as the source of data for machine learning in H2O.
Currently supported SQL databases are _MySQL_, _PostgreSQL_, and _MariaDB_. Data from these SQL databases can be pulled into H2O using 
the <code>import_sql_table</code> and <code>import_sql_select</code> functions.

* import_sql_table   
  This function imports a SQL table to H2OFrame in memory. This function assumes that the SQL table is not being updated and is stable. 
Users can run multiple SELECT SQL queries concurrently for parallel ingestion.

  _Note_: Be sure to start the h2o.jar in the terminal with your downloaded JDBC driver in the classpath:

  <code>java -cp <path_to_h2o_jar>:<path_to_jdbc_driver_jar> water.H2OApp</code>
  
  The import_sql_table function accepts the following parameters:

  connection_url: The URL of the SQL database connection as specified by the Java Database Connectivity (JDBC) Driver. For example, “jdbc:mysql://localhost:3306/menagerie?&useSSL=false“
    * table: The name of the SQL table
    * columns: A list of column names to import from SQL table. Default is to import all columns.
    * username: The username for SQL server
    * password: The password for SQL server
    * optimize: Specifies to optimize the import of SQL table for faster imports. Note that this option is experimental.

  python   
  <pre><code>connection_url = "jdbc:mysql://172.16.2.178:3306/ingestSQL?&useSSL=false"
  table = "citibike20k"
  username = "root"
  password = "abc123"
  my_citibike_data = h2o.import_sql_table(connection_url, table, username, password)</code></pre>

  * import_sql_select
  
    This function imports the SQL table that is the result of the specified SQL query to H2OFrame in memory. It creates a temporary SQL 
  table from the specified sql_query. Users can run multiple SELECT SQL queries on the temporary table concurrently for parallel 
  ingestion, and then drop the table.

    _Note_: Be sure to start the h2o.jar in the terminal with your downloaded JDBC driver in the classpath:

  <code>java -cp <path_to_h2o_jar>:<path_to_jdbc_driver_jar> water.H2OApp</code>
  
  The import_sql_select function accepts the following parameters:

* connection_url: URL of the SQL database connection as specified by the Java Database Connectivity (JDBC) Driver. For example, 
“jdbc:mysql://localhost:3306/menagerie?&useSSL=false“
* select_query: SQL query starting with SELECT that returns rows from one or more database tables.
* username: The username for the SQL server
* password: The password for the SQL server
* optimize: Specifies to optimize import of SQL table for faster imports. Note that this option is experimental.

  python   
  <pre><code>connection_url = "jdbc:mysql://172.16.2.178:3306/ingestSQL?&useSSL=false" 
  select_query = "SELECT bikeid from citibike20k"  
  username = "root"  
  password = "abc123"  
  my_citibike_data = h2o.import_sql_select(connection_url, select_query, username, password)</code></pre>
