FROM ubuntu:22.04

# Install system dependencies (if needed)
RUN apt-get update && \
    apt-get install -y wget bzip2 ca-certificates curl git && \
    rm -rf /var/lib/apt/lists/*

# Install Miniforge (x86_64)
ENV MINIFORGE_VERSION=latest
ENV CONDA_DIR=/opt/conda
RUN wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh -O /tmp/miniforge.sh && \
    bash /tmp/miniforge.sh -b -p $CONDA_DIR && \
    rm /tmp/miniforge.sh

ENV PATH=$CONDA_DIR/bin:$PATH

# Create a conda environment with Python 3.11
RUN conda create -y -n wakis python=3.11 && \
    conda clean -afy

# Activate environment by default
SHELL ["conda", "run", "-n", "wakis", "/bin/bash", "-c"]

# Install pip packages (example)
RUN pip install --no-cache-dir wakis['notebook'] && \
    pip uninstall vtk -y && \
    pip install --extra-index-url https://wheels.vtk.org vtk-osmesa
