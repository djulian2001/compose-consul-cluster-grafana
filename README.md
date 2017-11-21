A docker-compose of consul -> consul_exporter -> prometheus -> grafana 
==========
Using docker for mac, built a docker-compose cluster with 5 consul server agents,
a consul_exporter, a prometheus service, and grafana for the pretty stuff.

How to get this cluster up:
--------------
Clone the repo into a clean and fresh directory

Make sure the docker.app is running, build with the following version:
(Version 17.09.0-ce-mac35 (19611))

Run the following commands to build and bring up the cluster:

    docker-compose build

    docker-compose up

A few things to note about the consul cluster networking.

	# The consul cluster uses a private network, 192.168.99.0/24.
	# If you have used this network recently then this cluster will most likely fail.

	# The first consul_1 server agent has a bind ip address of 192.168.99.2 (not my favorite approach)
	# the rest of the consul servers, and other services, are dependent on this ip address.
	# To solve this issue you can do the following: (if you are ok with removing your unused networks)
	
	docker network prune

	# or ... run the cluster and get the ip address that the xxx_consul_1_1 is assigned and replace all
	#	192.168.99.2 with the new ip address, re-build the cluster and run, it should work.


When the consul agents start gossiping, with a web browser you should beable to access the following services

prometheus:
--------------
Adding data analysis steps to this service will have to be a future set of tasks.

	http://localhost:9090
![screen shot 2017-11-21 at 9 31 01 am](https://user-images.githubusercontent.com/27034956/33085873-4149ae98-cea3-11e7-8897-9210c55bdadf.png)


consul_exporter:
--------------

	http://consul_metrics:9107

grafana:
--------------

	http://localhost:3000

<img width="956" alt="screen shot 2017-11-21 at 9 52 54 am" src="https://user-images.githubusercontent.com/27034956/33085948-6d0afde8-cea3-11e7-9bbc-dbcf1f9da440.png">



Grafana Setup:
--------------
The grafana service requires some minimal setup to start pulling the data in from prometheus.

First login with admin:admin.

Then in the main menu (upper right corner grafana logo), select "Data Source"

Click "+ Add data source" input the following:

	Name: 		consul_metrics
	Type: 		<select> "prometheus"
	URL: 		http://localhost:9090
	Access: 	Direct
	Skip TLS:	<check box>

Click the "Add" button.

That should be it.  Oh wait... clean up

Clean up:
--------------
As this is just a learning development enviornment, I use the following commands to clean up:
	
	# within the directory with the docker-compose.yml file
	docker-compose stop
	docker-compose rm -v --force
	

Usage and learning:
--------------
Please feel free to share your thoughts, recommendations for improvements, and ideas
for adding analysis within prometheus and grafana.
