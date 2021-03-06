FROM scratch
MAINTAINER David Laube <dlaube@packet.net>
ADD rootfs.tar.gz /

LABEL name="Ubuntu Canonical Base Image" \
    vendor="Ubuntu" \
    license="GPLv2" \
    build-date="20170508"


# Setup the environment
ENV DEBIAN_FRONTEND=noninteractive

# Install packages
RUN apt-get -q update && \
    apt-get -y -qq upgrade && \
    apt-get install -y -qq \
		bash \
		bash-completion \
		bc \
		biosdevname \
		ca-certificates \
		cloud-init \
		cron \
		curl \
		dbus \
		dstat \
		ethstatus \
		file \
		fio \
		htop \
		ifenslave \
		ioping \
		iotop \
		iperf \
		iptables \
		iputils-ping \
		less \
		locales \
		locate \
		lsb-release \
		lsof \
		make \
		man-db \
		mdadm \
		mg \
		mosh \
		mtr \
		multipath-tools \
		nano \
		net-tools \
		netcat \
		nmap \
		ntp \
		ntpdate \
		open-iscsi \
		python-apt \
		python-yaml \
		rsync \
		rsyslog \
		screen \
		shunit2 \
		socat \
		software-properties-common \
		ssh \
		sudo \
		sysstat \
		tar \
		tcpdump \
		tmux \
		traceroute \
		unattended-upgrades \
		uuid-runtime \
		vim \
		wget \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/log/*

# Install a specific kernel
RUN apt-get -q update && \
	apt-get -y -qq upgrade && \
	apt-get install -y -qq \
	linux-image-4.13.0-25-generic \
	linux-image-extra-4.13.0-25

# Configure locales
RUN locale-gen en_US.UTF-8 && \
    dpkg-reconfigure locales

# Fix permissions
RUN chown root:syslog /var/log \
	&& chmod 755 /etc/default

# Fix cloud-init EC2 warning
RUN touch /root/.cloud-warnings.skip

# vim: set tabstop=4 shiftwidth=4:
