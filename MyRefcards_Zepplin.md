# Apache Zeplin on Hortonworks Ambari

### Purpose

Zeplin is open web based notebook that enables interactive data analysis

[github-quick-start](https://github.com/hortonworks-gallery/ambari-zeppelin-service#setup-the-ambari-service-quick-start)

[Quick start](https://zeppelin.apache.org/docs/0.6.2/quickstart/tutorial.html)

[Hortonworks zeppelin](https://hortonworks.com/apache/zeppelin/)


## Downlaod



## Install

[Installing Zeppelin on an Ambari-Managed Cluster](http://hortonworks.com/hadoop-tutorial/apache-zeppelin-hdp-2-4/)

'''
To obtain the HDP version for your HDP cluster, run the following command:

# hdp-select status hadoop-client | sed 's/hadoop-client - (.*)/1/'

# VERSION=`hdp-select status hadoop-client | sed 's/hadoop-client - ([0-9].[0-9]).*/1/'`

VERSION=`hdp-select status hadoop-client | sed 's/hadoop-client - \([0-9]\.[0-9]\).*/\1/'`

sudo git clone https://github.com/hortonworks-gallery/ambari-zeppelin-service.git /var/lib/ambari-server/resources/stacks/HDP/$VERSION/services/ZEPPELIN

sudo service ambari-server restart

Ambari dashboard, choose Actions -> Add Service:

'''


## Config

## Usage 

### Start

### Stop

## Projects

## Tips & Notes

### Help


### Version


### Commands 

    
### Uninstall Zeppelin

1. Note all the host components associated with the service

running on 
http://192.168.199.2:8080/api/v1/clusters/abydev/services/ZEPPELIN

'''
curl -u admin:admin -H "X-Requested-By: ambari" -X GET  http://192.168.199.2:8080/api/v1/clusters/abydev/services/ZEPPELIN
'''

2. Ensure the service is stopped (you can use the Ambari Web-UI to stop the service as well)

curl -u admin:admin -H "X-Requested-By: ambari" -X PUT -d '{"RequestInfo":{"context":"Stop Service"},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' http://192.168.199.2:8080/api/v1/clusters/abydev/services/ZEPPELIN

Stop individual components 

curl -u admin:admin -H "X-Requested-By: ambari" -X PUT -d '{"RequestInfo":{"context":"Stop Component"},"Body":{"HostRoles":{"state":"INSTALLED"}}}' http://192.168.199.2:8080/api/v1/clusters/abydev/hosts/node1.example.com/host_components/ZEPPELIN
curl -u admin:admin -H "X-Requested-By: ambari" -X PUT -d '{"RequestInfo":{"context":"Stop Component"},"Body":{"HostRoles":{"state":"INSTALLED"}}}' http://192.168.199.2:8080/api/v1/clusters/abydev/hosts/node2.example.com/host_components/ZEPPELIN
curl -u admin:admin -H "X-Requested-By: ambari" -X PUT -d '{"RequestInfo":{"context":"Stop Component"},"Body":{"HostRoles":{"state":"INSTALLED"}}}' http://192.168.199.2:8080/api/v1/clusters/abydev/hosts/node3.example.com/host_components/ZEPPELIN

Stop all component instances

curl -u admin:admin -H "X-Requested-By: ambari" -X PUT -d '{"RequestInfo":{"context":"Stop All Components"},"Body":{"ServiceComponentInfo":{"state":"INSTALLED"}}}' http://192.168.199.2:8080/api/v1/clusters/abydev/services/ZEPPELIN/components/ZEPPELIN_MASTER

3. Delete the whole SERVICE

curl -u admin:admin -H "X-Requested-By: ambari" -X DELETE  http://192.168.199.2:8080/api/v1/clusters/abydev/services/ZEPPELIN



#Error :
	Error downloading packages:
	  epel-release-7-8.noarch: [Errno 256] No more mirrors to try.
	https://community.hortonworks.com/questions/45084/zeppelin-installation-via-ambari-is-failing-on-hdp-1.html


# Upgrade Ambari Server 

http://docs.hortonworks.com/HDPDocuments/Ambari-2.2.2.0/bk_upgrading_Ambari/content/_upgrade_ambari.html

1. Stop the Ambari Server. On the host running Ambari Server:

	ambari-server stop

2. Stop all Ambari Agents. On each host in your cluster running an Ambari Agent:

	ambari-agent stop


3. Fetch the new Ambari repo and replace the old repository file with the new repository file on all hosts in your cluster.

For RHEL/CentOS/Oracle Linux 7:

	wget -nv http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.4.2.0/ambari.repo -O /etc/yum.repos.d/ambari.repo

4. Upgrade Ambari Server. On the host running Ambari Server:
	yum clean all
	yum info ambari-server

In the info output, visually validate that there is an available version containing "2.4.2"

	yum upgrade ambari-server

5. Upgrade all Ambari Agents. On each host in your cluster running an Ambari Agent:

For RHEL/CentOS/Oracle Linux:

	yum upgrade ambari-agent
	
6. After the upgrade process completes, check each host to make sure the new files have been installed:

	rpm -qa | grep ambari-agent

7. Upgrade Ambari Server database schema. On the host running Ambari Server:

	ambari-server upgrade

8. Start the Ambari Server. On the host running Ambari Server:

	ambari-server start

9. Start all Ambari Agents. On each host in your cluster running an Ambari Agent:

	ambari-agent start


10. Open Ambari Web.

Point your browser to http://<your.ambari.server>:8080
Log in, You will see a Restart indicator next to each service after upgrading.



