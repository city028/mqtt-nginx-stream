
# Nginx docker image for MQTT Reverse proxy

This is a small change to the Dockerfile Tekn0ir created here:

https://github.com/tekn0ir/nginx-stream

The only thing changed realy is the removal of the "EXPOSE 80 443" command as this allowed me to run multiple Docker instances in parallel without these ports(80 and 433) being exposed by multiple containers and just the ones I have specified during docker run. Below an example how I use this to reverse proxy an MQTT stream to a backend MQTT server.

### MQTT Stream example

```bash
docker run -dit -p 18832:1883 --name mqttnginx --restart unless-stopped -v /root/docker-nginx/conf/conf.d:/opt/nginx/stream.conf.d:ro -d mqtt-nginx-stream
```
In the above example I am exposing port 18832 to the outside world (my other container is exposed to 18831).

The below option in the command alows me to add my own config file:
```bash
-v <path to my local config file>:/opt/nginx/stream.conf.d:ro
 ```   
Config file (default.conf):

	upstream mqtt_server {
		server 192.168.0.3:1883;
	}

	server {
		listen 1883;
		proxy_pass mqtt_server;
	}
    

As you can see, my backend server (192.168.0.3) listenes on port 1883 and the server also listens on port 1883.

Please check out Tekn0ir's page for more information

https://github.com/tekn0ir/nginx-stream
