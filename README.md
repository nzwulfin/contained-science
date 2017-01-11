# contained-science
Building platform containers for experimenters and analysts to reuse

## GOAL:
Building  distributable, reusable, and consistent platforms with multiple options for reuse.  This provides experimenters and analysts ways to try different packages without major modifications to code or containers.  

## Python EXAMPLE:
Provide two options for Linear Algebra APIs in python 2: ATLAS and OpenBLAS.  OpenBLAS is built from source to show parity with building platforms on physical or virutal machines. These containers will execute python as if from the command line on a host.

### Creating the ATLAS platform container
On a RHEL 7 host with docker installed, in the `python` directory:

Build the ATLAS platform container
```
docker build -t atlas atlas/
```

### Creating the experiment container
Edit the Dockerfile to match the tag `-t atlas` given to the platform container in the previous step

```
vi exp/Dockerfile

FROM atlas
```
#### To add a test
* Create the new test in the `exp` directory
* Rebuild experiments tarball:
`tar cf experiments.tar *.py`
* Rebuild the experiment container

The experiment container will extract the tarball contents to `/tmp`

Build the experiment container
```
docker build -t exp-at exp/
```
#### To see the numpy configuration in the container
```
docker run exp-at -c 'import numpy; numpy.show_config()'
```

#### To run a benchmark script from the tarball
```
docker run exp-at /tmp/[test script]
docker run exp-at /tmp/test_scipy
```

### Creating the OpenBLAS platform container
On a RHEL 7 host with docker installed, in the `python` directory:

Build the platform container
```
docker build -t oblas oblas/
```

### Creating the experiment container
Edit the Dockerfile to match the tag `-t oblas` given to the platform container in the previous step

```
vi exp/Dockerfile

FROM oblas
```
#### To add a test
* Create the new test in the `exp` directory
* Rebuild experiments tarball:
`tar cf experiments.tar *.py`
* Rebuild the experiment container

The experiment container will extract the tarball contents to `/tmp`

Build the experiment container
```
docker build -t exp-ob exp/
```

#### To see the numpy configuration in the container
```
docker run exp-ob -c 'import numpy; numpy.show_config()'
```


#### To run a benchmark script from the tarball
```
docker run exp-at /tmp/[test script]
docker run exp-at /tmp/test_scipy
```

### Comparison of the two containers using a scipy benchmark
```
[root@testbed-7 python]# docker run exp-ob /tmp/test_scipy.py
cholesky: 0.0180000305176 sec
svd: 0.730859804153 sec

[root@testbed-7 python]# docker run exp-at /tmp/test_scipy.py
cholesky: 0.0387578010559 sec
svd: 10.1107485771 sec
```

## R EXAMPLE:
Provide R from upstream sources in a consistent manner.  These containers will execute R as if from the command line on a host.

### Creating the R platform container
On a RHEL 7 host with docker installed, in the rplatform directory:
*The Optional repository only is required on RHEL as some dependencies are not in the standard Server channel*

```
subscription-manager repos --enable rhel-7-server-optional-rpms
docker build -t rplatform rplatform/
```
### Creating the experiment container
Edit the experiment container to match the name given to the platform container in the previous step.  Any CRAN packages required would be added here.

```
vi exp/Dockerfile

FROM rplatform
```
#### To add a test
* Create the new test in the exp/ directory
* Rebuild experiments tarball:
 `tar cf experiments.tar *.R`
* Rebuild the experiment container
* Ensure any additional CRAN packages have been added to the Dockerfile

The experiment container will extract the tarball contents to /tmp

### Build the experiment container

```
docker build -t exp-r exp/
```

To run the benchmark package
```
docker run -e RTEST=[test name] exp-r
```
