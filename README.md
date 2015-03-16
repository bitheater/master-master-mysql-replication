# CentOS 7 MariaDB (MySQL) Master - Master replication
##Install MariaDB

    yum install mariadb-server
    service mariadb start
    mysql -u root
    
## Configuration
### Edit MariaDB configuration file

    vi /etc/my.cnf

    #Enable binary loggin
    log-basename=replication-node
    log-bin
    binlog-format=row
    #Set server unique id
    server_id=1
    skip-networking=0

### Grant replication rights to the replication user
    
    GRANT REPLICATION SLAVE ON *.* TO 'replication_user'@'%' IDENTIFIED BY 'password';
    
### On the master, flush and lock all tables by running 
    
    FLUSH TABLES WITH READ LOCK;
    
### Get the current position in the binary log by running 
    
    SHOW MASTER STATUS;
    +--------------------+----------+--------------+------------------+
    | File               | Position | Binlog_Do_DB | Binlog_Ignore_DB |
    +--------------------+----------+--------------+------------------+
    | mariadb-bin.00001  |      101 |              |                  |
    +--------------------+----------+--------------+------------------+
