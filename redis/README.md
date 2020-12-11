Redis Cheatsheet
====


Start / Stop local cluster via Docker
----
```
ip=$(ipconfig getifaddr en0) docker-compose -f docker-compose.yml up -d --build
docker-compose down
```
```
```

Docker Images
----
```
docker images
```
```
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
```


Docker Cleanup
----
```
docker rmi $(docker images -a -q)
```
```
```

Docker bash
----
```
docker run -it --net=redis_cluster-lan --rm redis:latest bash
```
```
[root@b1f27481ae89 ~]#
```


Check Version
----
```
docker run --net=redis_cluster-lan --rm redis:latest redis-cli --version
```
```
redis-cli 6.0.9
```

