# docker build -t scikit .
FROM atlas

RUN INSTALL_PKGS="python-setuptools make gcc gcc-c++" && \
yum -y install $INSTALL_PKGS && \
yum -y clean all

RUN PIP_PKGS="pandas scikit-learn xgboost" && \
easy_install pip && \
pip install $PIP_PKGS

# docker run <container> /tmp/benchmark
ENTRYPOINT ["python"]
CMD ["--help"]
