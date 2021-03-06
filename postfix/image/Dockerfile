FROM opensuse/leap:15.0

LABEL maintainer="Thomas Schulte <thomas@cupracer.de>"

EXPOSE 25 587
CMD ["/start.sh"]

RUN zypper --non-interactive ref \
	&& zypper --non-interactive up --no-recommends \
    && zypper --non-interactive in --no-recommends \
		postfix \
		postfix-mysql \
		rsyslog		

RUN zypper --non-interactive --gpg-auto-import-keys ar --refresh https://download.opensuse.org/repositories/Cloud:/OpenStack:/Master/openSUSE_Leap_15.0/Cloud:OpenStack:Master.repo \
	&& zypper --non-interactive --gpg-auto-import-keys ref \
	&& zypper --non-interactive in --no-recommends \
		python-setuptools \
		python-supervisor

RUN zypper clean -a

COPY target/etc/supervisord.conf /etc/supervisord.conf
COPY target/start.sh /start.sh
COPY target/postfix-wrapper.sh /usr/local/bin/postfix-wrapper.sh

RUN chmod 755 /start.sh /usr/local/bin/postfix-wrapper.sh
RUN mkdir /var/run/supervisord

COPY target/etc/rsyslog.conf /etc/rsyslog.conf
COPY target/etc/postfix/main.cf /etc/postfix/main.cf
COPY target/etc/postfix/master.cf /etc/postfix/master.cf

COPY target/etc/postfix/without_ptr /etc/postfix/without_ptr
COPY target/etc/postfix/postscreen_access /etc/postfix/postscreen_access
COPY target/etc/postfix/submission_header_cleanup /etc/postfix/submission_header_cleanup

COPY target/etc/postfix/sql/mysql_virtual_alias_domain_catchall_maps.cf /etc/postfix/sql/mysql_virtual_alias_domain_catchall_maps.cf
COPY target/etc/postfix/sql/mysql_virtual_alias_domain_mailbox_maps.cf /etc/postfix/sql/mysql_virtual_alias_domain_mailbox_maps.cf
COPY target/etc/postfix/sql/mysql_virtual_alias_domain_maps.cf /etc/postfix/sql/mysql_virtual_alias_domain_maps.cf
COPY target/etc/postfix/sql/mysql_virtual_alias_maps.cf /etc/postfix/sql/mysql_virtual_alias_maps.cf
COPY target/etc/postfix/sql/mysql_virtual_domains_maps.cf /etc/postfix/sql/mysql_virtual_domains_maps.cf
COPY target/etc/postfix/sql/mysql_virtual_mailbox_maps.cf /etc/postfix/sql/mysql_virtual_mailbox_maps.cf
COPY target/etc/postfix/sql/mysql_virtual_sender_maps.cf /etc/postfix/sql/mysql_virtual_sender_maps.cf

