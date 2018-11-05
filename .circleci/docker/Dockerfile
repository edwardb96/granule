# This script expects the build context to contain a folder granule-ci
# this should be an up-to-date version of the granule repository with a
# working package configuration.
FROM fedora:latest
# Install barebones packages.
RUN dnf -y install \
    llvm \
    llvm-devel \
    gcc-c++ \
    clang \
    z3 \
    libstdc++ \
    libstdc++-devel \
    findutils \
    wget \
    git

COPY granule-ci "/root/host-repo"
RUN mkdir "/root/stack-root"
# Directory for (ci) git project, not available during docker build.
RUN mkdir "/root/repo"
# Directory for build host git project, availble during docker build.

# Set stack root directory
ENV STACK_ROOT="/root/stack-root"
# Add stack bin to path
ENV PATH="/root/.local/bin:${PATH}"
# Set C Locale
ENV LANG="en_GB.UTF-8"

# Install stack
RUN wget -qO- https://get.haskellstack.org/ | sh

# Switch to host project to find dependencies to install.
WORKDIR "/root/host-repo"
# Delete any build directories copied from host.
RUN find . -path '*/.stack-work/*' -delete
# Install dependencies to stack root (including test dependencies)
RUN stack test --dependencies-only
# Delete the host-repo directory.
RUN rm -rf "/root/host-repo"

# Switcheroo ready for ci
WORKDIR "/root/repo"
