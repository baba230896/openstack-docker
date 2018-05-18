#issue : docker -v use host ownership. 
Solution: provide the 777 permission to host folder which is used by the docker for storage
# For building docker image

* Copy storage folder on your host
* copy all ring files from prxoy node to storage files folder
* config 
> rsyncd.conf
> account-server.conf
> object-server.conf
> container-server.conf
> ntp.conf 
reference of https://docs.openstack.org/swift/queens/install/storage-install-rdo.html

# on docker host 
install xfsprogs in host if centos or respective package with respective OS. swift support xfs
make xfs formatted partition and mount in host and gives in arg of docker -v 

EXAMPLE: /dev/sdb1 /srv/node/device0
* /etc/fstab entry can also done with reference of given below
 
* /dev/sdb /srv/node/sdb xfs noatime,nodiratime,nobarrier 0 0
* /dev/sdc /srv/node/sdc xfs noatime,nodiratime,nobarrier 0 0

* /srv/node is embedded in Dockerfile, if you want to change you can

if selinux is enabled then
* semanage fcontext -a -t swift_data_t /srv/node/device0 
* restorecon /srv/node/device0

this docker file contain build arg device
--
for single volume with single docker
* docker build -t --build-arg device=device_name image:version .
* make image from above docker command and then make instance as given below 

# For container single volume single docker
* docker run -d -v /srv/node/device0:/srv/node/device0:rw --net=host image_id

for multiple volume with single docker
* increase the build arg in docker file if you have more then 1 device, edit the Dockerfile
* docker build -t --build-arg device=device_name --build-arg device1=device1  image:version .
* make image from above docker command and then make instance as given below 

# For container multiple volume single docker
* docker run -d -v /srv/node/device0:/srv/node/device0:rw /srv/node/device1:/srv/node/device1:rw  --net=host image_id

open port in host because you are using host networking in docker 873,6200,6201,6202
