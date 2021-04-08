# From https://towardsdatascience.com/how-to-run-jupyter-notebook-on-docker-7c9748ed209f
ARG BASE_CONTAINER=jupyter/pyspark-notebook
FROM $BASE_CONTAINER
LABEL author="Michael Herzog"
USER root
RUN pip install findspark confluent_kafka pyspark flask nltk
# Switch back to jovyan to avoid accidental container runs as root
USER $NB_UID

ENV JUPYTER_ENABLE_LAB="yes"

