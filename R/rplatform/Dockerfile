# docker build -t rplatform .
FROM rhel7

# Needs rhel-7-server-optional-rpms enabled on the docker host that builds the container
RUN yum -y install http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-8.noarch.rpm

RUN INSTALL_PKGS="texinfo-tex R ed" && \
yum-config-manager --disable "*" && \
yum-config-manager --enable rhel-7-server-rpms && \
yum-config-manager --enable rhel-7-server-optional-rpms && \
yum-config-manager --enable epel && \
yum -y install $INSTALL_PKGS && \
yum -y clean all

# docker run -e RTEST=/tmp/benchmark
CMD  R < $RTEST --slave
