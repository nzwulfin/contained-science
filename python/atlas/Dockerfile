# docker build -t atlas .
FROM rhel7

RUN INSTALL_PKGS="numpy scipy blas python-setuptools make gcc gcc-c++" && \
yum -y install $INSTALL_PKGS && \
yum -y clean all

# docker run <container> /tmp/benchmark
ENTRYPOINT ["python"]
CMD ["--help"]
