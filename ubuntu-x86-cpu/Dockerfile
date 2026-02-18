FROM ubuntu:22.04

# Non-root user for safety UID and GID outside of normal range to avoid conflicts with host users
ARG UID=80000
ARG GID=80000

RUN groupadd -g ${GID} wakisuser \
    && useradd -m -s /bin/bash -u ${UID} -g ${GID} wakisuser

WORKDIR /home/wakisuser

# Install system dependencies while still in root 
RUN apt-get update && \
    apt-get install -y wget bzip2 ca-certificates curl git && \
    rm -rf /var/lib/apt/lists/*

# Switch to user for Miniforge setup and subsequent steps
USER wakisuser

# Install Miniforge (x86_64)
ENV MINIFORGE_VERSION=latest
ENV CONDA_DIR=/home/wakisuser/miniforge3
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
RUN conda run -n wakis pip install --no-cache-dir wakis['notebook'] && \
    conda run -n wakis pip uninstall vtk -y && \
    conda run -n wakis pip install --extra-index-url https://wheels.vtk.org vtk-osmesa

# Set bash as entrypoint
# ENTRYPOINT ["/bin/bash"]

# RUN echo 'source /opt/conda/etc/profile.d/conda.sh && conda activate wakis' >> /root/.bashrc
RUN conda init bash
RUN echo 'conda activate wakis' >> /home/wakisuser/.bashrc