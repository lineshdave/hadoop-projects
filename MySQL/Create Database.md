# Create Database

Check for all available databases in MySQL and install <b>world</b> database.

<OL>
<LI>Using the steps provided in the <b>Access MySQL</b> section, log into MySQL.</LI>
<LI>List all available database - type the command <b>show databases;</b>.</LI>
<LI>If <b>world</b> database is available, delete it for recreating it later - type the command <b>delete database world;</b>.</LI>
<LI>Download the <b>world.sql</b> file from the URL - (https://dev.mysql.com/docs/index-other.html). This file is available in zip format</LI>
<LI>Note that a browser within the Cloudera VM should be used to download the above zip file.</LI>
<LI>Extract contents of the downloaded zip file into any folder that can be referenced later. It has only one file <i>world.sql</i>.</LI>
<LI>From the MySQL prompt, run the downloaded <b>world.sql</b> file using the command - <b>source world.sql</b>. Ensure that the file is available in the same directory where the mysql command was run to login.</LI>
<LI>The above step will result in <b>world</b> database created along with its tables and data.</LI>
<LI>Use the new <b>world</b> datase and display the tables within the database using the following commands - (a) <b>use world;</b>; and, (b) <b>show tables;</b>
<LI>The activity of creating the <b>world</b> database is complete if the above steps results in few tables within this database being displayed.</LI>
</OL>
