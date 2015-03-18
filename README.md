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

    CHANGE MASTER TO 
    MASTER_HOST='172.16.0.102', 
    MASTER_USER='replication_user', 
    MASTER_PASSWORD='password', 
    MASTER_PORT=3306, 
    MASTER_LOG_FILE='mariadb-bin.000001', 
    MASTER_LOG_POS=556, 
    MASTER_CONNECT_RETRY=10;
