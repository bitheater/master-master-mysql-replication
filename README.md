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





# master-master-mysql-replication
Step by Step installation and configuration of a master -master mysql replication

    yum install mariadb-server
    service mariadb start
    mysql -u root
    
    CREATE USER 'slave_user'@'172.16.0.102' IDENTIFIED BY 'password';
    GRANT REPLICATION SLAVE ON *.* TO 'slave_user'@'%' IDENTIFIED BY 'password'
    
    CHANGE MASTER TO MASTER_HOST='172.16.0.102' , MASTER_USER='slave_user',MASTER_PASSWORD='password',MASTER_LOG_FILE='mariadb-bin.000001', MASTER_LOG_POS=1011;
    
    vi /etc/my.conf.d/server.cnf
    
    bind-address = 0.0.0.0
    server-id = 1
    slave-net-timeout = 30
    replicate-do-db = database_name
    log-bin = /var/log/mariadb/mariadb-bin.log
    log-bin-index = /var/log/mariadb/master-log-bin.index
    binlog-do-db = database_name
    relay-log = /var/lib/mysqld-relay-bin
    relay-log = /var/lib/mysql/slave-relay.log
    relay-log-index = /var/lib/mysql/slave-relay-log.index
    
    service mariadb restart
    
