FROM ubuntu:18.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update -qq && apt-get install -y nfs-kernel-server runit inotify-tools -qq
RUN mkdir -p /shared
RUN chmod -R 777 /shared

COPY ./exports/exports /etc/exports

RUN mkdir -p /etc/sv/nfs

COPY ./servicenfs/nfs.init /etc/sv/nfs/run
COPY ./servicenfs/nfs.stop /etc/sv/nfs/finish
COPY ./run/run.sh /usr/local/bin/nfs_setup

EXPOSE 111/udp 2049/udp 111/tcp 2049/tcp

ENTRYPOINT ["/usr/local/bin/nfs_setup"]
