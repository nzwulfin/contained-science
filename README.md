# contained-science
Various container examples for scientific computing benchmarks

## Using the python container
On a RHEL 7 host with docker installed:

docker build -t pybench .

docker run pybench /tmp/[test script]

### To see the numpy configuration in the container
docker run pybench -c 'import numpy; numpy.show_config()'

## To add a test
Create the new test in the python/ directory
Add a new COPY line to the Dockerfile
Rebuild the container

## Using the R container
On a RHEL 7 host with docker installed:

subscription-manager repos --enable rhel-7-server-optional-rpms

docker build -t rbench .

docker run -e RTEST=[test name] rbench 

## To add a test
Create the new test in the R/ directory
Add a new COPY line to the Dockerfile
Rebuild the container
