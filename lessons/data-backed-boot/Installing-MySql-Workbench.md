## Setting up and using MySql Workbench

[Download the installer](https://dev.mysql.com/downloads/workbench/)

Meanwhile, install mysql by running `brew install mysql`

Start your mysql server by running `brew services start mysql`

Next, run the command ` mysql_secure_installation` to configure your root username and password. There will be a sequence of Yes/No questions, be sure to say YES to resetting privileges. Follow the prompted directions to set your password for the root user.

Now, open MySql Workbench and click the + icon to create a new connection. Give the connection a name and also provide it the password you just set for the root user. Once you've created that connection, it should show up in the dashboard. Click it to open that connection.

Once you've connected to the database, you should be able to create a new Schema. Find the icon towards the top that looks like several cylinders and create a new schema- this will be the name of the database you can connect to in your application!

From there, you can double-click the name of your new Schema in the bottom left menu of Schemas in order to connect to it. You should see the name take on a bold font to distinguish it as the currently connected schema. Open up the schema and right click on the Tables drop-down to open up the option to create tables. Now you can create tables! 

You can add data to the tables by either running SQL commands (File>New Query Tab will open a text field for entering commands, and you can click the lightning bolt to run a query. I know, this program is hard to figure out at first!) or by clicking the table-looking icon when you hover over the name of a table you've created.