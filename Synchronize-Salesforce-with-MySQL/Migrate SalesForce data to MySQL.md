# Migrate SalesForce data to MySQL

You can use this template to syncronize data from Salesforce to MySQL based on the following logic:

* Retrieve the total number of records for a given Salesforce object
* Based on the total number of records, you can iteratively query the Salesforce object and construct an array of records
* Drop a table in MySQL in case it already exists
* Create an empty table in MySQL based on `column_mapping` array from the configuration object
* Insert Salesforce object records into the given table
* Send a Slack notification

![migrate-salesforce-data-to-mysql](D:\Work\Kaholo\plugins templates\salesforce_mysql.png)

## Plugins used in the template

* [SalesForce Plugin[(https://github.com/Kaholo/kaholo-plugin-salesforce)] for Kaholo
* [MySQL Plugin](https://github.com/Kaholo/kaholo-plugin-MySQL) for Kaholo
* [Slack Plugin](https://github.com/Kaholo/kaholo-plugin-slack) for Kaholo

## Configuration variables

* sf_object - The Saleforce object from which you want to retrieve records
* sf_limit - The limit of records to retrieve from a given Salesforce object
* database - The name of the MySQL database to use
* table_name - The name of the empty table to create in MySQL
* column_mapping - The mapping of the table columns including column names and data types.

Example:

{
    "sf_object": "Account",
    "sf_limit": 2000,
    "database": "kaholo",
    "table_name": "account",
    "column_mapping": [
        {"sf_object_field": null, "column_name": "id", "data_type": "INT UNSIGNED AUTO_INCREMENT PRIMARY KEY"},
        {"sf_object_field": "Id", "column_name": "sf_id", "data_type": "VARCHAR(18) NOT NULL"},
        {"sf_object_field": "Name", "column_name": "name", "data_type": "VARCHAR(255) NOT NULL"},
        {"sf_object_field": "Phone", "column_name": "phone", "data_type": "VARCHAR(255)"}
    ]
}