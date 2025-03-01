# db-tutor-migrate-postgresql-ec2-to-rds


<img width="850" alt="image" src="https://github.com/user-attachments/assets/5b994348-e5bd-4763-b821-f1f95c914633" />


# Guide to Backup and Restore PostgreSQL Database on AWS

## Backup PostgreSQL Database

### Step 1: Access AWS Systems Manager (SSM)
Use AWS SSM to log in to the EC2 instance that will be used for database backup.

### Step 2: Run the Backup Command
Run the following command to access PostgreSQL:
```sh
sudo -i -u postgres psql
```
Then, execute the following command to create a database dump:
```sh
sudo -u postgres PGPASSWORD="<password>" pg_dumpall -U <username> -f /tmp/dump.sql
```
The dump file will be saved in the `/tmp` directory with the name `sl_dump_v1.sql`.

---

## Restore PostgreSQL Database to RDS

### Step 1: Access AWS Systems Manager (SSM)
Use AWS SSM to log in to the EC2 instance that will be used for database restoration.

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
Ensure that the dump file (`sl_trial_dump.sql`) is available in the `/var/tmp` directory before executing this command.

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
- To enhance security, avoid storing passwords in direct commands. Use configuration files or environment variables instead.
- If you have tables in the `postgres`, `template0`, or `template1` databases, they will not be migrated. You must move those tables to a new database manually.

---

This documentation is intended to assist in the PostgreSQL database backup and restore process in an AWS environment. If you encounter any issues, check PostgreSQL logs or AWS CloudWatch for detailed error messages.

