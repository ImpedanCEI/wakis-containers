FROM --platform=arm64 ubuntu:24.04

# Non-root user for safety UID and GID outside of normal range to avoid conflicts with host users
ARG UID=80000
ARG GID=80000

RUN groupadd -g ${GID} wakisuser \
    && useradd -m -s /bin/bash -u ${UID} -g ${GID} wakisuser

WORKDIR /home/wakisuser

# System dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        python3.12 python3.12-venv python3-pip \
        git build-essential ca-certificates \
        libgl1 libglx-mesa0 libosmesa6 \
        && rm -rf /var/lib/apt/lists/*

# Python venv
RUN python3.12 -m venv venv
ENV PATH="/home/wakisuser/venv/bin:$PATH"

# Copy code
RUN git clone https://github.com/ImpedanCEI/wakis.git /home/wakisuser/wakis
WORKDIR /home/wakisuser/wakis

# Install Wakis, notebook, and VTK (CPU only, no cupy)
RUN . /home/wakisuser/venv/bin/activate \
 && pip install --upgrade pip \
 && pip install .['notebook'] \
 && pip uninstall vtk -y \
 && pip install --extra-index-url https://wheels.vtk.org vtk-osmesa
