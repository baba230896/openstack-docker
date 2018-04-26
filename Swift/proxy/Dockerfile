FROM centos/systemd

#install swift proxy

RUN yum -y install ntp; yum -y install centos-release-openstack-queens; yum -y install epel-release
RUN sed -i -e "s/enabled=1/enabled=0/g" /etc/yum.repos.d/CentOS-OpenStack-queens.repo
RUN yum --enablerepo=centos-openstack-queens,epel -y install openstack-swift-proxy python-memcached openssh-clients

#remove and add

RUN rm -f /etc/ntp.conf
ADD files/ntp.conf /etc/ntp.conf


RUN rm -f /etc/swift/proxy-server.conf
ADD files/proxy-server.conf /etc/swift/proxy-server.conf

RUN rm -f /etc/swift/swift.conf
ADD files/swift.conf /etc/swift/swift.conf


RUN chown -R root:swift /etc/swift
#enable services

RUN yum clean all
RUN systemctl enable ntpd; systemctl enable openstack-swift-proxy memcached.service
RUN systemctl enable openstack-swift-account.service openstack-swift-account-auditor.service openstack-swift-account-reaper.service openstack-swift-account-replicator.service
RUN systemctl enable openstack-swift-container.service openstack-swift-container-auditor.service openstack-swift-container-replicator.service openstack-swift-container-updater.service
RUN systemctl enable openstack-swift-object.service openstack-swift-object-auditor.service openstack-swift-object-replicator.service openstack-swift-object-updater.service

#port expose

EXPOSE 8080


CMD ["/usr/sbin/init"]
 


