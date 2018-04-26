FROM centos/systemd
ARG keystone_host
ARG password=adminpassword
ARG keystone_user

#install keystone

RUN yum -y install ntp; yum -y install centos-release-openstack-queens; yum -y install epel-release
RUN sed -i -e "s/enabled=1/enabled=0/g" /etc/yum.repos.d/CentOS-OpenStack-queens.repo
RUN yum --enablerepo=centos-openstack-queens,epel -y install openstack-keystone  httpd mod_wsgi


#remove and add configuration files


RUN rm -f /etc/ntp.conf
ADD files/ntp.conf /etc/ntp.conf


RUN rm -f /etc/keystone/keystone.conf
ADD files/keystone.conf /etc/keystone/keystone.conf

RUN rm -f /var/lib/keystone/keystone.sqlite

RUN keystone-manage db_sync

#initialize keys

RUN keystone-manage fernet_setup --keystone-user $keystone_user --keystone-group $keystone_user 
RUN keystone-manage credential_setup --keystone-user $keystone_user --keystone-group $keystone_user

#bootstrap keystone 

RUN keystone-manage bootstrap --bootstrap-password $password --bootstrap-admin-url http://$keystone_host:35357/v3/ --bootstrap-internal-url http://$keystone_host:5000/v3/ --bootstrap-public-url http://$keystone_host:5000/v3/ --bootstrap-region-id RegionOne

RUN rm -f /etc/httpd/conf/httpd.conf
ADD files/httpd.conf /etc/httpd/conf/httpd.conf

RUN ln -s /usr/share/keystone/wsgi-keystone.conf /etc/httpd/conf.d/ 


#enable services

RUN yum clean all
RUN systemctl enable ntpd; systemctl enable httpd 

#port expose

EXPOSE 5000  
EXPOSE 80
EXPOSE 35357


CMD ["/usr/sbin/init"]
