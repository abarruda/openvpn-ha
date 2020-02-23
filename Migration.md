Open VPN data volume migration script


sudo docker run --rm --volumes-from openvpn -v $(pwd):/backup busybox tar cvf /backup/openvpn-data.tar /etc/openvpn

# --rm: remove the container when it exits
# --volumes-from DATA: attach to the volumes shared by the DATA container
# -v $(pwd):/backup: bind mount the current directory into the container; to write the tar file to
# busybox: a small simpler image - good for quick maintenance
# tar cvf /backup/backup.tar /data: creates an uncompressed tar file of all the files in the /data directory

# create a new data container
docker create -v ovpn-data:/etc/openvpn --name openvpn giggio/openvpn-arm true

# untar the backup files into the new container᾿s data volume
docker run --rm --volumes-from openvpn -v $(pwd):/backup busybox tar xvf /backup/openvpn-data.tar /etc/openvpn


# compare to the original container
$ sudo docker run --rm --volumes-from DATA -v `pwd`:/backup busybox ls /data
sven.txt


# When migrating to Kubernetes, simply scp all of /etc/openvpn to the volume where the data lives.  In this case, the data was scp'd from the container to a network share which is attached as the open vpn data volume.
```
apk update
apk upgrade
apk add openssh-client
```




docker run -v ovpn-data:/etc/openvpn --rm busybox tar -cvf - -C /etc openvpn | xz > openvpn-backup.tar.xz


docker volume create --name ovpn-data
xzcat openvpn-backup.tar.xz | docker run -v ovpn-data:/etc/openvpn -i giggio/openvpn-arm tar -xvf - -C /etc
docker run -v ovpn-data:/etc/openvpn -d --name openvpn -p 1194:1194/udp --cap-add=NET_ADMIN --restart=unless-stopped  giggio/openvpn-arm
