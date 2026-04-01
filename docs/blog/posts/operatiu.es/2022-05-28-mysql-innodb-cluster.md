---
date: 
    created: 2022-05-28

title: 'MySQL InnoDB Cluster'

author: jmong

layout: post

categories:
    - Informàtica
    - mysql
    - Portada
tags:
    - altadisponibilitat
    - bbdd
    - cluster
    - database
    - group_replication
    - high_availability
    - mysql
    - mysql-shell
    - mysqlrouter
    - router
    - thebluebox
---

## Alta disponibilitat

Permetre oferir l’accés al sistema o recursos de forma continuada i sense errors, aturades ni pèrdues de fiabilitat i en cas que succeeixin que puguin tornar al seu estat òptim en el menor temps possible.

Una forma d’obtenir alta disponibilitat en MySQL és amb la utilització de MySQL Cluster per crear un grup de replicació o Group Replication i connectar amb els clients o les aplicacions, mitjançant un o diversos MySQL Router.

<a href="https://res.cloudinary.com/dbuv9r3p5/image/upload/mysql_innodb_cluster_i14mab.png" target="_blank">
  <img src="https://res.cloudinary.com/dbuv9r3p5/image/upload/w_400/mysql_innodb_cluster_i14mab.png">
</a>

El nostre projecte consistirà en crear un cluster de 3 servidors MySQL 8 en 3 MVs Debian server, amb mysql-shell i una altra Màquina Virtual que contindrà MySQL Router i MySQL client.

### Configuracions prèvies

>[!Nota]
La configuració de xarxa serà la següent:

| MV | Tipus | IP | Permís | ID |
|--|--|--|--|--|
| Server 0 (s0) | Cluster00 | 172.10.0.100 | R/W | server_id = 10 |
| Server 1 (s1) | Cluster01 | 172.10.0.101 | R/O | server_id = 11 |
| Server 2 (s2) | Cluster02 | 172.10.0.102 | R/O | server_id = 12 |
| Router (r0) | Router | 172.10.0.254 | | |

Instal·larem les Virtualbox guest additions en cada màquina

Instal·larem module-assistant i build-essential en cada servidor

<details>
    <summary>Mostra el codi</summary>
     <pre><code class="language-bash">
sudo m-a prepare  
[ Instal·lar guest additions ]
sudo su  
mount -t iso9660 -o loop /dev/sr0 /media/cdrom  
cd /media/cdrom  
sh VBoxLinuxAdditions.run  
cd  
umount /media/cdrom  
eject /dev/sr0 
     </code></pre>
</details>


Configuració de mostra d’un dels arxius de configuració de xarxa en **/etc/network/interfaces**

<details>
    <summary>Mostra el codi</summary>
     <pre><code class="language-bash">
auto lo  
iface lo inet loopback auto enp0s3  
iface enp0s3 inet dhcp

auto enp0s8  
iface enp0s8 inet static  
address 172.10.0.100  
netmask 255.255.255.0  
network 172.10.0.0  
broadcast 172.10.0.255  
gateway 172.10.0.254  
up route add -net 172.10.0.0/24 gw 172.10.0.254 dev enp0s8
     </code></pre>
</details>


Configuració de l’arxiu /etc/hostname i /etc/hosts

```
hostname cluster00
```

```
127.0.0.1	cluster00	localhost
127.0.1.1	cluster00

# The following lines ...
::1 	localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
#
172.10.0.100	cluster00 s0
172.10.0.101	cluster00 s1
172.10.0.102	cluster00 s2
172.10.0.254    router r0
```

### Preparació del grup de replicació o cluster

1.- Install mysql-community-server i mysql-shell en les 3 MV (memoria ram 1GB)  
2.- Configuració de /etc/my.cnf als 3 utilitzant la IP de cadascú i el port 33061 per loose-group\_replication\_local\_address=172.10.0.10**?**:33061

<details>
    <summary>Mostra el codi</summary>
     <pre><code class="language-bash">
## mysql client settings  
[mysql] 
# 
## mysql server settings  
[mysqld]  
basedir = /usr  
datadir = /var/lib/mysql  
port = 3306  
bind-address = *  
pid_file = /var/run/mysqld/mysqld.pid  
log_error = /var/log/mysql/error.log
# 
## InnoDB tunning  
innodb_buffer_pool_size = 100M  
innodb_buffer_pool_instances = 2  
innodb_log_file_size = 25M  
innodb_log_files_in_group = 2
# 
## Replication settings  
binlog_format = ROW  
binlog_transaction_dependency_tracking = WRITESET  
enforce_gtid_consistency = ON  
gtid_mode = ON
# 
master_info_repository = TABLE  
relay_log_info_repository = TABLE  
binlog_checksum = NONE  
log_slave_updates = ON  
plugin-load = group_replication.so
# 
log_bin = /var/lib/mysql/mysql-bin  
log_bin_index = /var/lib/mysql/mysql-bin.index  
replica_parallel_type = LOGICAL\_CLOCK  
replica_parallel_workers = 4  
replica_preserve_commit_order = ON  
report_port = 3306  
server_id = 10  
transaction_write_set_extraction = XXHASH64
# 
loose-group_replication_start_on_boot = OFF  
loose-group_replication_local_address = 172.10.0.100:33061  
loose-group_replication_bootstrap_group = OFF  
report_port = 3306  
report_host = 172.10.0.100  
# afegit per prevenció error només en server R/W  
group_replication_ip_allowlist=172.10.0.100,172.10.0.101,172.10.0.102  
skip-name-resolve
     </code></pre>
</details>


Es pot obtenir amb l’arxiu de configuració buit (només amb el \[mysqld\] server\_id=valor) i al configurar cada instància afegir la ruta de l’arxiu /etc/my.cnf i en cas de problemes d’escriptura, derivar-lo a /tmp/my.cnf de forma temporal per poder copiar-lo.

```
dba.configureInstance('clustermanager@cluster00:3306',{mycnfPath:'/etc/my.cnf'})
```

3.- Creació de l’usuari ‘clustermanager’@’%’ amb passwd ‘@MVM2016’ als 3 servidors amb GRANT ALL PRIVILEGES … WITH GRANT OPTION.

```
CREATE USER 'clustermanager'@'%' Identified by '@MVM2016';
GRANT ALL PRIVILEGES ON *.* TO 'clustermanager'@'%' WITH GRANT OPTION;
```

  
4.- Amb connexió root@localhost en mode SQL a cada servidor enviem la comanda:

```
RESET MASTER;
```

5.- En el servidor 0 canviem a \\js i enviem les comandes:

```
     dba.configureInstance('clustermanager@cluster00:3306')
     dba.configureInstance('clustermanager@cluster01:3306')
     dba.configureInstance('clustermanager@cluster02:3306')
```

```
     dba.checkInstanceConfiguration()
```

Ens hauria de tornar per a cadascú:

```
{
	"status": "ok"
}
```

Nota: En cluster01 i cluster02 podem desar ‘Y’ el password, si volem.

6.- Sortim de mysqlsh com root i tornem a entrar com a clustermanager en servidor 0 (per defecte entrem en mode JS

```
mysqlsh clustermanager@cluster00:3306
var cluster = dba.createCluster('DBcluster', {ipWhitelist: '172.10.0.100,172.10.0.101,172.10.0.102'})
cluster.status()
```

Ens hauria d’apareixer en mode R/W el cluster00

7.- A continuació i des de la connexió del servidor 0 (cluster00) afegirem les instàncies del servidor 1 i el servidor 2

```
cluster.addInstance('clustermanager@cluster01',{ipWhitelist: '172.10.0.100,172.10.0.101,172.10.0.102'})
cluster.addInstance('clustermanager@cluster02',{ipWhitelist: '172.10.0.100,172.10.0.101,172.10.0.102'})
```

```
cluster.status()
```

Ara ens hauria de tornar els tres servidors, el primer en mode R/W i els altres dos en mode R/O

Amb les opcions, podem alterar el cluster i canviar a MultI Primary o Single Primary

```
cluster.switchToMultiPrimaryMode()   -- passariem els tres servers com mode R/W
cluster.switchToSinglePrimaryMode()  -- passariem a l'opció per defecte, 1 R/W i els altres R/O
```

Amb la següent comanda:

```
cluster.setPrimaryInstance('cluster01:3306') -- Indicarem que volem que el server 1 sigui R/W de forma manual o qualsevol altre que especifiquem.
```

En el server 1 entrant en mode \\sql

```
STOP GROUP_REPLICATION;
```

SET GLOBAL group\_replication\_ip\_whitelist = “172.10.0.100,172.10.0.101,172.10.0.102”;  
o millor amb (ja que l’anterior entra en deprecated)

```
SET GLOBAL group_replication_ip_allowlist = "172.10.0.100,172.10.0.101,172.10.0.102";
```

```
START GROUP_REPLICATION;
```

### Inicialització del cluster de servidors 

Després de tenir les màquines apagades i per continuar amb la nostra feina, necessitem initcialitzar el cluster.

1.- Entrem a mysql shell en la MV s0 o cluster00 i les altres MV també funcionant. Podem entrar simplement amb: **mysqlsh** sense afegir res més

```
mysqlsh clustermanager@cluster00:3306
```

2.- Utilitzem shell.connect

```
shell.connect('clustermanager@cluster00:3306')
```

3.- Definim la variable cluster amb el reinici del cluster

```
var cluster = dba.rebootClusterFromCompleteOutage('DBcluster')
```

<details>
    <summary>Mostra el codi</summary>
     <pre><code class="language-bash">
The instance ‘172.10.0.101:3306’ was part of the cluster configuration.  
Would you like to rejoin it to the cluster? [y/N]: y The instance ‘172.10.0.102:3306’ was part of the cluster configuration.  
Would you like to rejoin it to the cluster? \[y/N\]: y

* Waiting for seed instance to become ONLINE…  
…  
…  
Rejoining …  
Rejoining …  
…  
…  
The cluster was successfully rebooted.
     </code></pre>
</details>

4.- Demanem l’estat del cluster

```
cluster.status()
```

Hauria de tornar l’estat de les tres instàncies amb el mode R/W i R/O de cada server

Si els cluster00 no està com R/W el podem promoure.

```
cluster.setPrimaryInstance('cluster00:3306')
```

5.- Opcional

Passem a mode SQL amb: **\\sql** i executem la següent consulta:

```
SELECT MEMBER_ID, MEMBER_HOST, MEMBER_PORT, MEMBER_STATE, MEMBER_ROLE, MEMBER_VERSION FROM performance_schema.replication_group_members;
```

Haurien de sortir els tres ONLINE

<a href="https://res.cloudinary.com/dbuv9r3p5/image/upload/member-id_ki7cvl.png" target="_blank">
  <img src="https://res.cloudinary.com/dbuv9r3p5/image/upload/w_480/member-id_ki7cvl.png">
</a>


### Configuració de MySQL Router

Instal·lació de **mysql-router-community** (amb mysql-tools habilitat) i **mysql-community-client**

```
sudo dpkg-reconfigure mysql-apt-config<br></br>sudo apt install mysql-router-community mysql-community-client<br></br>sudo dpkg -l | grep mysql-
```

```
ii mysql-apt-config 0.8.22-1 all Auto configuration for MySQL APT Repo.
ii mysql-router-community 8.0.29-1debian11 amd64 MySQL Router
```

Especificarem la IP del server R/W en la configuració de bootstrap

Nota: Ens podriem connectar amb root però hauriem de tenir un ‘root’@’%’

Podem crear un usuari de nom **‘router’@’%’** amb pass **@MVM2016** als 3 servidors

```
CREATE USER 'router'@'%' Identified by '@MVM2016';
GRANT ALL PRIVILEGES ON . TO 'router'@'%' WITH GRANT OPTION;
Flush privileges;
```

Ara ens connectarem amb l’usuari router però forçant sockets.

```
mysqlrouter --bootstrap router@172.10.0.100:3306 --directory mysql-router --conf-use-sockets --force
```

Com podem veure, ens ha creat el directori mysql-router a la nostra home i amb el següent contingut:

```
ls -l mysql-router/
```

```
total 28  
drwx—— 2 usuari usuari 4096 may 28 12:30 data  
drwx—— 2 usuari usuari 4096 may 28 12:30 log  
-rw——- 1 usuari usuari 1993 may 28 12:30 mysqlrouter.conf  
-rw——- 1 usuari usuari 96 may 28 12:30 mysqlrouter.key  
drwx—— 2 usuari usuari 4096 may 28 12:30 run  
-rwx—— 1 usuari usuari 144 may 28 12:30 start.sh  
-rwx—— 1 usuari usuari 185 may 28 12:30 stop.sh  
```

Nota: Per inicialitzar mysqlrouter un cop comprovem que el servei està actiu o en cas de que no apareguin els sockets, haurem d’utilitzar l’script start.sh

- **start.sh** — Inicia i crea els sockets
- **stop.sh** — Atura i elimina els sockets

Ara crearem una base de dades de nom **testCluster** al server cluster00 i es propagarà als 3 servers

```
CREATE DATABASE testcluster;
use testcluster;
CREATE TABLE taula ( id INT auto_increment, data timestamp default now(), nomhost varchat(20), Primary Key (id));
```

Novament en router examinem si s’està executant el servei

```
sudo ps -fea | grep mysqlrouter
```

```
[sudo] password for usuari:  
mysqlro+ 1555 1 0 12:58 ? 00:00:00 /usr/bin/mysqlrouter -c /etc/mysqlrouter/mysqlrouter.conf 
usuari 1610 874 0 13:48 pts/1 00:00:00 grep mysqlrouter
```

Anem a canviar l’arxiu que apunta a la seva configuració amb un enllaç simbòlic i prèviament canviem permissos de 600 a 664

```
chmod 664 /home/usuari/mysql-router/mysqlrouter.conf
sudo ln -s /home/usuari/mysql-router/mysqlrouter.conf /etc/mysqlrouter/mysqlrouter.conf
```

Reiniciem el servei de mysqlrouter

```
sudo service mysqlrouter restart
```

I tornem a comprovar

```
sudo ps -fea | grep mysqlrouter
```

Ens connectarem amb el client mysql des de mysqlrouter i amb l’usuari router amb el socket de mysqlrouter en el port R/W 6446

```
mysql -u router -p -h localhost -P 6446 -S /home/usuari/mysql-router/mysql.sock -v
```

Si fem un show databases, podem comprovar que veiem la bbdd testcluster i per tant ens hem connectat al Group Replication Server del nostre Cluster ‘DBcluster’

Des dels tres servers, si consultem la taula de testcluster, veurem que està buida

```
Select * From testcluster.taula;
Empty set (0.00 sec)
```

A quin node m’he connectat?

```
Select @@hostname;
```
|@@hostname|
|--|
|cluster00|
1 row in set (0.00 sec)


Farem un INSERT des de router

```
INSERT INTO taula VALUES (NULL, DEFAULT, (Select @@hostname));
```

Ara anem a consultar els servers

```
Select * From testcluster.taula;
```
| id | data | nomhost |
|--|--|--|
| 1 | 2022-05-28 14:29:46 | cluster00|
1 row in set (0.00 sec)


Supossem que falla cluster00 que està ara com R/W – Anem ha aturar-lo per simular una caiguda

Ens connectem a cluster00 i l’aturem

```
ssh usuari@s0
```

```
usuari@cluster00:~$ sudo systemctl stop mysql.service
```

Si intentem fer des de router un select.. veurem que intentarà reconnectar

<code class="language-bash">
mysql> Select * From testcluster.taula;
.
ERROR 2013 (HY000): Lost connection to MySQL server during query  
No connection. Trying to reconnect…  
Connection id: 88  
Current database: testcluster
</code>

<code class="language-bash">
mysql> Select * From testcluster.taula;
</code>

| id | data | nomhost |
|--|--|--|
| 1 | 2022-05-28 14:29:46 | cluster00 |
1 row in set (0.00 sec)


Si consultem en quin server estem, veurem que ja **no és** el cluster00

<code class="language-bash">
Select @@hostname;
</code>

| @@hostname |
|--|
| cluster02 |
1 row in set (0.00 sec)


En aquest cas ha promogut a cluster02 com R/W

Si ens connectem directament al cluster02 com clustermanager podem comprovar l’estat del cluster

```
ssh usuari@172.10.0.102
```

```
mysqlsh clustermanager@cluster02:3306
var cluster = dba.getCluster()
cluster.status()
```

<details>
    <summary>Mostra el codi</summary>
     <pre><code class="language-bash">
{  
“clusterName”: “DBcluster”,  
“defaultReplicaSet”: {  
“name”: “default”,  
“primary”: “172.10.0.102:3306”,  
“ssl”: “REQUIRED”,  
“status”: “OK_NO_TOLERANCE”,  
“statusText”: “Cluster is NOT tolerant to any failures. 1 member is not active.”,  
“topology”: {  
“172.10.0.100:3306”: {  
“address”: “172.10.0.100:3306”,  
“memberRole”: “SECONDARY”,  
“mode”: “n/a”,  
“readReplicas”: {},  
“role”: “HA”,  
“shellConnectError”: “MySQL Error 2003: Could not open connection to ‘172.10.0.100:3306’: Can’t connect to MySQL server on ‘172.10.0.100:3306’ (111)”,  
“status”: “(MISSING)”  
},  
“172.10.0.101:3306”: {  
“address”: “172.10.0.101:3306”,  
“memberRole”: “SECONDARY”,  
“mode”: “R/O”,  
“readReplicas”: {},  
“replicationLag”: null,  
“role”: “HA”,  
“status”: “ONLINE”,  
“version”: “8.0.29”  
},  
“172.10.0.102:3306”: {  
“address”: “172.10.0.102:3306”,  
“memberRole”: “PRIMARY”,  
“mode”: “R/W”,  
“readReplicas”: {},  
“replicationLag”: null,  
“role”: “HA”,  
“status”: “ONLINE”,  
“version”: “8.0.29”  
}  
},  
“topologyMode”: “Single-Primary”  
},  
“groupInformationSourceMember”: “172.10.0.102:3306”  
}  
     </code></pre>
</details>

Podem veure com el cluster00 té un status “**n/a**” i a més mostra “(**MISSING**)”  
El cluster01 continua com R/O  
El clustere02 ara està com R/W

Si tornem a fer el mateix **insert** des de router i comprovem, veurem quin host l’envia

<code class="language-bash">
mysql> Select * From testcluster.taula;
</code>

| id | data | nomhost |
|--|--|--|
| 1 | 2022-05-28 14:29:46 | cluster00 |
| 2 | 2022-05-28 14:55:57 | cluster02 |
2 rows in set (0.01 sec)


Des de cluster00 tornem a aixecar el servidor

```
usuari@cluster00:~$ sudo systemctl start mysql.service
```

Per últim, des del cluster02 tornem a demanar l’estat del group replication

```
cluster.status()
```

<details>
    <summary>Mostra el codi</summary>
     <pre><code class="language-bash">
{  
“clusterName”: “DBcluster”,  
“defaultReplicaSet”: {  
“name”: “default”,  
“primary”: “172.10.0.102:3306”,  
“ssl”: “REQUIRED”,  
“status”: “OK”,  
“statusText”: “Cluster is ONLINE and can tolerate up to ONE failure.”,  
“topology”: {  
“172.10.0.100:3306”: {  
“address”: “172.10.0.100:3306”,  
“memberRole”: “SECONDARY”,  
“mode”: “R/O”,  
“readReplicas”: {},  
“replicationLag”: null,  
“role”: “HA”,  
“status”: “ONLINE”,  
“version”: “8.0.29”  
},  
“172.10.0.101:3306”: {  
“address”: “172.10.0.101:3306”,  
“memberRole”: “SECONDARY”,  
“mode”: “R/O”,  
“readReplicas”: {},  
“replicationLag”: null,  
“role”: “HA”,  
“status”: “ONLINE”,  
“version”: “8.0.29”  
},  
“172.10.0.102:3306”: {  
“address”: “172.10.0.102:3306”,  
“memberRole”: “PRIMARY”,  
“mode”: “R/W”,  
“readReplicas”: {},  
“replicationLag”: null,  
“role”: “HA”,  
“status”: “ONLINE”,  
“version”: “8.0.29”  
}  
},  
“topologyMode”: “Single-Primary”  
},  
“groupInformationSourceMember”: “172.10.0.102:3306”  
}  
     </code></pre>
</details>

Podem veure com cluster00 torna a estar “ONLINE” però ha passat a mode R/O

Si el volem promoure novament, només necessitem la comanda:

```
cluster.setPrimaryInstance('cluster00:3306')
```

### Màquines virtuals

Pots descarregar les següents màquines virtuals (arxius .ova) preparades per configurar group replication.  

Estan preinstal·lades amb Debian 11 i necessiten només 1GB de memòria per afegir al cluster sense problemes.

<!--
<a href="https://bit.ly/3ar8Oga" title="ova – MySQL Cluster (986.707.456 bytes)">
  <img src="https://res.cloudinary.com/dbuv9r3p5/image/upload/VirtualBox-icon-150x150_wuuvvl.png" alt="MysqlCluster.ova">
</a>
-->

---

[![MysqlCluster.ova ](https://res.cloudinary.com/dbuv9r3p5/image/upload/w_90/VirtualBox-icon-150x150_wuuvvl.png)
**MySqlCluster.ova**
ova – MySQL Cluster ( 986.707.456 bytes )](https://bit.ly/3ar8Oga)

[![MysqlRouter.ova](https://res.cloudinary.com/dbuv9r3p5/image/upload/router-database-icon_yszi4m.png)  
**MysqlRouter.ova**  
ova – MySQL Router (2.211.416.576 bytes)](https://bit.ly/3NiZlWT)

---


### Bibliografia

[Alta disponibilitat](https://ioc.xtec.cat/materials/FP/Recursos/fp_asix_m11_/web/fp_asix_m11_htmlindex/WebContent/u4/a1/continguts.html)

<https://dev.mysql.com/doc/refman/8.0/en/group-replication.html>

<https://dev.mysql.com/doc/refman/8.0/en/mysql-innodb-cluster-introduction.html>

<https://dev.mysql.com/doc/mysql-shell/8.0/en/mysql-innodb-cluster.html>

**Configuració d’InnoDB cluster**: 
[Configuring Innodb Cluster](https://dev.mysql.com/doc/mysql-shell/8.0/en/configuring-innodb-cluster.html)

**Monitorització d’InnoDB cluster**: 
[Monitoring Innodb Cluster](https://dev.mysql.com/doc/mysql-shell/8.0/en/monitoring-innodb-cluster.html)

Eina gràfica de monitorització Percona: 
<https://www.percona.com/software/pmm/quickstart>

<https://dev.mysql.com/doc/mysql-router/8.0/en/mysql-router-innodb-cluster.html>

<https://kruschecompany.com/mysql-innodb-cluster-installation/>

<https://www.datalbi.com/article.php?id=62>

<https://lefred.be/content/mysql-innodb-cluster-howto-install-it-from-scratch/>

[Severalnines 1](https://severalnines.com/blog/mysql-innodb-cluster-80-complete-deployment-walk-through-part-one/)
[Severalnines 2](https://severalnines.com/database-blog/mysql-innodb-cluster-80-complete-operation-walk-through-part-two)

Vídeos

<https://www.youtube.com/user/mysqlespanol>

<https://dev.mysql.com/blog-archive/mysql-innodb-cluster-tutorial-videos/>