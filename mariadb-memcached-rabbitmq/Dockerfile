#mariadb server
FROM centos/systemd

#installation ntp, mariadb, rabbitmq, memcached

RUN yum -y install ntp; yum -y install centos-release-openstack-queens
RUN sed -i -e "s/enabled=1/enabled=0/g" /etc/yum.repos.d/CentOS-OpenStack-queens.repo
RUN yum --enablerepo=centos-openstack-queens -y install mariadb-server
RUN yum install epel-release
RUN yum --enablerepo=epel -y install rabbitmq-server memcached

#remove & add previous config file

RUN rm -f /etc/ntp.conf
ADD files/ntp.conf /etc/ntp.conf

RUN rm -f /etc/my.cnf
ADD files/my.cnf /etc/my.cnf

#enable services

RUN yum clean all
RUN systemctl enable ntpd; systemctl enable mariadb
RUN systemctl enable rabbitmq-server; systemctl enable  memcached 

#port expose

EXPOSE 3306
EXPOSE 123
EXPOSE 11211
EXPOSE 5672    

CMD ["/usr/sbin/init"]

