# Docker Test

## docker-compose

create a simple local development environment using the `docker-compose` tool.  

The environment should have two development services supporting `alpine` OS in version `3.9`:  
- a `backend` service running a `python` image in version `3.7` with local directory `backend` bound into `/app/backend`  
- a `frontend` service running a `node` imge in version `10.19.0` with local directory `frontend` bound into `app/frontend`  

and one database service:  
- `database` running a `mysql` image in version `5.6.47`  

Additionally, the `backend` service should expose its port `6000` from container to a publicly exposed port `6000` via the UDP protocol for local DHCP services.  
The `frontend` service should expose its port `8080` from the contier to a publicly exposed port `80` via the TCP protocol.  
Make sure to use *long* syntax for port definitions and *short* syntax for volume defintions.  

The `frontend` service should be exposed via an external network called `frontend-ingress`, while the `backend` and `database` services should use a local network called `database`.  

The services should start in the follwoing order: `database, backend, frontend`.

### HINT: you will be writing a YAML file

<details>
<summary> Solution (Check once you finish the task): </summary>

```
version: "3.9"
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
       image: python:3.7
       volumes:
         - backend:/app/backend
     ports:
       - target: 6000
         published: 6000
         protocol: udp
         mode: host
     networks:
       - database

     frontend:
       depends_on: backend
       image: node:10.19.0
       volumes:
       - frontend:/app/frontend
       ports:
       - target: 8080
         published: 80
         protocol: tcp
         mode: ingress
       networks:
        -frontend-ingress

     database:
       image: mysql:5.6.47
       networks:
       - database
```
</details>
