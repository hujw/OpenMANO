# OpenMANO
Refer to https://github.com/nfvlabs/openmano/wiki/Getting-starte
## OpenMANO Installation
* Download the installation script
```
$ wget https://github.com/nfvlabs/openmano/raw/master/scripts/install-openmano.sh
$ chmod +x install-openmano.sh 
$ sudo ./install-openmano.sh root nchc*2
```
* Use apt-get to install the related packages
```
$ sudo apt-get install git screen wget python-yaml python-libvirt python-bottle \
  python-mysqldb python-jsonschema python-paramiko python-argcomplete python-requests
$ sudo apt-get install python-novaclient python-keystoneclient python-glanceclient python-neutronclient
```
* Clone the git repository
```
$ git clone https://github.com/nfvlabs/openmano.git openmano
```
* Install the mysql database
```
$ sudo apt-get install mysql-server
$ mysql_secure_installation 
```
Note that:
1. 'mysql_secure_installation' is used to config the root's password and remove remote access to the root user.
2. The length of user password must be more than 8.
* Start mysql service and login 
```
$ sudo systemctl start mysql
$ sudo /usr/bin/mysql -u root -p
```

* Create the two database used by MANO
```
$ sudo mysqladmin -u root -p create vim_db
$ sudo mysqladmin -u root -p create mano_db

mysql> CREATE USER 'vim'@'localhost' identified by 'vimpasswd';
mysql> GRANT ALL PRIVILEGES ON vim_db.* TO 'vim'@'localhost';
mysql> CREATE USER 'mano'@'localhost' identified by 'manopasswd';
mysql> GRANT ALL PRIVILEGES ON mano_db.* TO 'mano'@'localhost';
```
* Initialize both databases
```
$ openvim/database_utils/init_vim_db.sh   -uvim -pvimpasswd
$ openmano/database_utils/init_mano_db.sh -umano -pmanopasswd
```

## Floodlight (version 0.90) Installation
```
$ sudo apt-get install build-essential default-jdk ant python-dev
$ cd ..
$ wget https://github.com/floodlight/floodlight/archive/v0.90.tar.gz
$ tar xvzf v0.90.tar.gz
$ cd floodlight-0.90
$ export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre  <-- confirm the path of JAVA_HOME
$ ant
```
* Edit the configuration file before starting Floodlight
```
$ vi openmano/scripts/flow.properties
$ vi openmano/scripts/flow-logback.xml
```
* Run it (Two ways)
```
using the script
$ openmano/scripts/service-openmano.sh openflow start 

execute manually
$ java  -Dlogback.configurationFile=openmano/scripts/flow-logback.xml \
  -jar floodlight-0.90/target/floodlight.jar -cf openmano/scripts/flow.properties
```

## OpenMANO GUI Setup
Check whether apache server is installed or not, if not, please install it.
```
$ sudo service apache2 status
$ sudo apt-get install apache2
```
Change the user owner of the directory 'openmano-gui'
```
sudo chown -R www-data ./openmano-gui 
```
Create a link to apache web directory
```
sudo ln -s /<full path>/openmano-gui /var/www/html/openmano
```
