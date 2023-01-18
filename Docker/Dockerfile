ARG DEBIAN_CODENAME=bullseye
FROM debian:${DEBIAN_CODENAME}

ARG DEBIAN_MAJOR_VER=11
ARG SALT_MAJOR_VER=3005
ARG SALT_MINOR_VER=1
ARG SALT_BUILD="ds-1"
ARG SALT_VER="${SALT_MAJOR_VER}.${SALT_MINOR_VER}+${SALT_BUILD}"

# Safely fail in case of failure in piped command
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install latest available version of curl
RUN apt-get update && \
    apt-get satisfy curl -y && \
    apt-get install --no-install-recommends glibc-source=2.31-13+deb11u5 \
                                            curl=7.74.0-1.3+deb11u3 \
                                            bsdmainutils=12.1.7+nmu3 \
                                            dctrl-tools=2.24-3+b1 \
                                            debconf-utils=1.5.77 \
                                            dmidecode=3.3-2 \
                                            net-tools=1.60+git20181103.0eebece-1 \
                                            openssh-client=1:8.4p1-5+deb11u1 \
                                            -y \
                                            && apt-get clean \
                                            && rm -rf /var/lib/apt/lists/*

RUN mkdir /etc/apt/keyrings && \
    curl -fsSL -o /etc/apt/keyrings/salt-archive-keyring.gpg "https://repo.saltproject.io/salt/py3/debian/${DEBIAN_MAJOR_VER}/amd64/${SALT_MAJOR_VER}/salt-archive-keyring.gpg" && \
    echo "deb [signed-by=/etc/apt/keyrings/salt-archive-keyring.gpg arch=amd64] https://repo.saltproject.io/salt/py3/debian/${DEBIAN_MAJOR_VER}/amd64/${SALT_MAJOR_VER} bullseye main" | tee /etc/apt/sources.list.d/salt.list

# Second apt-get update required to fetch from repo.saltproject.io
RUN apt-get update -o Dir::Etc::sourcelist=/etc/apt/sources.list.d/salt.list && \
    apt-get install --no-install-recommends -t "o=SaltProject,a=stable" salt-master=${SALT_VER} \
                                                                        salt-minion=${SALT_VER} \
                                                                        salt-ssh=${SALT_VER} \
                                                                        salt-syndic=${SALT_VER} \
                                                                        salt-cloud=${SALT_VER} \
                                                                        salt-api=${SALT_VER} \
                                                                        -y \
                                                                        && apt-get clean \
                                                                        && rm -rf /var/lib/apt/lists/*
COPY salt-services.sh /usr/bin
RUN chmod +x /usr/bin/salt-services.sh

EXPOSE 8000 4505-4506

CMD [ "/usr/bin/salt-services.sh" ]
