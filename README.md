# HA Trino Clusters 

This project aims to bring up a working model of highly available trino clusters via docker with the help of envoy.

# How to

1. Start docker daemon within your host
2. Change directory to project root and run `docker-compose up -d`
    
    This should bring up the following services:
    
    - envoy
    - controlplane
    - postgres
    - trino_1
    - trino_2

3. Perform `docker ps` to make sure all services are up
4. Setup trino CLI using trino doc given [here](https://trino.io/docs/current/client/cli.html#installation)
5. Connect to trino clusters and fire queries as shown below: 

        $./trino http://localhost:8000

        trino> select * from system.runtime.nodes;

6. Subsequent queries will round robin between clusters trino_1 and trino_2. 
7. Bring down cluster `trino_1` using `docker-compose stop trino_1`
8. Queries will continue to get served from `trino_2` showcasing high availability of clusters as expected
9. Bring back `trino_1` with `docker-compose up -d`
10. The cluster will automatically get registered on to envoy and start serving queries along with `trino_2` 