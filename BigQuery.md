# Introduction to Google Cloud BigQuery

## Introduction

BigQuery is Google Cloud's enterprise data warehouse, built upon a serverless architecture. BigQuery data is stored in table/column format and is accessed and managed through standard SQL, which can be executed through BigQuery's API client libraries, built for several common languages.

From an observability perspective, BigQuery is particularly suited for storing and analyzing observability data, such as event data produced by performance monitoring applications. BigQuery is highly scalable and it includes powerful features such as a high bandwidth data analysis engine, supported by machine learning. BigQuery also supports the monitoring of its own performance, by computing and storing metrics around the operations executed upon the data it houses. 

In this course, you'll become familiar with BigQuery through hands-on practice, by executing SQL queries in both the BigQuery SQL Workspace and through the BigQuery command-line tool.
 
By the the end of this course, you'll have a working knowledge of BigQuery, your BigQuery SQL Workspace and Linux command line tool will be up and running, and you'll be equipped to continue experimenting with SQL on your own, using the many BigQuery resources that Google and other practitioners provide.

## Connecting to the BigQuery Sandbox

The BigQuery Workspace is available to anyone with a Google Account at no cost. However Google requires
you to have set up a payment method, for at minimum, verification purposes.

If you already have a Google account with a payment method defined, the BigQuery setup will consist
of just a few steps. Otherwise, they'll be just a few more steps for entering and verifying your information.

To get started, go to: https://cloud.google.com/bigquery . If you're already signed up to use BigQuery, then you'll
be in the right place and you can skip to the lab, just after this section. 

Otherwise, click either of the **getting started** buttons you'll find on the welcome/initialization screen.

![](/assets/images/sign_up_1b.png)

After you've completed the signup steps, you'll be taken to the Google Cloud Console. Scroll to find a link to BigQuery and select it.

![](/assets/images/sign_up_2.png)

When you've arrived at the BigQuery Workspace shown below, you're ready to get going!

![](/assets/images/BigQuery_Console_bd.png)

### Lab: Execute SQL Commands in the BigQuery Workspace

Step 1. You should be logged into BigQuery Workspace, otherwise open it in a new browser tab with https://console.cloud.google.com/bigquery .

Step 2. BigQuery provides sample 'Public Trends' data in a special dataset. Within the initial Workspace banner, Click VIEW DATASET. 

![](/assets/images/Lab1_image_1.png)

If prompted again in another pop-up window, click View Dataset again. 

The **bigquery-public-data** dataset appears in the Explorer pane of the console, on the left, just below your personal dataset, which will be issued a randomly generated name such as 'beaming-storm-379618'.

![](/assets/images/BQ_Explorer_bd.png)

Step 3. Expand the bigquery-public-data dataset in the Explorer pane to reveal the sample table groups that you can practice upon.
Each dataset contains one or more tables. Scroll down to and expand the **census_bureau_usa** table group. 
 
Step 4. Click the **population_by_zip_2010** table to open it in the workspace to the right.  Here you'll see the list of fields that comprise the table.  
Take a moment to review the field names and definitions. Let's now run some queries on it.  

Step 5. Just above the fields list, open the ![](/assets/images/Query_dropdown.png) menu and select **In a new tab**.  The workspace opens with a simple, incomplete SQL SELECT statement. The SELECT clause is shown in error, because you need to supply to the clause, the fields to retrieve. 
 
Select all of the table fields by entering an asterisk * between the SELECT and FROM clauses, making no other changes to the statement. It should look as shown.

~~~
SELECT * FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` LIMIT 1000 
~~~

Step 6. Click the RUN command. The Query results should appear in the a panel below the statement. The first 1000 records in the **population_by_zip_2010** table.
 
Step 7. Modify the statement as shown below and re-run it to retrieve the population for a specific zip code. You can copy/paste
and overwrite statements into the query workspace.

SELECT sum(population) FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` WHERE zipcode = '12054'

Then try this query again with your hometown zipcode.

Step 8. Just for fun, let's query the **samples.shakespeare** table, which contains the text of the entire works of Shakespeare.
The following query counts the number of distinct phrases in Shakespeare's work where the supplied string is found.

SELECT word, SUM(word_count) AS count FROM `bigquery-public-data`.samples.shakespeare WHERE word LIKE '%love%' GROUP BY word 

## Accessing BigQuery Data Outside the Sandbox

BigQuery is more than just an interactive workspace where you can query, import, analyze, and shape data with SQL. BigQuery serves as a data repository used by enterprise cloud services and applications, regardless of how they are deployed.  
  
BigQuery provides APIs for use in Go, Java, Node.js, PHP, Python, C#, and Ruby code. Accessing and managing BigQuery data through code boosts the flexibility and power of SQL queries by introducing parameters, variables, logic and functions, or any other programming constructs (e.g. loops) you require.

Thankfully, Google Cloud makes it easy to connect to BigQuery externally and experiment with coded SQL, through **Google Cloud Shell**, the **bq command-line tool**, and the **CloudShell Editor**. Let's practice using these tools.

### Lab: Access BigQuery Data Through Google Cloud Shell

Step1. Access Google Cloud Shell in a separate browser tab, using the following URL https://console.cloud.google.com/bigquery?cloudshell=true .

Wait a few moments as Cloud Shell provisions a Compute Engine virtual machine running a Debian-based Linux operating system for your temporary use. The Cloud Shell terminal should open in a panel at the bottom of the screen:

![](/assets/images/Cloudshell_Terminal.png)
 
Step2. But instead of creating, editing and running scripts solely on this bash shell command line, let's make this exercise even easier by using the Google Cloudshell Editor. 
 
Just above the terminal, to the right,  Click ![](/assets/images/open_editor.png) and give it a moment to appear. If prompted, open the Home Workspace, and again, if prompted Activate the shell.

Step3. Now add the terminal back into the workspace by clicking **Open Terminal** above the editor.

![](/assets/images/open_terminal.png)

You should now see the Explorer panel, with a folder/file directory under your Google account name. If you mouse-over the top of this directory, you should see icons for creating new files and folders. 

![](/assets/images/new_file.png)

Create a new script file with the name **myfirstbqscript.sh**

Copy the following into the script (in two lines) 

~~~~
bq query --use_legacy_sql=false \ 
'SELECT sum(population) FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` WHERE zipcode = "12054"' 
~~~~

! Careful! 
Make sure the backslash at the end of the first line is recognized. If it is red, 

bq query --use_legacy_sql=false \ 

delete it and retype it starting from preceding characters, until it is recognized. Otherwise your script will not run properly.

On the command line in the bottom panel, run your script with ***$ ./myfirstbqscript.sh***  If prompted, click to authorize use of the bq command. 
 
Was there a permissions error when you ran it? 

With no other permissions settings entered, each newly defined script file you attempt run will require you to open up the permissions. 
On the command line, execute ***$ chmod +x myfirstbqscript.sh*** 

Try running the script again. You should get a result similar to the one below. 

![](/assets/images/bq_sql_output_bd.png)

 
## Converting SQL Queries to Parameterized Queries 

When executing SQL from code, it's impractical to have to edit an SQL statement each time you want to produce slightly different results.  

Recall the zipcode example from the previous lab:

~~~~
'SELECT sum(population) FROM `bigquery-public-data.census_bureau_usa.population_by_zip_2010` WHERE zipcode = "12054"' 
~~~~

If you could substitute the literal "12054" in the command with a parameter, you could supply the script the value on the command line, each time you ran the query. Parameterizing SQL statements is central to the technique of writing dynamic SQL, which builds SQL statements dynamically at runtime. Dynamic SQL is commonly used in SQL stored procedures.
 
### Lab: Convert your SQL query into a parameterized query.

#### Part I ####

Modify **myfirstbqscript.sh** to receive the value for the zip code from command line. 

Review: Running your script on the command line with a parameter would look like this:

***$ ./myfirstbqscript.sh 12054***

Within the script, the parameter value is placed in the variable ***$1*** . If you placed multiple parameters on
the command line, they would be received in the variables $1, $2, $3 etc.
 
For robustness and readability, you could add this line of code as the first line in your script.

***zipcode=$1***

And instead of ***WHERE zipcode = "12054"'*** you would need to modify that clause as follows:

***WHERE zipcode = "'$zipcode'"'*** 

Step x. Modify ***./myfirstbqscript.sh*** accordingly and run it with a single zipcode parameter.

#### Part II ####
 
Just for fun, create another script named ***shakespeare.sh***, to run the same SQL statement we ran in the BigQuery Workspace: 

~~~
SELECT word, SUM(word_count) AS count FROM `bigquery-public-data`.samples.shakespeare WHERE word LIKE '%love%' GROUP BY word 
~~~

Converting the ***LIKE '%love%'*** clause to use a parameter is a little trickier and not well documented. 

Here you need to use ***LIKE "%'$search_string'%"***

Give it a try, and enjoy your Shakespeare data mining!


 

## Other Things to Try

Write script-driven BigQuery SQL that accepts multiple parameters, but also elegantly  

operates when none or some of the parameters are supplied on the command line (or through a call). 

Formulate the SQL statement so that when parameters are missing, it finds a workable default behavior. 


BigQuery is particularly suited for storing and analyzing observability data

(Optional Lab) Upload and query sample event data to BigQuery and execute SQL commands  
to simulate how BigQuery might be used to manage and analyze observability event data stored in the cloud.  

 Now lets apply our script to some event data 

 upload the table 

 Type the SQL into the sandbox 

 Create a new script with the same SQL 

 Parameterize it 

 
## Summary 

## Resources


