# docker build -t atlas-exp .
# docker build -t oblas-exp .
# docker build -t scikit-exp .
FROM atlas
#FROM oblas
#FROM scikit

ARG workdir=/tmp

ADD experiments.tar $workdir
WORKDIR $workdir
