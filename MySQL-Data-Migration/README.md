# MySQL-Data-Migration

This pipeline template allows you to connect to MySQL, select data from a source table, and insert the records into a destination table.

![mysql-data-migration](https://i.imgur.com/VeyXkle.png)

* **MySQL Connectivity**: Performs a test connectivity with MySQL. You need to provide the connection string to be used when connecting to the configured MYSQL database.
* **MySQL Source Table**: This Action uses the **Execute Query** method and selects data from the source table. The data is selected with a limit of 10 records. In this example, the query string used to retrieve the data is ```SELECT * FROM kaholo.netflix limit 10;``` and includes the name of the source table and the limit of records - which can be further customized.
* **MySQL Destination Table**: This Action uses the **Insert Data to Table** method and loads data from the source table to the new table. In this example, the data to insert in the new table is provided by accessing the results of the previous Action ```actions.MySQL2.result```.

## Plugins used in this template

* [MySQL](https://github.com/Kaholo/kaholo-plugin-MySQL) Plugin for Kaholo
