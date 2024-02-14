# Migrate GLPI to another host

---

## General steps

1. Set up the new host: Install the required software (e.g., web server, PHP, database server) on the new host. Make sure it meets the system requirements for GLPI.

2. Backup the GLPI files: On the current host, create a backup of the GLPI files. This typically involves creating a copy of the GLPI directory.

3. Export the GLPI database: Export the GLPI database from the current host. The method for exporting the database depends on the database management system you are using (e.g., MySQL, PostgreSQL). You can use tools like `mysqldump` or `pg_dump` to export the database to a file.

4. Transfer the GLPI files: Transfer the backup of the GLPI files to the new host. You can use tools like `rsync` or `scp` to copy the files to the new host.

5. Import the GLPI database: Import the GLPI database into the new host's database management system. Again, the method for importing the database depends on the database management system you are using. You can use tools like `mysql` or `psql` to import the database from the exported file.

6. Update configuration files: Update the configuration files on the new host to reflect the new database connection details and any other necessary changes. The configuration files are typically located in the GLPI directory.

7. Set file permissions: Set the appropriate file permissions on the GLPI files and directories to ensure they are accessible by the web server.

8. Test the migration: Access the GLPI installation on the new host using a web browser and verify that everything is working correctly. Test various functionalities to ensure that the migration was successful.

Note: The specific commands and steps may vary depending on your operating system and setup. Make sure to refer to the documentation specific to your environment for detailed instructions.


## Full backup of the GLPI database

1. Open a terminal or command prompt on the server where GLPI is installed.

2. Run the following command to create a full backup of the GLPI database:

   ```bash
   mysqldump -u {{username}} -p {{database_name}} > glpi_backup.sql
   ```

   Replace `{{username}}` with the username of the MySQL user with sufficient privileges to access the GLPI database, and `{{database_name}}` with the name of the GLPI database.

   You will be prompted to enter the password for the MySQL user. Enter the password and press Enter.

3. The `mysqldump` command will create a backup file named `glpi_backup.sql` in the current directory. This file contains the SQL statements necessary to recreate the GLPI database.

4. Move the backup file to a safe location or copy it to an external storage device to ensure its safety.


## Import a database into GLPI

1. Create a backup of the database you want to import. This backup should be in a format that can be imported into the database management system used by GLPI (e.g., SQL dump file).

2. Transfer the backup file to the server where GLPI is installed. You can use tools like `scp` or `rsync` to copy the file to the appropriate location.

3. Access the command line interface of the database management system used by GLPI. For example, if you are using MySQL, you can use the `mysql` command.

4. Create a new empty database for GLPI, if you haven't already done so. You can use a command like `CREATE DATABASE glpi;` to create a new database named "glpi".

5. Import the backup file into the newly created database. The command to import the file depends on the format of the backup file. For example, if you have a SQL dump file named "glpi_backup.sql", you can use a command like `mysql -u {{username}} -p {{database_name}} < glpi_backup.sql` to import the file.

   Replace `{{username}}` with the username of the database user with sufficient privileges to access the GLPI database, and `{{database_name}}` with the name of the GLPI database.

   You will be prompted to enter the password for the database user. Enter the password and press Enter.

Note: The specific commands and steps may vary depending on the database management system you are using and your setup. Make sure to refer to the documentation specific to your environment for detailed instructions.


## Change the database configuration for GLPI

1. Locate the GLPI configuration file. In a typical installation, the configuration file is named `/var/www/glpi/config/config_db.php`.

2. Open the `config_db.php` file using a text editor.

3. Modify the following values according to your new database configuration:

   ```php
   public $dbhost = 'localhost';
   public $dbuser = 'user';
   public $dbpassword = 'password';
   public $dbdefault = 'glpi_db';
   ```

   These lines specify the database server, username, password, and database name used by GLPI.


4. Save the changes to the `config_db.php` file.

5. Restart the web server to apply the changes.

After updating the configuration file and restarting the web server, GLPI should start using the new database configuration.
Make sure that the new database server is accessible from the GLPI server and that the specified username and password have the necessary privileges to access the new database.


## Change database user password

To change the password for a user in MySQL, you can follow these steps:

1. Open a terminal or command prompt on the server where MySQL is installed.

2. Log in to the MySQL server as the root user or a user with sufficient privileges. You can use the following command:

   ```bash
   mysql -u root -p
   ```

   You will be prompted to enter the password for the root user. Enter the password and press Enter.

3. Once you are logged in to the MySQL server, switch to the database where the user's password needs to be changed. For example, if the user is in the `glpi` database, you can use the following command:

   ```sql
   USE glpi;
   ```

   Replace `glpi` with the actual name of the database.

4. Change the password for the user using the `ALTER USER` statement. For example, if the user you want to change the password for is named `glpi_user` and you want to set the new password to `new_password`, you can use the following command:

   ```sql
   ALTER USER 'glpi_user'@'localhost' IDENTIFIED BY 'new_password';
   ```

   Replace `glpi_user` with the actual username, `localhost` with the appropriate hostname or IP address, and `new_password` with the desired new password.

5. Flush the privileges to ensure that the changes take effect:

   ```sql
   FLUSH PRIVILEGES;
   ```

6. Exit the MySQL server:

   ```sql
   EXIT;
   ```

   This will return you to the command prompt.
