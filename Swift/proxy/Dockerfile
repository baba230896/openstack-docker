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

#enable services

RUN yum clean all
RUN systemctl enable ntpd; systemctl enable openstack-swift-proxy

#port expose

EXPOSE 8080


CMD ["/usr/sbin/init"]
 


