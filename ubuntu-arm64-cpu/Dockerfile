FROM --platform=arm64 ubuntu:24.04

# System dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        python3.11 python3.11-venv python3-pip \
        git build-essential ca-certificates \
        libgl1-mesa-glx libosmesa6 \
        && rm -rf /var/lib/apt/lists/*


# Non-root user for safety
RUN useradd -ms /bin/bash wakisuser
USER wakisuser
WORKDIR /home/wakisuser

# Python venv
RUN python3.12 -m venv venv
ENV PATH="/home/wakisuser/venv/bin:$PATH"

# Copy code
RUN git clone https://github.com/ImpedanCEI/wakis.git /home/wakisuser/wakis
WORKDIR /home/wakisuser/wakis

# Install Wakis, notebook, and VTK (CPU only, no cupy)
RUN pip install --upgrade pip \
 && pip install .['notebook'] \
 && pip uninstall vtk -y \
 && pip install --extra-index-url https://wheels.vtk.org vtk-osmesa
