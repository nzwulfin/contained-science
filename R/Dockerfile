FROM rhel7

# Needs rhel-7-server-optional-rpms enabled on the docker host that builds the container
RUN yum -y install http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-8.noarch.rpm
RUN INSTALL_PKGS="time texinfo-tex R" && yum-config-manager --enable rhel-7-server-optional-rpms && yum -y install $INSTALL_PKGS

COPY bench.R /tmp/
COPY R-benchmark-25.R /tmp/
RUN R -e 'install.packages("SuppDists", dependencies = TRUE, repos="http://cran.us.r-project.org")'

# docker run -e RTEST=/tmp/benchmark
CMD  time R < $RTEST --slave

