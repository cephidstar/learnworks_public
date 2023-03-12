# Introduction to Google Cloud BigQuery

## Introduction

BigQuery is Google Cloud's enterprise data warehouse for managing and analyzing data. Built upon a serverless architecture, BigQuery requires no infrastructure management. BigQuery data is stored in table/column format and is accessed and managed though standard SQL by which there are no restrictions against the full range of language capabilities, including updating data and creating or deleting tables. 

From an observability perspective, BigQuery supports monitoring by providing built-in performance metrics around the management and analysis operations you perform on your data.

BigQuery is particularly suited for storing and processing observability data, such as event data produced by application performance management systems. BigQuery is a highly scalable and features a data analysis engine that can handle terabytes of data in seconds and petabytes of data in minutes. It is flexible in terms of storage and analysis options, allowing users to store and analyze data within BigQuery or assess it where it already exists. It provides provides powerful tools like BigQuery ML and BI Engine for analyzing and understanding data.

In this course, you'll become familiar with BigQuery through hands-on practice, by executing SQL queries in the BigQuery
Sandbox and though the BigQuery command-line tool.
 
At the end of this course, you'll be ready experiment on your own, and with programming, and you'll be prepared to using the many resources available ...


## Connecting to the BigQuery Console 

The BigQuery sandbox is available to anyone with a Google Account at no cost. 
Here's how [enable access so you can finish the course]  to get started. 

### Lab: Connect to the BigQuery Console and execute SQL commands on preloaded sample table data, using the BigQuery sandbox. 


Step 1. In a new browser tab, open BigQuery console with https://console.cloud.google.com/bigquery 

![](/assets/images/BigQuery_Console_bd.png)

Step 2. Click View Dataset. If prompted again, click View Dataset again. 

The 'bigquery-public-data' dataset appears in the Explorer pane of the console. 

![](/assets/images/BigQuery_Public_dataset.png)

Step 3. Expand the bigquery-public-data dataset. Shown in the Explorer pane are sample table groups that you can practice upon.
Each dataset contains one or more tables. 

![](/assets/images/BQ_Explorer_bd.png)

Step 4. Scroll down to and expand the **census_bureau_usa** table group. 
 
Step 5. Click the **population_by_zip_2010** table to open it in the console workspace.  Here you'll see the list of fields that comprise the table.  
Take a moment to read the field names and definitions. Let's now run some queries on it.  

Step 6. Just above the fields list, open the ![](/assets/images/Query_dropdown.png) menu and select **In a new tab**.  The workspace opens with a simple, incomplete SQL SELECT statement. The SELECT clause is shown in error, because you need to supply the fields to retrieve. 
 
Enter an asterisk * between the SELECT and FROM clauses to return all fields, making no other changes to the statement. It should look as shown.

SELECT * FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` LIMIT 1000 

Step 7. Click the RUN command. The Query results should appear in the a panel below the statement. The first 1000 records in the **population_by_zip_2010** table.
 
Step 8. Modify the statement as shown below to retrieve the population for a specific zip code. You can copy/paste
and overwrite statements into the query tab.

SELECT sum(population) FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` WHERE zipcode = '12054'

Try is again with your hometown zipcode.

Step 9. Run this query in BigQuery to retrieve Shakespeare data from the **samples.shakespeare** table, which contains the text
of the entire works of Shakespeare.

SELECT word, SUM(word_count) AS count FROM `bigquery-public-data`.samples.shakespeare WHERE word LIKE '%love%' GROUP BY word 


## Accessing BigQuery Data Outside the Sandbox

BigQuery is more than just sandbox (cloud console), an interactive workspace where you can query data, import, analyze, build data. BigQuery serves a data repository that can be accessed  by other cloud services and by applications regardless of how they are deployed and   
  
You can query the tables and data housed in BigQuery through the sandbox  

Through the BigQuery API, available for use in Go, Java, Node.js, PHP, Python C# Ruby   

Through Python  

or using scripts in the CloudShell using the bq command-line too.
  
Accessing BigQuery in programming affords us more flexibility in constructing our queries, using variables and parameters, or any other programming constructs (e.g. loops) you require.  

Fortunately, Google Cloud makes connecting to BigQuery [and scripting] externally, quite easy by providing [  Cloudshell ] and use bash shell scripts 

to execute SQL queries, to familiarize you with building tools 



### Lab: Access BigQuery Data through the bq command-line tool  

 

 Before you can use the bq command-line tool, you must use the Google Cloud console to create or select a project.
 
In a separate browser tab, use the following URL to open the Cloudshell editor where you'll write your script: https://console.cloud.google.com/bigquery?cloudshell=true 


Give it a moment as Cloud Shell provisions a Compute Engine virtual machine running a Debian-based Linux operating system for your temporary use.  

 

The Cloud Shell terminal should open in a panel below.  
 

[After the command line appears,  
But let's make this exercise even easier by using the friendlier  by opening the Cloudshell editor. 
 
Just about the terminal, to the right,  Click Open Editor and give it a moment to appear. 
To launch the editor, click  Open Editor on the toolbar of the Cloud Shell window. 

 
Open Home Workspace 

Activate the shell 

 
Now it's time to execute our queries using a script 
 
To do this, all we need to do is to get the BigData Query written inside the script and execute our 

script through the Google Cloud Shell Console, which will help us to connect to the BigQuery service as 

![](/assets/images/bq_query_new_file_bd.png)

Create a script: and give it the name myfirstbqscript.sh 

 

Copy the following into the script (in two lines) 

 
~~~~
bq query --use_legacy_sql=false \ 
'SELECT sum(population) FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` WHERE zipcode = "12054"' 
~~~~
 

! Careful! 

 

Run it with ./sample.sh 
 

What happened when you ran it? 
chmod +x 
try again 
 

If prompted to authorize, click authorize. 
 
If you still get an error, make sure the backslash in the first line is recognized. If it is red, 

delete it and start typing from the last token so that the line looks like this. 
 

bq query --use_legacy_sql=false \ 
 
Now run your query again. You should get a result similar to the one below. 

![](/assets/images/bq_sql_output_bd.png)

 
## Converting SQL queries to parameterized queries 

 

(Text) Parameterized SQL 

 

[The power and importance of parameterized queries] 

 

Suppose you wanted to substitute the literal '%love%'  in the command with a parameter that you could supply each time you run the query, say in a pop-up dialogue. In this form, the type of SQL is called dynamic SQL. 
 
In some programming contexts, the ? symbol can be used as a placeholder for an SQL parameter, however in BigQuery UI this does not work. 

 

Although you can create a dynamic SQL statement in BigQuery, it's quite complex.  
 
 

### Lab: Convert your SQL queries to parameterized queries, You will boost the power of your queries to dynamically modify query targets. 

 

Now let's modify our script to allow you to parameterize your SQL statement and 

receive the values for the parameter from the command line, much as you would for 

any value you'd like to pass into a script.   
 
Then as in this case, you could simply execute the script by name on the command line while introducing the searched for string as the 1st parameter, like this. 
 
Now edit your script to look like this: 
echo $1 

zipcode=$1 

bq query --use_legacy_sql=false \ 

 'SELECT sum(population) FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` WHERE zipcode = "'$zipcode'"' 

 

Run it with ./sample.sh 12054 
 
Run it with any zipcode you choose. 
 
Just for fun, create another script named shakespeare.sh with the following:  [what it does] 
echo $1 

search_string=$1 

bq query --use_legacy_sql=false \ 

 'SELECT word, SUM(word_count) AS count FROM `bigquery-public-data`.samples.shakespeare WHERE word LIKE "%'$search_string'%" GROUP BY word' 

 

[What's the point] [what could you do next?] 

[Stuf to try:  Write script-driven BigQuery SQL that accepts multiple parameters, but also elegantly  

operates when none or some of the parameters are supplied on the command line (or through a call). 

Formulate the SQL statement so that when parameters are missing, it finds a workable default behavior. 

 

## (Optional) 4. Upload and query sample event data to BigQuery 

 

(Optional Lab) Upload and query sample event data to BigQuery and execute SQL commands  
to simulate how BigQuery might be used to manage and analyze observability event data stored in the cloud.  

 

Now lets apply our script to some event data 

 

upload the table 

 

Type the SQL into the sandbox 

 

Create a new script with the same SQL 

 

Parameterize it 

 

[Review of what we did and how we met the objectives] 

 

[Things for you to try] 

 

[References]  

 

 

Summary 
