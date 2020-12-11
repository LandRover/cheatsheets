FROM redis:6.0.9

MAINTAINER Oleg Glozman <git@landroverz.com>

COPY rediscluster.conf /etc/redis/rediscluster.conf

ENTRYPOINT redis-server /etc/redis/rediscluster.conf