
I’ve logged into Starburst Galaxy and completed all the necessary setup [infrastructure steps](https://github.com/monimiller/dbt-galaxy-covid-demo/blob/main/INFRA_SETUP.MD) so that I am now ready to connect to dbt Cloud.  First thing’s first, you need to set up a project. 

**Project name:** dbt-galaxy
Choose a connection: Starburst



At step 3, Configure your environment, navigate back to the cluster you created in Starburst Galaxy. Click the Connection info button, and then select dbt as the client of choice. You should see connection information that looks something like this. 



Download the information and bring it back to dbt Cloud.  Put in the appropriate host and port from the connection info. Then, put in your username for Starburst Galaxy, but also attach the role you want to use (shown as User in the connection info). Enter your Galaxy password.

Catalog: dbt_aws_tgt - name of the target AWS catalog in Galaxy
Schema: dbt_mmiller - default schema for your models, add an identifier for yourself
Target Name: default - won’t come up in this scenario 
Threads: 6 - default value



Next, test the connection to make sure everything is configured properly. 


Create a fork of the tutorial repository. Then, select this fork as your repository in dbt Cloud. I’m connected to dbt Cloud through GitHub, so my repositories automatically popped up. Search and select your fork. 

Now that your project is set up, you should see the repository imported into dbt. You should be able to run everything without making any edits, but if you want to edit any of the models, create a new branch. 



Run the following commands on your main branch:

Dbt deps

dbt run
dbt test
dbt build



Now, all your models have completed and the data pipelines have successfully transformed your data. Navigate back to Starburst Galaxy and secure access so that your consumers only are able to view the final aggregate tables, and only have access to select from the tables.

First select from each table to validate your output is indeed what you expect. 


SELECT * FROM agg_nation;




SELECT * FROM agg_region;


 

Navigate to the Roles and privileges tab, then select Add role. Enter the information and create the new role. 


Click within the newly created role, and navigate to the Privileges tab. Select Add privilege.


Add the table privilege information for both tables:

Modify privileges: Table
Catalog contains the tables: dbt_aws_tgt
Which schemas can this role access: dbt_mmiller (same one created in dbt)
Which tables can this role access: agg_nation
What can they do: Allow select from table


Modify privileges: Table
Catalog contains the tables: dbt_aws_tgt
Which schemas can this role access: dbt_mmiller (same one created in dbt)
Which tables can this role access: agg_region
What can they do: Allow select from table


Now, navigate to the consume_layer_access role in the top right corner. 


Try running select statements and deletes to see what permissions are now available to your data consumers. 

SELECT * FROM agg_nation;


SELECT * FROM agg_region;

DELETE FROM agg_region where region = 'ASIA';


DELETE FROM agg_nation where nation = 'JAPAN';

Try running select statements and deletes to see what permissions are now available to your data consumers. 


Congrats! You’ve created an open data lake architecture, created a data pipeline, and secured proper access to the final tables using dbt Cloud and Starburst Galaxy.  If you are interested in completing another tutorial, follow the Starburst Galaxy quickstart on dbt Cloud’s website. 
