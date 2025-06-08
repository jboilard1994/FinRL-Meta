FROM python:3.8

ENV VENV_DIR=/opt/venv/_dev
ENV PATH=$VENV_DIR/bin:$PATH
SHELL ["/bin/bash", "-c"]

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    wget \
    build-essential \
    gcc \
    g++ \
    swig \
    curl \
    python3-dev

RUN wget http://prdownloads.sourceforge.net/ta-lib/ta-lib-0.4.0-src.tar.gz && \
    tar -xvzf ta-lib-0.4.0-src.tar.gz && \
    cd ta-lib/ && \
    ./configure --prefix=/usr && \
    make && \
    make install && \
    ldconfig && \
    cd .. && rm -rf ta-lib ta-lib-0.4.0-src.tar.gz

COPY requirements.txt .

RUN pip install -r requirements.txt -I

RUN pip install jupyterlab

# Add Tini. Tini operates as a process subreaper for jupyter. This prevents kernel crashes.
ENV TINI_VERSION v0.6.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod +x /usr/bin/tini

COPY . /workspace
WORKDIR /workspace