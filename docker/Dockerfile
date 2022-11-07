# Base Image
FROM debian:9

# Metadata
LABEL description="UEF Bioinformatics Center / PheWeb platform"
LABEL maintainer="Teemu Kuulasmaa <teemu.kuulasmaa@uef.fi>"

# Environment
ENV LC_CTYPE en_US.UTF-8
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV BASH_ENV ~/.bashrc

# PheWeb data dir. This directory must contain generated-by-pheweb directory together with config.py and pheno-list.json files. 
# Optionally the directory may contain also custom_templates directory.
ENV PHEWEB_DIR /data

# Install required 
RUN apt-get -q update && \
    apt-get -q -y install --no-install-recommends build-essential ca-certificates curl unzip git git wget python3-pip python3-dev libffi-dev libz-dev libssl-dev libbz2-dev liblzma-dev --yes --fix-missing && \
    rm -rf /var/lib/apt/lists/*

# Create directories
#RUN mkdir -p /opt $PHEWEB_DIR
RUN mkdir -p $PHEWEB_DIR

# Define working directory.
#WORKDIR /opt

# Clone PheWeb repository
#RUN git clone https://uef-bic@dev.azure.com/uef-bic/public/_git/pheweb /opt/pheweb

RUN pip3 install -U cython setuptools wheel && \
    pip3 install git+https://dev.azure.com/uef-bic/public/_git/pheweb
#    pip3 install /opt/pheweb

# clean-up
RUN apt-get -q -y purge curl unzip && \
    apt-get -q -y autoremove && \
    apt-get -q -y clean && \
    rm -rf /var/lib/apt/lists/* /var/lib/apt/extended_states \
        /var/cache/* /var/lib/dpkg/status /var/lib/dpkg/status-old \
        /etc/ld.so.cache /var/log/faillog /var/log/lastlog \
        /var/lib/apt/extended_states

# Define working directory.
#WORKDIR /

# Define default entrypoint.
CMD cd $PHEWEB_DIR && pheweb serve --port 8080 --num-workers 4
