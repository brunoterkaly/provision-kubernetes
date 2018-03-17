FROM jenkinsci/blueocean:latest
USER root

RUN apk add --no-cache --update \
    python \
    python-dev \
    py-pip \
    build-base \
    && pip install virtualenv \
    && rm -rf /var/cache/apk/*

RUN pip install -U setuptools
RUN pip install -U pylint
USER jenkins
