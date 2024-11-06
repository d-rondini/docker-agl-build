FROM ubuntu:22.04

# Environment variables affecting AGl build target
ENV AGL_BRANCH="quillback"
ENV AGL_MANIFEST="quillback_17.1.2.xml"
ENV AGL_MACHINE="ebisu"
ENV AGL_FEATURE="agl-demo"
ENV AGL_RECIPE="agl-ivi-demo-qt"
ENV GIT_USER="AGL builder"
ENV GIT_EMAIL="info@samplesw.com"

# Support environment variables
ENV LANG="en_US.UTF-8"
ENV DEBIAN_FRONTEND=noninteractive
ENV AGL_TOP="/home/builder/AGL"
ENV EBISU_BIN_PATH="/home/builder/inputdata"
ENV PATH="$PATH:/home/builder/bin"

# Install packages required by AGL
RUN apt update -y && apt upgrade -y && \
    apt install -y tzdata keyboard-configuration
RUN dpkg-reconfigure -f noninteractive tzdata keyboard-configuration
RUN apt install -y gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat \
    cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git \
    python3-jinja2 python3-subunit zstd liblz4-tool file locales libacl1 rsync curl joe
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    dpkg-reconfigure --frontend=noninteractive locales && \
    update-locale LANG=en_US.UTF-8

# Install Google repo tool
RUN curl https://storage.googleapis.com/git-repo-downloads/repo > /bin/repo
RUN chmod a+x /bin/repo

# Setup directories
RUN useradd -u 1000 -m -s /bin/bash builder
USER builder
WORKDIR /home/builder

RUN mkdir -p $AGL_TOP && \
    mkdir -p $EBISU_BIN_PATH && \
    mkdir -p /home/builder/config && \
    mkdir -p /home/builder/.ssh && \
    mkdir -p /home/builder/bin

# Copy known_hosts to automatically enable download from github
COPY --chown=builder:builder known_hosts /etc/ssh/ssh_known_hosts
COPY --chown=builder:builder Makefile $AGL_TOP/$AGL_BRANCH/Makefile

# Setup git configuration
RUN git config --global user.email "$GIT_EMAIL"
RUN git config --global user.name "$GIT_USER"
# Avoid repo to always ask for colors interactively
RUN git config --global color.ui false

# Setup git repo in order to use local manifest
WORKDIR $AGL_TOP/$AGL_BRANCH

CMD make AGL_MACHINE=${AGL_MACHINE} FEATURE=${AGL_FEATURE} RECIPE=${AGL_RECIPE} BRANCH=${AGL_BRANCH}
