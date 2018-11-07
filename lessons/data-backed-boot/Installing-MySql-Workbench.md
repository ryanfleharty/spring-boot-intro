## Setting up MySql Workbench

[Download the installer](https://dev.mysql.com/downloads/workbench/)

Meanwhile, install mysql by running `brew install mysql`

Start your mysql server by running `brew services start mysql`

Next, run the command ` mysql_secure_installation` to configure your root username and password. There will be a sequence of Yes/No questions, be sure to say YES to resetting privileges. Follow the prompted directions to set your password for the root user.

Now, open MySql Workbench and click the + icon to create a new connection. Give the connection a name and also provide it the password you just set for the root user. Once you've created that connection, it should show up in the dashboard. Click it to open that connection.

Once you've connected to the database, you should be able to create a new Schema. Find the icon towards the top that looks like several cylinders and create a new schema- this will be the name of the database you can connect to in your application!