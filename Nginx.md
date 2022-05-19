## Nginx

By Jack Szwergold

Get the version of Nginx from the command line:

    nginx -v

If installed in a non-standard location—like `/opt`—run it like this:

    /opt/nginx/sbin/nginx -v

The output should be something like this:

    nginx version: nginx/1.4.1

#### Install Nginx on Ubuntu 14.04:

	sudo aptitude install nginx-full

Sundry service control commands:

	sudo service nginx restart
	sudo service nginx start
	sudo service nginx stop
	sudo service nginx status

#### Install `ngxtop`.

Install `ngxtop` to monitor realtime stats on the server like this. First install PIP like this:

	sudo aptitude install python-pip

Then install `ngxtop` like this:

	sudo pip install ngxtop

And then monitor the Nginx service like this:

	sudo ngxtop

An example of showing 200 top items and filtering out items that don’t begin with `/sockjs`:

	sudo ngxtop -n 200 --filter 'not request_path.startswith("/sockjs")'

#### Disable Nginx version number from header calls.

Open up the config file here:

	sudo nano /etc/nginx/nginx.conf

Look for this line:

	# server_tokens off;

And then uncomment it like this:

	server_tokens off;

Finally, restart Nginx like this:

	sudo service nginx restart

#### Sundry stuff.

Default Nginx page located here:

	/usr/share/nginx/html/index.html

### Nginx config examples.

A basic reverse proxy config for Nginx:

	map $http_upgrade $connection_upgrade {
	  default upgrade;
	  '' close;
	}
	
	server {
	
	  listen 80 default_server;
	  listen [::]:80 default_server ipv6only=on;
	
	  root /usr/share/nginx/html;
	  index index.html index.htm;
	
	  server_name vagrant.local;
	
	  # location / {
	  #   rewrite ^ https://$server_name$request_uri? permanent;
	  # }
	
	  location / {
	
	    proxy_pass http://127.0.0.1:8080;
	    proxy_http_version 1.1;
	    proxy_set_header Upgrade $http_upgrade;
	    proxy_set_header Connection $connection_upgrade;
	    proxy_set_header X-Forwarded-For $remote_addr;
	
	    # if ($uri != '/') {
	    #   expires 30d;
	    # }
	
	  }
	
	}

***

A more advanced Nginx reverse proxy config. This case Nginx is acting as a reverse proxy for a Gunicorn Python application server.

While in the other, simpler config above the `proxy_pass` setting simply goes directly to the web server at `http://127.0.0.1:8080`, this config leverages an `upstream` config namded `app_server` that is then called by `proxy_pass` as `proxy_pass http://app_server;`.

The core benefit of this setup is it is easier to add other members to an application pool but, in the case of a single member “pool”, the `fail_timeout=0` option in the `upstream` config tells Nginx, “Try to connect to the upstream server indicated until you get a response and don’t timeout any earlier than the `proxy_connect_timeout`, `proxy_read_timeout` and `proxy_send_timeout`.”

This helps in cases where, in this case, the Python application somehow fails, the failure doesn’t result in something like a “502 Bad Gateway” error. Setting `fail_timeout=0` means the Nginx server will just keep on trying to connect to the `upstream app_server` server until it gets a response. And if it can’t do that in 60 seconds in this case, _then_ die and toss a 502 error.

	# For more information on configuration, see:
	#   * Official English Documentation: http://nginx.org/en/docs/
	#   * Official Russian Documentation: http://nginx.org/ru/docs/
	
	user nginx;
	worker_processes auto;
	error_log /var/log/nginx/error.log;
	worker_rlimit_nofile 1024;
	
	# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
	include /usr/share/nginx/modules/*.conf;
	
	events {
	    worker_connections 512;
	}
	
	http {
	  log_format main '$remote_addr - $remote_user [$time_local] "$request" '
	                  '$status $body_bytes_sent "$http_referer" '
	                  '"$http_user_agent" "$http_x_forwarded_for"';
	
	  default_type application/octet-stream;
	  access_log /var/log/nginx/access.log  main;
	
	  # This hides the server version from the response header.
	  server_tokens off;
	
	  ignore_invalid_headers on;
	
	  upstream app_server {
	    keepalive 32;
	    server unix:/run/gunicorn.sock fail_timeout=0;
	  }
	
	  server {
	    listen 80 default_server;
	    listen [::]:80 default_server;
	    server_name _;
	    return 301 https://$host$request_uri;
	  }
	
	  server {
	    listen 443 ssl default_server;
	    listen [::]:443 ssl default_server;
	    ssl on;
	    ssl_certificate "/etc/nginx/ssl/beep2020_SANs.crt";
	    ssl_certificate_key "/etc/nginx/ssl/private/beep2020_SANs.key";
	
	    root /usr/share/nginx/public/beep;
	
	    # Get request time in milliseconds starting when the first byte
	    # is read by the client.
	    add_header X-Time $request_time;
	
	    # https://docs.gunicorn.org/en/latest/deploy.html
	    location / {
	      try_files $uri @gunicorn_proxy;
	    }
	
	    location @gunicorn_proxy {
	      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	      proxy_set_header X-Forwarded-Proto $scheme;
	      proxy_set_header Host $http_host;
	      proxy_http_version 1.1;
	      proxy_set_header Connection "";
	      keepalive_timeout 0;
	      # we don't want nginx trying to do something clever with
	      # redirects, we set the Host: header above already.
	      proxy_redirect off;
	      proxy_connect_timeout 60s;
	      proxy_read_timeout 60s;
	      proxy_send_timeout 60s;
	      # proxy_pass http://unix:/run/gunicorn.sock;
	      proxy_pass http://app_server;
	    }
	  }
	}

***

*Nginx (c) by Jack Szwergold; written on September 27, 2015. This work is licensed under a Creative Commons Attribution-NonCommercial 4.0 International License (CC-BY-NC-4.0).*
