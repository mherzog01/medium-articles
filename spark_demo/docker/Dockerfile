# From https://towardsdatascience.com/how-to-run-jupyter-notebook-on-docker-7c9748ed209f
ARG BASE_CONTAINER=jupyter/minimal-notebook
FROM $BASE_CONTAINER
LABEL author="Michael Herzog"
USER root
RUN pip install jupyterlab findspark confluent_kafka pyspark
# Switch back to jovyan to avoid accidental container runs as root
USER $NB_UID