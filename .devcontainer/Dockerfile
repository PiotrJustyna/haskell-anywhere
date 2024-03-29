FROM debian:bullseye-slim

ENV LANG C.UTF-8

# [Option] Install zsh
ARG INSTALL_ZSH="true"
# [Option] Upgrade OS packages to their latest versions
ARG UPGRADE_PACKAGES="false"

# Install needed packages and setup non-root user.
# Use a separate RUN statement to add your own dependencies
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
COPY library-scripts/*.sh /tmp/library-scripts/
RUN apt-get update \
    && export DEBIAN_FRONTEND=noninteractive \
    && /bin/bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" "true" "true"\
    && rm -rf /tmp/library-scripts \
    && apt-get install -y --no-install-recommends \
        dpkg-dev \
        gcc \
        libc6-dev \
        libffi-dev \
        libgmp-dev \
        libnuma-dev \
        libtinfo-dev \
        zlib1g-dev \
        threadscope \
    && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/*

# Install latest GHCup in the non-root user home
USER $USERNAME

ARG GHC_VERSION="9.2.4"

ENV GHCUP_CURL_OPTS="-k"
ENV BOOTSTRAP_HASKELL_NONINTERACTIVE="1"
ENV BOOTSTRAP_HASKELL_GHC_VERSION="$GHC_VERSION"
ENV BOOTSTRAP_HASKELL_CABAL_VERSION="recommended"

RUN curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh

ENV PATH="/home/$USERNAME/.cabal/bin:/home/$USERNAME/.ghcup/bin:$PATH"