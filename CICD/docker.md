`docker run -d --name influxdb --network grafana -p 8086:8086 -v ${pwd}:/var/lib/influxdb/ influxdb`