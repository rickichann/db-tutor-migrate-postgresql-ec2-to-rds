# db-tutor-migrate-postgresql-ec2-to-rds


<img width="911" alt="image" src="https://github.com/user-attachments/assets/91752ced-0645-46e8-a842-9b56e2c45682" />


Migrating PostgreSQL from EC2 to AWS RDS with a Complete Backup and Restore Guide


Why Migrate from PostgreSQL on EC2 to AWS RDS?

Managing PostgreSQL on an EC2 instance provides flexibility, but it also comes with challenges such as manual maintenance, scaling issues, and high availability concerns. By migrating to Amazon RDS, users can take advantage of automated backups, easy scalability, and built-in high availability features. This guide provides a step-by-step process to back up your PostgreSQL database from an EC2 instance and restore it to an Amazon RDS instance.



## Backup PostgreSQL Database

### Step 1: Access AWS Systems Manager (SSM)
Use AWS SSM to log in to the PostgreSQL EC2 instance.

### Step 2: Run the Backup Command
Run the following command to access PostgreSQL:
```sh
sudo -i -u postgres
```
Then, execute the following command to create a database dump:
```sh
pg_dumpall -U postgres -f /var/tmp/dump.sql -v
```
The dump file will be saved in the `/var/tmp` directory with the name `dump.sql`.

---

## Restore PostgreSQL Database to RDS

### Step 1: Access AWS Systems Manager (SSM)
Use AWS SSM to log in to the PostgreSQL EC2 instance.

### Step 2: Ensure EC2 Connectivity to RDS
Check the connectivity between EC2 and RDS using the following command:
```sh
telnet <rds_endpoint> 5432
```
If the connection is successful, you will receive a response from the PostgreSQL server.

### Step 3: Execute the Restore Process
Use the following command to restore the database:
```sh
PGPASSWORD="<password>" psql -h <rds_endpoint> -U postgres -f /var/tmp/dump.sql
```
Ensure that the dump file (`dump.sql`) is available in the `/var/tmp` directory before executing this command.

### Step 4: Validate the Restore Result
After the restore process is complete, validate the data by logging into the database and checking the records:
```sh
PGPASSWORD="<password>" psql -h <rds_endpoint> -U postgres
```
Then, run the following SQL commands to verify that the data has been restored:
```sql
\l   -- List all databases
\dt  -- List all tables in the database
SELECT COUNT(*) FROM <table_name>;  -- Check the number of records in a table
```

If all data has been successfully restored, the process is complete.

---

## Additional Notes
- Replace `<password>`, `<username>`, `<rds_endpoint>`, and `<table_name>` with your actual configuration.
- Ensure you have the necessary permissions to perform database backup and restore operations.

---

This documentation is intended to assist in the PostgreSQL database backup and restore process in an AWS environment. If you encounter any issues, check PostgreSQL logs or AWS CloudWatch for detailed error messages.

