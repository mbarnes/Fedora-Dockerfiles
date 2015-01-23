FROM fedora:20
MAINTAINER Colin Walters <walters@redhat.com>

# http://www.projectatomic.io/blog/2014/09/running-syslog-within-a-docker-container/

RUN yum update -y && yum -y install rsyslog && yum clean all
RUN sed -i -e 's,$ModLoad imjournal,#\0,' \
           -e 's,$IMJournalStateFile,#\0,' \
           -e 's,$OmitLocalLogging on,$OmitLocalLogging off,' \
           /etc/rsyslog.conf

# References the systemd journal.
RUN rm -rf /etc/rsyslog.d/listen.conf

# /proc/kmsg not readable even as root for some reason.
# Test: python -c "open('/proc/kmsg')"
RUN sed -i -e 's,$ModLoad imklog,#\0,' /etc/rsyslog.conf

# Still getting errors on imuxsock...
#
# rsyslogd: cannot create '/dev/log': Address already in use
# rsyslogd: imuxsock does not run because we could not aquire any socket
# rsyslogd: activation of module imuxsock failed
#
# Tried substituting "$ModLoad imuxsock" with this, but no change:
#
#   module(load="imuxsock" SysSock.Unlink="off")
#
# The Unlink option is supposed to prevent rsyslogd from unlinking and
# recreating the socket on startup, since Docker already mounted it.

CMD ["/usr/sbin/rsyslogd", "-n"]

