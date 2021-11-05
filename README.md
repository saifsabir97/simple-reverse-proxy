# simple-reverse-proxy
Simple demonstration of using Nginx to act as a reverse proxy for subdomains along with port forwarding.

**Example usecase:**

Host multiple apps on a single server at different ports. 

When a user wants to access a specific app, they can specify the corresponding sub-domain in the URL so that Nginx redirects the request to appropriate internal port. 

In this way, your clients don't need to worry about specifying different ports every time they want to access a different app made by you.

**Steps to set up this project:**

1. Navigate to `nginx`, run the command `docker-compose up`. This will start the reverse proxy.

2. Navigate to `web-server-a`, run the command `docker-compose up`. This will start the web server `a`.


2. Navigate to `web-server-b`, run the command `docker-compose up`. This will start the web server `b`.

**Steps to test the project:**

1. When a user enters `http://subdomain-a.localhost:8000`, they see the response of `web-server-a`.

2. When a user enters `http://subdomain-b.localhost:8000`, they see the response of `web-server-b`.

This is because in the `nginx.conf` file, we have mentioned the rules accordingly. For `web-server-a`, we have the config as
```
    server {
         listen 8000;
         server_name subdomain-a.localhost;
         location / {
              proxy_pass http://host.docker.internal:8080/;
         }
    }
```
This means that when a request comes for `subdomain-a.localhost:8000`, it is redirected by Nginx to `http://host.docker.internal:8080/`, which is where the web server `a` runs.
