# HA Trino Clusters 

This project aims to bring up a working model of highly available trino clusters via docker with the help of envoy.

# How to

1. Start docker daemon within your host
2. Change directory to project root and run `docker-compose up -d`
    
    This should bring up the following services:
    
    - envoy
    - controlplane
    - postgres
    - clusterA(coord+workers)
    - clusterB(coord+workers)

3. Perform `docker ps` to make sure all services are up
4. Setup trino CLI using trino doc given [here](https://trino.io/docs/current/client/cli.html#installation)
5. Connect to trino clusters and fire queries as shown below: 

        $./trino http://localhost:8090

        trino> select * from system.runtime.nodes;

6. Subsequent queries will round robin between clusters clusterA and clusterB 
7. Bring down cluster `clusterA` using `docker-compose stop clusterA-coord`
8. Queries will continue to get served from `clusterB` showcasing high availability of clusters as expected
9. Bring back `clusterA` with `docker-compose up -d`
10. The cluster will automatically get registered on to envoy and start serving queries along with `clusterB` 
