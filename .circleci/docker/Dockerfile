FROM fedora:latest
# Install barebones packages.
RUN dnf -y install \
    llvm \
    llvm-devel \
    gcc-c++ \
    clang \
    libstdc++ \
    libstdc++-devel \
    findutils \
    wget \
    git

COPY granule "/root/host-repo"
RUN mkdir "/root/stack-root"
# Directory for (ci) git project, not available during docker build.
RUN mkdir "/root/repo"
# Directory for build host git project, availble during docker build.

# Set stack root directory
ENV STACK_ROOT="/root/stack-root"
ENV PATH="/root/.local/bin:${PATH}"
ENV LANG="en_GB.UTF-8"

# Install stack
RUN wget -qO- https://get.haskellstack.org/ | sh

# Switch to host project to find dependencies to install.
WORKDIR "/root/host-repo"
# Delete any build directories copied from host.
RUN find . -path '*/.stack-work/*' -delete
# Install dependencies to stack root
RUN stack install --dependencies-only granule-frontend
RUN stack install --dependencies-only granule-compiler
RUN stack install --dependencies-only granule-interpreter
RUN stack install --dependencies-only granule-repl
# Delete the host-repo directory.
RUN rm -rf "/root/host-repo"

# Switcheroo ready for ci
WORKDIR "/root/repo"
