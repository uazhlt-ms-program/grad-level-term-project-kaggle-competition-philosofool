# .devcontainer/Dockerfile
# Lightweight Python base image
FROM python:3.11-slim

# Set working directory
WORKDIR /workspace

# Install system utilities
RUN apt-get update && apt-get install -y \
    git curl vim build-essential sudo \
    libpq-dev postgresql-client \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip
RUN python -m pip install --upgrade pip
RUN pip install --no-cache-dir pandas ipykernel numpy pytest matplotlib scikit-learn
RUN pip install --no-cache-dir kaggle

# Create non-root user (matches host UID)
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Nice prompt
RUN echo 'export PS1="[\[\033[0;36m\]\u@\h \[\033[0;33m\]\D{%Y-%m-%d %H:%M:%S}\[\033[0m\]] \[\033[0;32m\]\w\[\033[0m\]\\$ "' >> /home/$USERNAME/.bashrc \
    && chown $USER_UID:$USER_GID /home/$USERNAME/.bashrc

# Switch to non-root user
USER $USERNAME