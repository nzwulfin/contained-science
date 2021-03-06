# docker build -t oblas .
# Derived from instructions here https://gist.github.com/lucasoldaini/2d548cafa7ea4d147aa2bb1c7cd393cc
FROM rhel7
ARG OBLASVER=v0.2.18
ARG NUMPYVER=v1.11.3
ARG SCIPYVER=v0.15.1

LABEL openblas=$OBLASVER \
      numpy=$NUMPYVER \
      scipy=$SCIPYVER


# Needs rhel-7-server-optional-rpms enabled on the docker host that builds the container
# yum-config-manager won't always succeed if it's the first yum command run
RUN  yum repolist && \
yum-config-manager --enable rhel-7-server-optional-rpms && \
INSTALL_PKGS="gcc gcc-gfortran gcc-c++ binutils autoconf automake make git python-setuptools Cython python-devel" && \
yum -y install $INSTALL_PKGS && \
yum -y clean all

RUN git clone https://github.com/xianyi/OpenBLAS && \
git clone https://github.com/numpy/numpy.git &&\
git clone https://github.com/scipy/scipy.git

# OpenBLAS build

COPY OpenBLAS.conf /etc/ld.so.conf.d/
RUN cd /OpenBLAS && \
git checkout tags/$OBLASVER && \
make CC="gcc -m64" FC="gfortran -m64" RANLIB="ranlib" FFLAGS=" -O2 -fPIC" TARGET= BINARY=64 USE_THREAD=1 && \
make install && \
ldconfig && \
make clean

# NumPy build
COPY site.cfg /numpy/

RUN cd /numpy && \
git checkout tags/$NUMPYVER && \
unset CPPFLAGS && \
unset LDFLAGS && \
python setup.py build --fcompiler=gnu95 && \
python setup.py install && \
git clean -xdf

# SciPY build

RUN cd /scipy && \
git checkout tags/$SCIPYVER && \
python setup.py config_fc --fcompiler=gnu95 install && \
git clean -xdf

# docker run <container> /tmp/benchmark
ENTRYPOINT ["python"]
CMD ["--help"]
