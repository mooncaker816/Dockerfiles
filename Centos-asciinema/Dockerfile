FROM centos

RUN yum -y install epel-release \
    python34 \
    python34-setuptools && \
    yum clean all && \
    rm -rf /var/cache/yum

RUN easy_install-3.4 pip
RUN pip3 install asciinema
ENV LANG en_US.utf8

ENTRYPOINT [ "/bin/bash" ]