Download Link: https://assignmentchef.com/product/solved-sdev400-homework-2
<br>
AWS DynamoDB is a NO SQL solution to databases. It is a fully managed cloud database supportig both document and key-value store models. DynamoDB tables do not require a schema. You do not need to define any attributes, or data types at table creation time. You just need to define the Primary Key(s).

This document will provide an overview of DynamoDB and how to access and use many of the features using the AWS CLI and the AWS SDK.

<strong>Introduction to DynamoDB: </strong>

Similar to other AWS services, with DynamoDB you don’t have to get involved with hardware provisioning, setup and configuration, replication, software patching, or other database maintenance or set-up steps.

There are many useful features in DynamoDB. One worth noting is the ability to automatically delete expired or older, no longer needed items from tables to reduce storage usage and cost. DynamoDB also scales as needed and doesn’t suffer some of the issues associated with scaling large relational databases.

Core components of DynamoDB include tables, items, and attributes. Similar to relational databases, a table is a collection of data. Each table can contain multiple items which are related to the theme or topic of the table. Items can be thought of as rows or records in a relational database. Items are comprised of multiple attributes. For example, a Student table will have multiple items and each item might consist of attributes such as first name, last name, and email address.

Each item in the table will have a unique identifier or primary key. For example, in the Student table the primary key might be the email address or some other unique student identifier. One main difference between SQL and NoSQL databases is that with NoSQL databases, neither the attributes nor their data types need to be defined at creation. Each item can have its own distinct attributes. This makes for some very interesting and ad-hoc data structures. For example, one item in the Student table may include attributes for first name, last name and email address whereas another item may contain first name, last name, email address as well as the student’s middle name and country of birth.

<strong>Primary Keys: </strong>

DynamoDB supports simple and composite primary keys. A simple primary key is one attribute that clearly and uniquely identifies the item in the table. A composite primary key consists of up to two attributes that uniquely identifies an item in the table. An example of a good simple primary key would be a Driver’s License number or Email Address. Each of these keys would uniquely identify an individual. However; sometimes more than one item is required to unique identify an item. For example, consider a Semesters table. Attributes such as Semester (Fall, Spring, Summer) would be a component of the primary key but would not be unique. If we added the Year as another attribute, the composite key should uniquely identify the semester. For example, Fall 2019, or Spring 2020.

Note that indexes are available in NoSQL databases as well. They are not required but they can be used to further enhance queries and searches for items within a table.

DynamoDB uses primary keys to uniquely identify each item in a table and secondary indexes to provide more query flexibility

<strong>Core Elements in DynamoDB: </strong>

DynamoDB has functions, callable through AWS CLI and other Programming API’s, supporting most table and database requirements. As described in the AWS DynamoDB documentation, the following functions are available:

<ul>

 <li>CreateTable – Creates a new table. Optionally, you can create one or more secondary indexes, and enable DynamoDB Streams for the table.</li>

 <li>DescribeTable– Returns information about a table, such as its primary key schema, throughput settings, index information, and so on.</li>

 <li>ListTables – Returns the names of all of your tables in a list.</li>

 <li>UpdateTable – Modifies the settings of a table or its indexes, creates or remove new indexes on a table, or modifies DynamoDB Streams settings for a table.</li>

 <li>DeleteTable – Removes a table and all of its dependent objects from DynamoDB.</li>

 <li>PutItem – Writes a single item to a table. You must specify the primary key attributes, but you don’t have to specify other attributes.</li>

 <li>BatchWriteItem – Writes up to 25 items to a table. This is more efficient than calling PutItem multiple times because your application only needs a single network round trip to write the items. You can also use BatchWriteItem for deleting multiple items from one or more tables.</li>

 <li>GetItem – Retrieves a single item from a table. You must specify the primary key for the item that you want. You can retrieve the entire item, or just a subset of its attributes.</li>

 <li>BatchGetItem – Retrieves up to 100 items from one or more tables. This is more efficient than calling GetItem multiple times because your application only needs a single network round trip to read the items.</li>

 <li>Query – Retrieves all items that have a specific partition key. You must specify the partition key value. You can retrieve entire items, or just a subset of their attributes. Optionally, you can apply a condition to the sort key values, so that you only retrieve a subset of the data that has the same partition key. You can use this operation on a table, provided that the table has both a partition key and a sort key. You can also use this operation on an index, provided that the index has both a partition key and a sort key.</li>

 <li>Scan – Retrieves all items in the specified table or index. You can retrieve entire items, or just a subset of their attributes. Optionally, you can apply a filtering condition to return only the values that you are interested in and discard the rest.</li>

 <li>UpdateItem – Modifies one or more attributes in an item. You must specify the primary key for the item that you want to modify. You can add new attributes and modify or remove existing attributes. You can also perform conditional updates, so that the update is only successful when a user-defined condition is met. Optionally, you can implement an atomic counter, which increments or decrements a numeric attribute without interfering with other write requests.</li>

 <li>DeleteItem – Deletes a single item from a table. You must specify the primary key for the item that you want to delete.</li>

 <li>BatchWriteItem – Deletes up to 25 items from one or more tables. This is more efficient than calling DeleteItem multiple times because your application only needs a single network round trip to delete the items. You can also use BatchWriteItem for adding multiple items to one or more tables.</li>

</ul>

<strong>AWS-CLI Commands </strong>

The section will provide examples of calling a few of the more popular functions for creating a table, putting an item in the table,  getting an items from the table, updating an item in the table and deleting an item in the table. The examples will use the AWS CLI to perform the functions and provide verification through screen captures and views in the AWS management console.

Recall, we use the Cloud9 environment to run the AWS CLI commands for this class.

Documentation for the AWS-CLI for specific commands, sub-commands and options for DynamoDB is found here:

<a href="https://docs.aws.amazon.com/cli/latest/reference/dynamodb/index.html#cli-aws-dynamodb">https://docs.aws.amazon.com/cli/latest/reference/dynamodb/index.html#cli</a><a href="https://docs.aws.amazon.com/cli/latest/reference/dynamodb/index.html#cli-aws-dynamodb">–</a><a href="https://docs.aws.amazon.com/cli/latest/reference/dynamodb/index.html#cli-aws-dynamodb">aws</a><a href="https://docs.aws.amazon.com/cli/latest/reference/dynamodb/index.html#cli-aws-dynamodb">–</a><a href="https://docs.aws.amazon.com/cli/latest/reference/dynamodb/index.html#cli-aws-dynamodb">dynamodb</a> <strong>Create a table: </strong>

To create a table named Students with a primary key of email, the following command should be executed at the command prompt:

aws dynamodb create-table –table-name Students –attribute-definitions

AttributeName=email,AttributeType=S –key-schema

AttributeName=email,KeyType=HASH –provisioned-throughput

ReadCapacityUnits=5,WriteCapacityUnits=5

Note a couple of important requirements for this call:

<ol>

 <li>The Attribute definitions should only list the required Attributes that help uniquely identify the item in the table. In this example, only email is listed. The email will be the HASH.</li>

 <li>When you add items to the table, additional attributes (e.g. Lastname, Firstname, StateofResidence) can be added on the fly</li>

 <li>Attributes are case sensitive. Email is not the same as email.</li>

 <li>Valid AttributeTypes for primary keys include S (for string), N(for number) and B (for binary). Non-primary key attributes may also be Lists, Sets and Maps.</li>

</ol>




A successful response will return in output in JSON format as shown in Figure 1.




<em>Figure 1 AWS CLI Create Table </em>

Navigating to the DynamoDB service in the AWS Management Console, will show the addition of the new Students table with the primary key of “email” as shown in figure 2. Note, previously created tables will also be displayed.




<em>Figure 2 AWS Management Console Confirmation of Table Creation </em>

If you wanted to create a table named Studentsv2 with a composite primary key consisting of email and Lastname, the following command would be entered.

aws dynamodb create-table –table-name Studentsv2 –attribute-definitions AttributeName=Lastname,AttributeType=S AttributeName=email,AttributeType=S -key-schema AttributeName=email,KeyType=HASH

AttributeName=Lastname,KeyType=RANGE –provisioned-throughput

ReadCapacityUnits=5,WriteCapacityUnits=5







As shown in figure 3, the resulting script run from the Cloud9 environment shell will result in a similar output as before but will include the additional composite key attribute.

<em>Figure 3 JSON output snippet for Studentsv2 </em>

After logging into the AWS management console and selecting the DynamoDB service, the console will provide the expected tables along with their primary keys as shown in Figure 4.




<em>Figure 4 AWS Console Showing Created Tables </em><strong>Put an Item in the Students Table: </strong>

The AWS CLI syntax to put an item in a DynamoDB table uses the put-item sub-command and a reference to a specific JSON file that includes the values for each attribute to be input into the table.  The following command will insert one item into the Students DynamoDB table.

aws dynamodb put-item –table-name Students –item file://onestudent.json -return-consumed-capacity TOTAL

The data is listed in the file named onestudent.json and includes the following information. Also, when invoking the command be sure the file exists in the same directory you currently reside. Notice in the screen capture below, the onestudent.json file is in the directory from where the AWS CLI command was called.

{

“email”: {“S”: “<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0f7c6e6363762162667b6c676a63634f7c7b7a6b6a617b217a627a6c216a6b7a">[email protected]</a>”},

“firstname”: {“S”: “Sally”} ,

“lastname”: {“S”: “Mitchell”}  }










<em>Figure 5 Putting an Item in the Students Table </em>

Logging into the AWS Console and selecting the Students table and then the items tab, will reveal the newly entered item as shown in Figure 6.