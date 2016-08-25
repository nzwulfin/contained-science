FROM rhel7

RUN INSTALL_PKGS="numpy scipy blas" && yum -y install $INSTALL_PKGS

#COPY test_numpy.py /tmp/
#COPY test_scipy.py /tmp/
ADD experiments.tar /tmp/

# docker run <container> /tmp/benchmark
ENTRYPOINT ["python"]
CMD ["--help"]

