# docker build -t 6-atlas .
FROM rhel6

RUN INSTALL_PKGS="numpy scipy blas" && \
yum -y install $INSTALL_PKGS && \
yum -y clean all

# docker run <container> /tmp/benchmark
ENTRYPOINT ["python"]
CMD ["--help"]
