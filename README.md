# mysql-sync

Syncs two mysql databases by only inserting/deleting/updating the necessary records.


Useful in cases where mysqldump generates gigabytes of data

##Usage:

`mysql-sync [source-dsn] [target-dsn] [output-option]`

dsn being `username:password@hostname:port/database_name`

output-option `verbose outputs all output of INSERTs UPDATEs etc, json provides JSON encoded output for a return value to another script otherwise a minimal table update  output is the resulting output`

Example: `mysql-sync root:password@localhost:port/source_db root:password@localhost:port/dest_db json`


If the remote mysql port is not open, tunnel it via SSH i.e 
`ssh -L 3308:localhost:3306 -o TCPKeepAlive=no -o ServerAliveInterval=15 user@host`
then `mysql-sync root:password@localhost:3308/voeb root:password@localhost:3306/v verbose`


## What the script does


- lists all tables on the remote and local host
- compares the checksum of each mysql table
- syncs the tables where checksum is invalid
- verifies the sync was successful via another checksum


The databases *must* have the same schema. Therefore, the script will stop if

- the number of tables is not similar 
- the structure of a table is not similar i.e describe my_table
