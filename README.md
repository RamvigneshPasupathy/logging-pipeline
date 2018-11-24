
**My Logging Pipeline**
===================


**ELK Server**
-------------
###  **Server Configuration**
Virtual Box Settings     | Value | Remarks 
------------------------ | ----- | -------
OS | Ubuntu 14.04 - 64 bit  
Network (Adapter 1) | Attached to Internal Network (intnet) | For networking between VMs inside Virtual Box 
Network (Adapter 2) | Attached to NAT | For internet access within the VM
Disk     | 25 GB VDI
Memory    | 4GB
> **Note:** Use the following command in the host machine to enable intnet (Default DHCP of Virtual Box). [Reference!](https://superuser.com/a/339567/328460)
>
>     VBoxManage dhcpserver add --netname intnet --ip 10.10.10.1 --netmask 255.255.255.0 --lowerip 10.10.10.2 --upperip 10.10.10.12 --enable
<hr>

### **Software Installations**
**Oracle Java 8**
> ***installation..***
>
>	  sudo add-apt-repository -y ppa:webupd8team/java
>	  sudo apt-get update
>	  sudo apt-get -y install oracle-java8-installer
> ***env variable will be automatically set..***
>
>     echo $JAVA_HOME
> ***if not set, try this..*** [*Reference!*](http://tipsonubuntu.com/2016/07/31/install-oracle-java-8-9-ubuntu-16-04-linux-mint-18/)
>
> 	  sudo apt install oracle-java8-set-default

**Elasticsearch**
> ***installation..***
>
> 	  wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
>	  echo "deb http://packages.elastic.co/elasticsearch/2.x/debian stable main" | sudo tee -a /etc/apt/sources.list.d/elasticsearch-2.x.list
>	  sudo apt-get update
>	  sudo apt-get -y install elasticsearch
> ***basic configurations..***
>
> 	  # open config file
>     sudo vi /etc/elasticsearch/elasticsearch.yml
>
>	  # uncomment and modify the values for network.host, host.port
>	  # write out and exit
> 	  network.host: localhost
>	  host.port: 9200
>
>	  # `localhost` can be modified with the ip address of your elk server
> 	  # modify other fields like `cluster.name` as you prefer
>
> ***open port 9200 to let other servers to access the elasticsearch api..***
>
>     ufw enable
>	  ufw allow 9200
> ***start elasticsearch as a service..***
>
>	  sudo service elasticsearch start
> ***check if elasticsearch is working properly..***
>
>	- open `localhost:9200` in any browser
>	- and the result should be something like this
>				
>			{
>				"cluster_name" : "my-elastic-cluster",
>				"cluster_uuid" : "qvYrCZtzRpyrffNWTGl00Q",
>				"version" : {
> 					"number" : "2.4.6",
>  					"build_hash" : "5376dca9f70f3abef96a77f4bb22720ace8240fd",
> 					"build_timestamp" : "2017-07-18T12:17:44Z",
>					"build_snapshot" : false,
>					"lucene_version" : "5.5.4"
>				},
>				"tagline" : "You Know, for Search"
>			}
>
>***to start elasticsearch service on boot (optional)..***
>
>     # will work for ubuntu 14.04
> 	  sudo update-rc.d elasticsearch defaults 95 10
**Logstash**
> ***installation..***
>
>     echo 'deb http://packages.elastic.co/logstash/2.2/debian stable main' | sudo tee /etc/apt/sources.list.d/logstash-2.2.x.list
>     sudo apt-get update
>     sudo apt-get install logstash
**Kibana**
> ***installation..***
>
>     echo "deb http://packages.elastic.co/kibana/4.5/debian stable main" | sudo tee -a /etc/apt/sources.list.d/kibana-4.5.x.list
>     sudo apt-get update
>     sudo apt-get -y install kibana
> ***basic configurations..***
>
> 	  # open config file
>     sudo vi /opt/kibana/config/kibana.yml
>
>	  # uncomment and modify the values for server.host
>	  # write out and exit
> 	  server.host: "localhost"
>
>	  # `localhost` can be modified with the ip address of your elk server
>
> ***start elasticsearch as a service..***
>
>     sudo service kibana start
>***to start kibana service on boot (optional)..***
>
>     # will work for ubuntu 14.04
> 	  sudo update-rc.d kibana defaults 96 9
