# Template Repository:
# - https://github.com/devcontainers/templates/tree/main/src/jekyll
# - https://github.com/microsoft/vscode-dev-containers/tree/v0.245.2/containers/jekyll-3
#   - This repository has been archived.
#   - Most of the contents of this repository have been migrated to the above organization (devcontainers).

# Get a image.
# - https://mcr.microsoft.com/v2/devcontainers/jekyll/tags/list
# - https://mcr.microsoft.com/v2/vscode/devcontainers/jekyll/tags/list
ARG IMAGE_VERSION
ARG RUBY_VERSION
ARG DEBIAN_VERSION
FROM mcr.microsoft.com/devcontainers/jekyll:${IMAGE_VERSION}-${RUBY_VERSION}-${DEBIAN_VERSION}
# FROM mcr.microsoft.com/vscode/devcontainers/jekyll:${IMAGE_VERSION}-${RUBY_VERSION}-${DEBIAN_VERSION}

# Install packages.
RUN apt update && export DEBIAN_FRONTEND=noninteractive && apt -y install --no-install-recommends task-japanese
RUN apt update && export DEBIAN_FRONTEND=noninteractive && apt -y install --no-install-recommends bash-completion
RUN apt update && export DEBIAN_FRONTEND=noninteractive && apt -y install --no-install-recommends iputils-ping
RUN apt update && export DEBIAN_FRONTEND=noninteractive && apt -y install --no-install-recommends dnsutils
RUN apt update && export DEBIAN_FRONTEND=noninteractive && apt -y install --no-install-recommends tree

# Install Docker.
RUN apt update && export DEBIAN_FRONTEND=noninteractive && apt -y install ca-certificates curl gnupg
RUN install -m 0755 -d /etc/apt/keyrings
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
RUN chmod a+r /etc/apt/keyrings/docker.gpg
RUN echo \
    "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
    "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
    tee /etc/apt/sources.list.d/docker.list > /dev/null
RUN apt update && export DEBIAN_FRONTEND=noninteractive && apt -y install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
ARG HOST_DOCKER_GID
RUN groupmod docker -g ${HOST_DOCKER_GID}
RUN usermod vscode -aG docker

# Set a language and a territory.
RUN sed -i -E 's/# (ja_JP.UTF-8)/\1/' /etc/locale.gen
RUN locale-gen
ENV LANG=ja_JP.UTF-8
ENV LANGUAGE=ja_JP.UTF-8
ENV LC_ALL=ja_JP.UTF-8

# Set a timezone.
ENV TZ=Asia/Tokyo

# Switch user to non-root.
USER vscode
WORKDIR /home/vscode/

# Config ruby related package.
RUN bundle config set path "vendor/bundle"

# Display datetime in the bash_histroy.
ENV HISTTIMEFORMAT='[%Y/%m/%d %H:%M:%S %Z] '
