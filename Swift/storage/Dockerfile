FROM centos/systemd
ARG device

#install swift storage

RUN yum -y install ntp; yum -y install centos-release-openstack-queens; yum -y install epel-release
RUN sed -i -e "s/enabled=1/enabled=0/g" /etc/yum.repos.d/CentOS-OpenStack-queens.repo
RUN yum --enablerepo=centos-openstack-queens,epel -y install openstack-swift-account openstack-swift-container openstack-swift-object xfsprogs rsync openssh-clients



ADD files/rsyncd.conf /etc/rsyncd.conf
ADD files/account-server.conf /etc/swift/account-server.conf
ADD files/object-server.conf /etc/swift/object-server.conf
ADD files/container-server.conf /etc/swift/container-server.conf

ADD files/account.ring.gz /etc/swift/account.ring.gz
ADD files/container.ring.gz /etc/swift/container.ring.gz
ADD files/object.ring.gz /etc/swift/object.ring.gz

ADD files/swift.conf /etc/swift/swift.conf



RUN chown swift. /etc/swift/*.gz

RUN mkdir -p mkdir -p /srv/node/$device
RUN chown -R swift:swift /srv/node
RUN mkdir -p /var/cache/swift
RUN chown -R root:swift /var/cache/swift
RUN chmod -R 775 /var/cache/swift



RUN yum clean all
RUN systemctl enable ntpd
RUN systemctl enable rsyncd.service
RUN systemctl enable openstack-swift-account.service openstack-swift-account-auditor.service openstack-swift-account-reaper.service openstack-swift-account-replicator.service

RUN systemctl enable openstack-swift-container.service openstack-swift-container-auditor.service openstack-swift-container-replicator.service openstack-swift-container-updater.service

RUN systemctl enable openstack-swift-object.service openstack-swift-object-auditor.service openstack-swift-object-replicator.service openstack-swift-object-updater.service

#port expose

EXPOSE 873
EXPOSE 6200
EXPOSE 6201
EXPOSE 6202


CMD ["/usr/sbin/init"]

