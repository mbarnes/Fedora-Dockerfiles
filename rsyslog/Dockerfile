FROM fedora:20
MAINTAINER Colin Walters <walters@redhat.com>

# http://www.projectatomic.io/blog/2014/09/running-syslog-within-a-docker-container/

RUN yum update -y && yum -y install rsyslog && yum clean all
RUN sed -i -e 's,$ModLoad imjournal,#\0,' \
           -e 's,$IMJournalStateFile,#\0,' \
           -e 's,$OmitLocalLogging on,$OmitLocalLogging off,' \
           /etc/rsyslog.conf
CMD ["/usr/sbin/rsyslogd", "-n"]

