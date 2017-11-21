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


In a web browser you should beable to access the following services at:

prometheus:
--------------
Adding data analysis steps to this service will have to be a future set of tasks.

	http://localhost:9090

consul_exporter:
--------------

	http://consul_metrics:9107

grafana:
--------------

	http://localhost:3000

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
