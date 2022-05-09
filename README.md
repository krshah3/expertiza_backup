# Backups

## Database backup
- What databases are backed up
    - expertiza_production
    - pg_wiki
- Creation script `create_database_backup.py`
- Deletion Script `delete_database_backup.sh`
- Backup Strategy
    - Daily till the last weekly
    - Weekly till the last monthly
    - Monthly till the end of last semester
- Directory Structure
    ```bash
     |--backups/
        |--database/
            |--daily/
            |--weekly/
            |--monthly/
    ```

## Filepsace backup
- Expertiza filespace is on the Expertiza server (expertiza.csc.ncsu.edu) at /var/www/expertiza/current/pg_data/
- Creation script `create_filespace_backup.sh`
- Deletion Script `delete_filespace_backup.sh`
- Bakcup Strategy
    - Daily incremental backup
    - Monthly full backup
- Directory Structure
    ```bash
     |--backups/
        |--filespace/
            |--incr/
            |--full/
    ```
## Backup Location

- We are using NCSU's <a href="https://oit.ncsu.edu/my-it/teaching-research/research-storage/">Research Storage</a> to store our backups. 
- Storage size: 2TB
- Server: expertiza.csc.ncsu.edu
- Mount Point: /mnt/efg

## Automation
- We are automating running the above scripts using cronjobs
- The commands required to run the script are as below

```bash
# Database: daily deletion
0 19 * * * /usr/bin/python /home/efg/backup_scripts/create_database_backup.py daily

# Database: weekly deletion
0 20 * * 3 /usr/bin/python /home/efg/backup_scripts/create_database_backup.py weekly

# Database: monthly deletion
0 8 1 * * /usr/bin/python /home/efg/backup_scripts/create_database_backup.py monthly

# Database: daily deletion 
0 22 * * * /home/efg/backup_scripts/delete_database_backups.sh

# Filespace: Daily incremental backups
0 0 * * * /home/efg/backup_scripts/create_filespace_backup.sh incr > /dev/null 2>&1

# Filespace: Monthly full backups
0 0 1 * * /home/efg/backup_scripts/create_filespace_backup.sh full zip > /dev/null 2>&1

# Filespace: Monthly backups deletion
0 0 1 * * /home/efg/backup_scripts/delete_filespace_backup.sh > /dev/null 2>&1
```

- A reference to the cron format can be found <a href="https://crontab.guru/">here</a>.

## Contact

- Research Storage Issues
    - <a href="https://oit.ncsu.edu/help-support/">NCSU IT Desk</a>
- All other issues
    - Karan Shah (krshah3@ncsu.edu)
    - Sneha Aradhey (svaradhe@ncsu.edu)
    - Chinmay Terse (cterse@ncsu.edu)

