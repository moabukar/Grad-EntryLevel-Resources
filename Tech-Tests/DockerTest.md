# Docker Test

## docker-compose

create a simple local development environment using the `docker-compose` tool.  

The environment should have two development services supporting `centos` OS in version `8.4.2105`:  
- a `backend` service running a `ruby` image in version `3.0` with local directory `backend` bound into `/project/backend`  
- a `frontend` service running a `aspnet` image in version `4.8` with local directory `frontend` bound into `project/frontend`  

and one database service:  
- `db` running a `mysql` image in version `5.7.37`  

Additionally, the `backend` service should expose its port `6000` from container to a publicly exposed port `6000` via the UDP protocol for local DHCP services.  
The `frontend` service should expose its port `8080` from the contier to a publicly exposed port `80` via the TCP protocol.  
Make sure to use *long* syntax for port definitions and *short* syntax for volume defintions.  

The `frontend` service should be exposed via an external network called `fend-ingress`, while the `backend` and `db` services should use a local network called `database`.  

The services should start in the follwoing order: `db, backend, frontend`.

### HINT: you will be writing a YAML file

<details>
<summary> Solution (Check once you finish the task): </summary>

```
version: "centos8.4.2105"
networks:
     network1:
       external: true
       name: frontend-ingress
     network2:
       external: false
       name: database
services:
     backend:
       depends_on: databse
       image: ruby:3.0
       volumes:
         - backend:/project/backend
     ports:
       - target: 6000
         published: 6000
         protocol: udp
         mode: host
     networks:
       - database

     frontend:
       depends_on: backend
       image: aspnet:4.8
       volumes:
       - frontend:/project/frontend
       ports:
       - target: 8080
         published: 80
         protocol: tcp
         mode: ingress
       networks:
        -fend-ingress

     db:
       image: mysql:5.7.37
       networks:
       - database
```
</details>
