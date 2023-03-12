# Introduction to Google Cloud BigQuery

## Introduction

BigQuery is Google Cloud's enterprise data warehouse, built upon a serverless architecture. BigQuery data is stored in table/column format and is accessed and managed through standard SQL. {How is the SQL executed}.

From an observability perspective, BigQuery is particularly suited for storing and analyzing observability data, such as event data produced by performance monitoring applications. BigQuery ishighly scalable and it includes powerful features such as a high bandwidth data analysis engine and machine learning. BigQuery also supports monitoring its own performance by generating and storing metrics around the operation executed upon the data it houses. These metrics are available for creating charts and alerts.

In this course, you'll become familiar with BigQuery through hands-on practice, by executing SQL queries in both the BigQuery
Sandbox and through the BigQuery command-line tool.
 
At the end of this course, you'll be ready experiment on your own, and with programming, and you'll be prepared to using the many resources available ...

## Connecting to the BigQuery Console 

The BigQuery sandbox is available to anyone with a Google Account at no cost. 
Here's how [enable access so you can finish the course]  to get started. 

If you have a google account with a payment method
If you don't

### Lab: Connect to the BigQuery Console and execute SQL commands on preloaded sample table data, using the BigQuery sandbox. 


Step 1. In a new browser tab, open BigQuery console with https://console.cloud.google.com/bigquery 

![](/assets/images/BigQuery_Console_bd.png)

Step 2. Click View Dataset. If prompted again, click View Dataset again. 

The 'bigquery-public-data' dataset appears in the Explorer pane of the console, on the left. 

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

Try this query again with your hometown zipcode.

Step 9. Run this query in BigQuery to retrieve Shakespeare data from the **samples.shakespeare** table, which contains the text
of the entire works of Shakespeare.

The following query counts the number of distinct phraases in Shakespeare's work where the supplied string is found.
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



### Lab: Access BigQuery Data Through The bq command-line Tool  

Before you can use the bq command-line tool, you must use the Google Cloud console to create or select a project.
 
In a separate browser tab, use the following URL to open the Cloudshell editor where you'll write your script: https://console.cloud.google.com/bigquery?cloudshell=true 

Wait a few moments as Cloud Shell provisions a Compute Engine virtual machine running a Debian-based Linux operating system for your temporary use.  
The Cloud Shell terminal should open in a separate panel at the bottom of the screen:

![](/assets/images/Cloudshell_Terminal.png)
 
But instead of creating, editing and running scripts solely on this bash shell command line, let's make this exercise even 
easier by using the Google Cloudshell fulscreen editor. 
 
Just above the terminal, to the right,  Click **Open Editor** and give it a moment to appear. 
If prompted, Open the Home Workspace, and again, if prompted Activate the shell.

Then add the terminal back in by clicking terminal.

Now it's time to execute our queries using a script. To do this, all we need to do is to get the BigData Query written inside the script and execute our 
script through the Google Cloud Shell Console, which will help us to connect to the BigQuery service as 

![](/assets/images/bq_query_new_file_bd.png)

Create a script: and give it the name myfirstbqscript.sh 

Copy the following into the script (in two lines) 

~~~~
bq query --use_legacy_sql=false \ 
'SELECT sum(population) FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` WHERE zipcode = "12054"' 
~~~~
 

! Careful! 
If you still get an error, make sure the backslash in the first line is recognized. If it is red, 
delete it and start typing from the last token so that the line looks like this. 
bq query --use_legacy_sql=false \ 
 

On the command line in the bottom panel, run it with ./myfirstbqscript.sh 
 
Was there a permissions error when you ran it? With no other permissions settings entered, each newly defined script file you
attempt run will require you to open up the permissions. On the command line, execute

chmod +x myfirstbqscript.sh 

Try running the script again. If prompted, click to authorize use of the bq command. 
You should get a result similar to the one below. 

![](/assets/images/bq_sql_output_bd.png)

 
## Converting SQL Queries to Parameterized Queries 

When executing SQL from code, it's impractical to have to edit an SQL statement each time you want to produce slightly different results.  

Using the zipcode example from the previous lab:

~~~~
'SELECT sum(population) FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` WHERE zipcode = "12054"' 
~~~~

Suppose you wanted to substitute the literal "12054" in the command with a parameter for which you could supply a value on the command line each time you ran the query script.

Parameterizing SQL statements is central to the technique of writing dynamic SQL, which builds SQL statements dynamically at runtime.
Dynamic SQL is commonly used in SQL stored procedures.
 
### Lab: Convert your SQL query into a parameterized query.

Now let's modify our script to allow you to parameterize your SQL statement and receive the values for the parameter from the command line, much as you would for 
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

 This syntax for the Shakespeare example is a little trickier. Here you need to use **LIKE "%'$search_string'%"**

[What's the point] [what could you do next?] 

Stuff to try:  Write script-driven BigQuery SQL that accepts multiple parameters, but also elegantly  

operates when none or some of the parameters are supplied on the command line (or through a call). 

Formulate the SQL statement so that when parameters are missing, it finds a workable default behavior. 

 

## (Optional) 4. Upload and query sample event data to BigQuery 

BigQuery is particularly suited for storing and analyzing observability data

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
