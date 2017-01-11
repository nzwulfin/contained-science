# docker build -t scikit .
FROM atlas

RUN PIP_PKGS="pandas scikit-learn xgboost" && \
easy_install pip && \
pip install $PIP_PKGS

# docker run <container> /tmp/benchmark
ENTRYPOINT ["python"]
CMD ["--help"]
