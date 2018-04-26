Copy storage folder on your host
copy all ring files from prxoy node to storage files folder

config rsyncd.conf, account-server.conf, object-server.conf and container-server.conf and ntp.conf with reference of
https://docs.openstack.org/swift/queens/install/storage-install-rdo.html

install xfsprogs in host if centos or respective package with respective OS. swift support xfs
make xfs formatted partition and mount in host and gives in arg of docker -v 

EXAMPLE: /dev/sdb1 /srv/node/device0
/etc/fstab entry can also done with reference of given below
---------for reference only--- 
/dev/sdb /srv/node/sdb xfs noatime,nodiratime,nobarrier,logbufs=8 0 2
/dev/sdc /srv/node/sdc xfs noatime,nodiratime,nobarrier,logbufs=8 0 2
-----------
/srv/node is embedded in Dockerfile, if you want to change you can

this docker file contain build arg device (same as mount point)

* docker build -t --build-arg device=device_name  image:version .
make image from above docker command and then make instance as given below 

* docker run -d --net=host -v /srv/node/device0:/srv/node/device0:rw image_id

open port in host because you are using host networking in docker 873,6200,6201,6202
