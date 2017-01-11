# docker build -t r-exp .
FROM rplatform

RUN R -e 'install.packages("data.table", dependencies = TRUE, repos="http://cran.us.r-project.org")'

RUN R -e 'install.packages("SuppDists", dependencies = TRUE, repos="http://cran.us.r-project.org")'

ADD experiments.tar /tmp/
