```
FROM debian:10

  

LABEL maintainer="noah"

  

ENV DEBIAN_FRONTEND=noninteractive

  

# Replace default apt sources with archived Debian 10 repos

RUN sed -i 's|deb.debian.org|archive.debian.org|g' /etc/apt/sources.list && \

sed -i 's|security.debian.org|archive.debian.org|g' /etc/apt/sources.list && \

echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99no-check-valid

  

# Update and install GCC 8 + build tools

RUN apt-get update && apt-get install -y \

build-essential \

gcc-8 g++-8 \

make cmake gdb wget curl vim git python3 python3-pip \

man-db less procps net-tools iproute2 iputils-ping \

&& update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 100 \

&& update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-8 100 \

&& rm -rf /var/lib/apt/lists/*

  

WORKDIR /root

  

CMD ["bash"]
```