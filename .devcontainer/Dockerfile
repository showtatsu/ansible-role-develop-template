FROM public.ecr.aws/ubuntu/ubuntu:22.04_stable
ARG DOCKER_GROUP_GID=999

ENV PYTHONUNBUFFERED 1

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
        ca-certificates \
        curl \
        gnupg \
        lsb-release \
        git \
        sudo \
    # Docker CE repository
    && curl -fsSL https://download.docker.com/linux/ubuntu/gpg |\
        gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg \
    && chmod a+r /usr/share/keyrings/docker-archive-keyring.gpg \
    && echo \
        "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] "\
        "https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" |\
        tee /etc/apt/sources.list.d/docker.list > /dev/null \
    # Github CLI repository
    && curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg |\
        gpg --dearmor -o /usr/share/keyrings/githubcli-archive-keyring.gpg \
    && chmod a+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] "\
        "https://cli.github.com/packages stable main" | \
        tee /etc/apt/sources.list.d/github-cli.list > /dev/null

RUN apt-get update \
    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
        docker-ce-cli \
        docker-buildx-plugin \
        docker-compose-plugin \
        gh \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
        sshpass \
        vim \
        nodejs \
        openssh-client \
        ansible \
        ansible-lint \
        python3 \
        python3-pip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN groupadd -g ${DOCKER_GROUP_GID} docker && \
    useradd -u 1000 -m -s /bin/bash -G root,adm,sudo,docker developer && \
    echo 'developer ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/developer

COPY molecule/default/requirements.txt /tmp/pip-tmp/
RUN pip --disable-pip-version-check --no-cache-dir install -r /tmp/pip-tmp/requirements.txt
