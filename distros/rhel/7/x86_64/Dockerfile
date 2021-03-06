FROM scratch
ADD rootfs.tar.gz /
MAINTAINER David Laube <dlaube@packet.net>
LABEL Description="Packet's RHEL 7 OS base image" Vendor="Packet.net"
LABEL Version="2018.02.20.00"
ARG SMUSER
ARG SMPASS

# Register system with redhat, temporarily
RUN subscription-manager register \
	--username $SMUSER \
	--password $SMPASS \
	--auto-attach \
	--force

RUN yum update  ${YUM_OPTS} -y && \
	yum install ${YUM_OPTS} -y \
	audit \
	bash \
	bash-completion \
	ca-certificates \
	chrony \
	cloud-init \
	cloud-utils-growpart \
	cron \
	curl \
	device-mapper-multipath \
	dhclient \
	ethstatus \
	hwdata \
	ioping \
	iotop \
	iperf \
	iscsi-initiator-utils \
	keyutils \
	locate \
	logrotate \
	make \
	mdadm \
	mg \
	microcode_ctl \
	mtr \
	net-tools \
	nmap-ncat \
	ntp \
	ntpdate \
    NetworkManager-team \
    NetworkManager-tui \
	openssh-clients \
	openssh-server \
	openssl \
	parted \
	pciutils \
	redhat-lsb-core \
	rsync \
	rsyslog \
	screen \
	socat \
	sudo \
	sysstat \
	systemd \
	tar \
	tcpdump \
	teamd \
	tmux \
	traceroute \
	tuned \
	vim \
	wget \
	&& yum clean all

# Remove default eth0 dhcp config
RUN rm -f /etc/sysconfig/network-scripts/ifcfg-eth0

# Reinstall iputils due to non-priv user bug, fix cap
RUN yum -y reinstall iputils

# Add service to fix POSIX 1003.1e capabilities on ping
RUN bash -c "$(/bin/echo -e "cat > /usr/lib/systemd/system/setcap.service <<EOM\
\n[Unit]\
\nDescription=Setup setcap ping\
\nAfter=multi-user.target\
\n \
\n[Service]\
\nType=oneshot\
\nExecStart=/usr/sbin/setcap 'cap_net_admin,cap_net_raw+ep' /usr/bin/ping\
\nRemainAfterExit=true\
\nStandardOutput=journal\
\n \
\n[Install]\
\nWantedBy=multi-user.target\
\nEOM\n")"
RUN ln -s /usr/lib/systemd/system/setcap.service /etc/systemd/system/setcap.service
RUN ln -s /usr/lib/systemd/system/setcap.service /etc/systemd/system/multi-user.target.wants/setcap.service

# Install a specific kernel and deps
#RUN yum -y install kernel-3.10.0-693.17.1.el7 microcode linux-firmware grub2-efi grub2 efibootmgr

# Adjust generic initrd
#RUN dracut --filesystems="ext4 vfat" --mdadmconf --force /boot/initramfs-3.10.0-693.17.1.el7.x86_64.img 3.10.0-693.17.1.el7.x86_64

# Adjust root account
RUN passwd -d root && passwd -l root

# Remove RHN registration
RUN rm -f /etc/sysconfig/rhn/systemid

# vim: set tabstop=4 shiftwidth=4:
