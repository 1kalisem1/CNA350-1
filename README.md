#### CNA350-1 MariaDB Docker image and DatabaseShard

This docker image runs the latest version of MariaDB and MaxScale

### Step 1: You would need to run and install 

#sudo apt install git
#sudo apt install docker
#sudo apt install docker-compose
#sudo apt install mariadb-client
#sudo apt install mariadb-server
#sudo apt install net-tools

## Fork the maxscale repository from https://github.com/gustanik/CNA350 then run the following command

#git clone https://github.com/1kalisem1/CNA350-1/

## Switch to git directory

#cd CNA350-1/maxscale
kalisem11@kalisem11:~/CNA350-1/maxscale

### Step 2: You would need to modify the docker-compose file.yml to support PHPMyAdmin and move the architecture from master-salve to shardded by editing the docker-compose.yml and maxscale.cnf files

## Modify the files by running the following commands 

#sudo nano docker-compose.yml
#sudo nano maxscale.cnf

## To check on the status of a container and to start the constainers use the following commands

#sudo docker ps
       Name                   Command            State            Ports         
--------------------------------------------------------------------------------
maxscale_master2_1    docker-entrypoint.sh       Up      0.0.0.0:4003->3306/tcp 
                      mysql ...                                                 
maxscale_master_1     docker-entrypoint.sh       Up      0.0.0.0:4001->3306/tcp 
                      mysql ...                                                 
maxscale_maxscale_1   docker-entrypoint.sh       Up      0.0.0.0:3306->3306/tcp,
                      maxsc ...                          0.0.0.0:4006->4006/tcp,
                                                         0.0.0.0:4007->4007/tcp,
                                                         0.0.0.0:8989->8989/tcp 
maxscale_slave1_1     docker-entrypoint.sh       Up      0.0.0.0:4002->3306/tcp 
                      mysql ...                                                 
maxscale_slave2_1     docker-entrypoint.sh       Up      0.0.0.0:4004->3306/tcp 
                      mysql ...                                                 
phpmyadmin            /docker-entrypoint.sh      Up      0.0.0.0:8080->80/tcp   
                      apac ...                                                  

#sudo docker-compose up -d 

## To logon to PHPMYAdmin go to http://127.0.0.1:8080 

User: maxuser
Pass: maxpwd

## To check the MaxScale server run the following command

#sudo docker-compose exec maxscale list servers
┌─────────┬───────────┬──────┬─────────────┬─────────────────┬───────────┐
│ Server  │ Address   │ Port │ Connections │ State           │ GTID      │
├─────────┼───────────┼──────┼─────────────┼─────────────────┼───────────┤
│ server1 │ master    │ 3306 │ 0           │ Master, Running │ 0-3000-32 │
├─────────┼───────────┼──────┼─────────────┼─────────────────┼───────────┤
│ server2 │ slave1    │ 3306 │ 0           │ Slave, Running  │ 0-3000-32 │
├─────────┼───────────┼──────┼─────────────┼─────────────────┼───────────┤
│ server3 │ master2   │ 3306 │ 0           │ Master, Running │ 0-3002-31 │
├─────────┼───────────┼──────┼─────────────┼─────────────────┼───────────┤
│ server4 │ slave2    │ 3306 │ 0           │ Slave, Running  │ 0-3002-31 │
├─────────┼───────────┼──────┼─────────────┼─────────────────┼───────────┤
│ Shard-A │ 127.0.0.1 │ 4006 │ 0           │ Running         │           │
├─────────┼───────────┼──────┼─────────────┼─────────────────┼───────────┤
│ Shard-B │ 127.0.0.1 │ 4007 │ 0           │ Running         │           │
└─────────┴───────────┴──────┴─────────────┴─────────────────┴───────────┘

## To list the databases that exist on the maxscale/slq directory run the following command

#mysql -umaxuser -pmaxpwd -h 127.0.0.1 -P 3306 -e "show databases"
+--------------------+
| Database           |
+--------------------+
| mysql              |
| information_schema |
| performance_schema |
| zipcodes_one       |
| zipcodes_two       |
+--------------------+

## To access master shard database from zipcode one run the following command

#mysql -u maxuser -pmaxpwd -h 127.0.0.1 -P 3306 -e "SELECT * FROM zipcodes_one.zipcodes_one LIMIT 10;"
+---------+-------------+-----------+-------+--------------+-----------+------------+--------------------+---------------+-----------------+---------------------+------------+
| Zipcode | ZipCodeType | City      | State | LocationType | Coord_Lat | Coord_Long | Location           | Decommisioned | TaxReturnsFiled | EstimatedPopulation | TotalWages |
+---------+-------------+-----------+-------+--------------+-----------+------------+--------------------+---------------+-----------------+---------------------+------------+
|     705 | STANDARD    | AIBONITO  | PR    | PRIMARY      | 18.14     | -66.26     | NA-US-PR-AIBONITO  | FALSE         |                 |                     |            |
|     610 | STANDARD    | ANASCO    | PR    | PRIMARY      | 18.28     | -67.14     | NA-US-PR-ANASCO    | FALSE         |                 |                     |            |
|     611 | PO BOX      | ANGELES   | PR    | PRIMARY      | 18.28     | -66.79     | NA-US-PR-ANGELES   | FALSE         |                 |                     |            |
|     612 | STANDARD    | ARECIBO   | PR    | PRIMARY      | 18.45     | -66.73     | NA-US-PR-ARECIBO   | FALSE         |                 |                     |            |
|     601 | STANDARD    | ADJUNTAS  | PR    | PRIMARY      | 18.16     | -66.72     | NA-US-PR-ADJUNTAS  | FALSE         |                 |                     |            |
|     631 | PO BOX      | CASTANER  | PR    | PRIMARY      | 18.19     | -66.82     | NA-US-PR-CASTANER  | FALSE         |                 |                     |            |
|     602 | STANDARD    | AGUADA    | PR    | PRIMARY      | 18.38     | -67.18     | NA-US-PR-AGUADA    | FALSE         |                 |                     |            |
|     603 | STANDARD    | AGUADILLA | PR    | PRIMARY      | 18.43     | -67.15     | NA-US-PR-AGUADILLA | FALSE         |                 |                     |            |
|     604 | PO BOX      | AGUADILLA | PR    | PRIMARY      | 18.43     | -67.15     | NA-US-PR-AGUADILLA | FALSE         |                 |                     |            |
|     605 | PO BOX      | AGUADILLA | PR    | PRIMARY      | 18.43     | -67.15     | NA-US-PR-AGUADILLA | FALSE         |                 |                     |            |
+---------+-------------+-----------+-------+--------------+-----------+------------+--------------------+---------------+-----------------+---------------------+------------+

## To access master shard database from zipcode two run the following command

#mysql -u maxuser -pmaxpwd -h 127.0.0.1 -P 3306 -e "SELECT * FROM zipcodes_two.zipcodes_two LIMIT 10;"
+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+
| Zipcode | ZipCodeType | City        | State | LocationType | Coord_Lat | Coord_Long | Location             | Decommisioned | TaxReturnsFiled | EstimatedPopulation | TotalWages |
+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+
|   42040 | STANDARD    | FARMINGTON  | KY    | PRIMARY      | 36.67     | -88.53     | NA-US-KY-FARMINGTON  | FALSE         | 465             | 896                 | 11562973   |
|   41524 | STANDARD    | FEDSCREEK   | KY    | PRIMARY      | 37.4      | -82.24     | NA-US-KY-FEDSCREEK   | FALSE         |                 |                     |            |
|   42533 | STANDARD    | FERGUSON    | KY    | PRIMARY      | 37.06     | -84.59     | NA-US-KY-FERGUSON    | FALSE         | 429             | 761                 | 9555412    |
|   40022 | STANDARD    | FINCHVILLE  | KY    | PRIMARY      | 38.15     | -85.31     | NA-US-KY-FINCHVILLE  | FALSE         | 437             | 839                 | 19909942   |
|   40023 | STANDARD    | FISHERVILLE | KY    | PRIMARY      | 38.16     | -85.42     | NA-US-KY-FISHERVILLE | FALSE         | 1884            | 3733                | 113020684  |
|   41743 | PO BOX      | FISTY       | KY    | PRIMARY      | 37.33     | -83.1      | NA-US-KY-FISTY       | FALSE         |                 |                     |            |
|   41219 | STANDARD    | FLATGAP     | KY    | PRIMARY      | 37.93     | -82.88     | NA-US-KY-FLATGAP     | FALSE         | 708             | 1397                | 20395667   |
|   40935 | STANDARD    | FLAT LICK   | KY    | PRIMARY      | 36.82     | -83.76     | NA-US-KY-FLAT LICK   | FALSE         | 752             | 1477                | 14267237   |
|   40997 | STANDARD    | WALKER      | KY    | PRIMARY      | 36.88     | -83.71     | NA-US-KY-WALKER      | FALSE         |                 |                     |            |
|   41139 | STANDARD    | FLATWOODS   | KY    | PRIMARY      | 38.51     | -82.72     | NA-US-KY-FLATWOODS   | FALSE         | 3692            | 6748                | 121902277  |
+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+

## To remove the cluster and maxscale containers once done run the following command

#sudo docker-compose down -v


