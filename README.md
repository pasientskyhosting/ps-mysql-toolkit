
ps-mysql-toolkit
----------------------

Alpine Docker image to run [Percona Toolkit commands][percona-toolkit], [MySQLTuner][mysqltuner] and the MySQL backup tool [mydumper][mydumper].


How to get?
-----------

**1) Run a container**

The first method of get the images is directly run a container with the desired tag like the example below:

    docker run --rm -it pasientskyhosting/ps-mysql-toolkit pt-query-digest --help

This command will pull the `:latest` image (if you do not have it already) and execute `pt-query-digest --help` command.

**2) Pull the image**

You can also pull the desired image tag from the Docker Hub with:

    docker pull pasientskyhosting/ps-mysql-toolkit:1.0

This command will pull the image (if you do not have it already) without creating any container like the first method does.

**3) Build it**

Finally you can build the needed image from scratch. You will need to clone `pasientskyhosting/ps-mysql-toolkit` from GitHub and use Docker build command:

    docker build -t pasientskyhosting/ps-mysql-toolkit .

## percona-toolkit commands
```
pt-align
pt-archiver
pt-config-diff
pt-deadlock-logger
pt-diskstats
pt-duplicate-key-checker
pt-fifo-split
pt-find
pt-fingerprint
pt-fk-error-logger
pt-heartbeat
pt-index-usage
pt-ioprofile
pt-kill
pt-mext
pt-mongodb-query-digest
pt-mongodb-summary
pt-mysql-summary
pt-online-schema-change
pt-pmp
pt-query-digest
pt-show-grants
pt-sift
pt-slave-delay
pt-slave-find
pt-slave-restart
pt-stalk
pt-summary
pt-table-checksum
pt-table-sync
pt-table-usage
pt-upgrade
pt-variable-advisor
pt-visual-explain
```

## Mydumper
mydumper is a tool used for backing up MySQL database servers much
faster than the mysqldump tool distributed with MySQL. It also has the
capability to retrieve the binary logs from the remote server at the same time
as the dump itself. The advantages of mydumper are:

- Parallelism (hence, speed) and performance (avoids expensive character set conversion routines, efficient code overall)
- Easier to manage output (separate files for tables, dump metadata, etc, easy to view/parse data)
- Consistency - maintains snapshot across all threads, provides accurate master and slave log positions, etc
- Manageability - supports PCRE for specifying database and tables inclusions and exclusions

## mydumper usage

```
$ docker exec -it pasientskyhosting/ps-mysql-toolkit:1.0 mydumper --help
Usage:
  mydumper [OPTION*] multi-threaded MySQL dumping

Help Options:
  -?, --help                  Show help options

Application Options:
  -B, --database              Database to dump
  -T, --tables-list           Comma delimited table list to dump (does not exclude regex option)
  -O, --omit-from-file        File containing a list of database.table entries to skip, one per line (skips before applying regex option)
  -o, --outputdir             Directory to output files to
  -s, --statement-size        Attempted size of INSERT statement in bytes, default 1000000
  -r, --rows                  Try to split tables into chunks of this many rows. This option turns off --chunk-filesize
  -F, --chunk-filesize        Split tables into chunks of this output file size. This value is in MB
  -c, --compress              Compress output files
  -e, --build-empty-files     Build dump files even if no data available from table
  -x, --regex                 Regular expression for 'db.table' matching
  -i, --ignore-engines        Comma delimited list of storage engines to ignore
  -N, --insert-ignore         Dump rows with INSERT IGNORE
  -m, --no-schemas            Do not dump table schemas with the data
  -d, --no-data               Do not dump table data
  -G, --triggers              Dump triggers
  -E, --events                Dump events
  -R, --routines              Dump stored procedures and functions
  -W, --no-views              Do not dump VIEWs
  -k, --no-locks              Do not execute the temporary shared read lock.  WARNING: This will cause inconsistent backups
  --no-backup-locks           Do not use Percona backup locks
  --less-locking              Minimize locking time on InnoDB tables.
  -l, --long-query-guard      Set long query timer in seconds, default 60
  -K, --kill-long-queries     Kill long running queries (instead of aborting)
  -D, --daemon                Enable daemon mode
  -I, --snapshot-interval     Interval between each dump snapshot (in minutes), requires --daemon, default 60
  -L, --logfile               Log file name to use, by default stdout is used
  --tz-utc                    SET TIME_ZONE='+00:00' at top of dump to allow dumping of TIMESTAMP data when a server has data in different time zones or data is being moved between servers with different time zones, defaults to on use --skip-tz-utc to disable.
  --skip-tz-utc
  --use-savepoints            Use savepoints to reduce metadata locking issues, needs SUPER privilege
  --success-on-1146           Not increment error count and Warning instead of Critical in case of table doesn't exist
  --lock-all-tables           Use LOCK TABLE for all, instead of FTWRL
  -U, --updated-since         Use Update_time to dump only tables updated in the last U days
  --trx-consistency-only      Transactional consistency only
  --complete-insert           Use complete INSERT statements that include column names
  -h, --host                  The host to connect to
  -u, --user                  Username with the necessary privileges
  -p, --password              User password
  -a, --ask-password          Prompt For User password
  -P, --port                  TCP/IP port to connect to
  -S, --socket                UNIX domain socket file to use for connection
  -t, --threads               Number of threads to use, default 4
  -C, --compress-protocol     Use compression on the MySQL connection
  -V, --version               Show the program version and exit
  -v, --verbose               Verbosity of output, 0 = silent, 1 = errors, 2 = warnings, 3 = info, default 2
  --defaults-file             Use a specific defaults file
```

## myloader usage
```
$ docker exec -it pasientskyhosting/ps-mysql-toolkit:1.0 myloader --help
Usage:
  myloader [OPTION*] multi-threaded MySQL loader

Help Options:
  -?, --help                        Show help options

Application Options:
  -d, --directory                   Directory of the dump to import
  -q, --queries-per-transaction     Number of queries per transaction, default 1000
  -o, --overwrite-tables            Drop tables if they already exist
  -B, --database                    An alternative database to restore into
  -s, --source-db                   Database to restore
  -e, --enable-binlog               Enable binary logging of the restore data
  -h, --host                        The host to connect to
  -u, --user                        Username with the necessary privileges
  -p, --password                    User password
  -a, --ask-password                Prompt For User password
  -P, --port                        TCP/IP port to connect to
  -S, --socket                      UNIX domain socket file to use for connection
  -t, --threads                     Number of threads to use, default 4
  -C, --compress-protocol           Use compression on the MySQL connection
  -V, --version                     Show the program version and exit
  -v, --verbose                     Verbosity of output, 0 = silent, 1 = errors, 2 = warnings, 3 = info, default 2
  --defaults-file                   Use a specific defaults file
```

## MySQLTuner
MySQLTuner is a script written in Perl that allows you to review a MySQL installation quickly and make adjustments to increase performance and stability. The current configuration variables and status data is retrieved and presented in a brief format along with some basic performance suggestions.

MySQLTuner supports ~300 indicators for MySQL/MariaDB/Percona Server in this last version.

## mysqltuner usage

```
$ docker exec -it pasientskyhosting/ps-mysql-toolkit:1.0 mysqltuner --help
Name:
     MySQLTuner 1.7.19 - MySQL High Performance Tuning Script
Important Usage Guidelines:
    To run the script with the default options, run the script without
    arguments Allow MySQL server to run for at least 24-48 hours before
    trusting suggestions Some routines may require root level privileges
    (script will provide warnings) You must provide the remote server's
    total memory when connecting to other servers

Connection and Authentication:
     --host <hostname>           Connect to a remote host to perform tests (default: localhost)
     --socket <socket>           Use a different socket for a local connection
     --port <port>               Port to use for connection (default: 3306)
     --user <username>           Username to use for authentication
     --userenv <envvar>          Name of env variable which contains username to use for authentication
     --pass <password>           Password to use for authentication
     --passenv <envvar>          Name of env variable which contains password to use for authentication
     --ssl-ca <path>             Path to public key
     --mysqladmin <path>         Path to a custom mysqladmin executable
     --mysqlcmd <path>           Path to a custom mysql executable
     --defaults-file <path>      Path to a custom .my.cnf

Performance and Reporting Options:
     --skipsize                  Don't enumerate tables and their types/sizes (default: on)
                                 (Recommended for servers with many tables)
     --skippassword              Don't perform checks on user passwords(default: off)
     --checkversion              Check for updates to MySQLTuner (default: don't check)
     --updateversion             Check for updates to MySQLTuner and update when newer version is available (default: don't check)
     --forcemem <size>           Amount of RAM installed in megabytes
     --forceswap <size>          Amount of swap memory configured in megabytes
     --passwordfile <path>       Path to a password file list(one password by line)

Output Options:
     --silent                    Don't output anything on screen
     --nogood                    Remove OK responses
     --nobad                     Remove negative/suggestion responses
     --noinfo                    Remove informational responses
     --debug                     Print debug information
     --noprocess                Consider no other process is running
     --dbstat                    Print database information
     --nodbstat                  Don't Print database information
     --tbstat                    Print table information
     --notbstat                  Don't Print table information
     --idxstat                   Print index information
     --noidxstat                 Don't Print index information
     --sysstat                   Print system information
     --nosysstat                 Don't Print system information
     --pfstat                    Print Performance schema
     --nopfstat                  Don't Print Performance schema
     --verbose                   Prints out all options (default: no verbose, dbstat, idxstat, sysstat, tbstat, pfstat)
     --bannedports               Ports banned separated by comma(,)
     --maxportallowed            Number of ports opened allowed on this hosts
     --cvefile <path>            CVE File for vulnerability checks
     --nocolor                   Don't print output in color
     --json                      Print result as JSON string
     --buffers                   Print global and per-thread buffer values
     --outputfile <path>         Path to a output txt file
     --reportfile <path>         Path to a report txt file
     --template   <path>         Path to a template file
```




## Useful Examples
**pt-show-grants extracts, orders, and then prints grants for MySQL user accounts.**
```
docker run --name ps-mysql-toolkit pasientskyhosting/ps-mysql-toolkit pt-show-grants h=127.0.0.1,u=dbmanager,p=Sup3rS3cr3T
```

**mydumper - dump databases with --regex functionality**
```
docker run \
  --name mydumper \
  --rm \
  -v /some/backup/directory:/backups \
  pasientskyhosting/ps-mysql-toolkit \
 mydumper \
  --host=127.0.0.1 \
  --user=dbmanager \
  --password=Sup3rS3cr3T \
  --regex '^(?!(mysql\.|test\.))' \
  --outputdir=/backups \
  --less-locking \
  --compress
```
**myloader - load databases**
```
docker run \
  --name myloader \
  --rm \
  -v /some/backup/directory:/backups \
  pasientskyhosting/ps-mysql-toolkit \
 myloader \
  --host=127.0.0.1 \
  --user=dbmanager \
  --password=Sup3rS3cr3T \
  --directory=/backups
```
[percona-toolkit]: https://www.percona.com/doc/percona-toolkit/3.0/index.html
[percona-toolkit manual]: https://learn.percona.com/hubfs/Manuals/Percona_Toolkit/Percona-Toolkit-3-0/PerconaToolkit-3.0.13.pdf
[mydumper]: https://github.com/maxbube/mydumper
[mysqltuner]: https://github.com/major/MySQLTuner-perl 