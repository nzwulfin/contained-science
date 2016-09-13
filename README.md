# contained-science
Various container examples for scientific computing benchmarks

## GOAL:
Distributing reusable and consistent platforms with multiple options for experimental use.  This provides experimenters ways to try different packages without major modifications to code or containers.  

## EXAMPLE:
Comparing the RHEL distribution ATLAS packages to a locally compiled OpenBLAS package on RHEL 7, with a simple set of NumPy scripts.

### Building the ATLAS platform container
On a RHEL 7 host with docker installed:
```
cd python/
ln Dockerfile.atlas Dockerfile
```
use `-f` if Dockerfile already exists

Build the platform container
```
docker build -t atlas .
```

Edit the experiment container to match the name given to the platform container in the previous step

```
ln Dockerfile.exp Dockerfile -f
vi Dockerfile

FROM atlas
```

### Build the experiment container

```
docker build -t exp-at .
```

To run the experiment package
```
docker run exp-at /tmp/[test script]
```

#### To see the numpy configuration in the container
`docker run exp-at -c 'import numpy; numpy.show_config()'`

#### To add a test
* Create the new test in the python/ directory
* Rebuild experiments tarball:
`tar cf experiments.tar *.py`
* Rebuild the experiment container

### Building the OpenBLAS platform container
On a RHEL 7 host with docker installed:
```
cd python/
ln Dockerfile.oblas Dockerfile
```
use `-f` if Dockerfile already exists

Build the platform container
```
docker build -t oblas .
```

Edit the experiment container to match the name given to the platform container in the previous step

```
ln Dockerfile.exp Dockerfile -f
vi Dockerfile

FROM oblas
```

### Build the experiment container

```
docker build -t exp-ob .
```

To run the experiment package
```
docker run exp-ob /tmp/[test script]
```

#### To see the numpy configuration in the container
`docker run exp-ob -c 'import numpy; numpy.show_config()'`

### Run the experiment containers side by side

[root@testbed-7 python]# docker run exp-ob /tmp/test_scipy.py
cholesky: 0.0180000305176 sec
svd: 0.730859804153 sec
[root@testbed-7 python]# docker run exp-at /tmp/test_scipy.py
cholesky: 0.0387578010559 sec
svd: 10.1107485771 sec

#### To add a test
* Create the new test in the python/ directory
* Rebuild experiments tarball:
`tar cf experiments.tar *.py`
* Rebuild the experiment container

## GOAL:
Redistribute a common set of R packages from CRAN to experimenters, without need for local installation

### Building the R platform container
On a RHEL 7 host with docker installed:

```
cd R/
subscription-manager repos --enable rhel-7-server-optional-rpms
ln Dockerfile.R Dockerfile
```
use `-f` if Dockerfile already exists

Build the platform container
```
docker build -t rplatform .
```

Edit the experiment container to match the name given to the platform container in the previous step

```
ln Dockerfile.exp Dockerfile -f
vi Dockerfile

FROM rplatform
```

### Build the experiment container

```
docker build -t exp-r .
```

To run the experiment package
```
docker run -e RTEST=[test name] exp-r
```

#### To add a test
* Create the new test in the R/ directory
* Rebuild experiments tarball:
 `tar cf experiments.tar *.R`
* Rebuild the experiment container
