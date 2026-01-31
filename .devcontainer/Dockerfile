FROM ubuntu:24.04

ARG DEBIAN_FRONTEND=noninteractive
ARG NVIM_VERSION=v0.10.4

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    git \
    tar \
    xz-utils \
    ripgrep \
    fzf \
    less \
    unzip \
    && rm -rf /var/lib/apt/lists/*

# Neovim (pinned)
RUN curl -fsSL -o /tmp/nvim.tar.gz \
      "https://github.com/neovim/neovim/releases/download/${NVIM_VERSION}/nvim-linux-x86_64.tar.gz" \
    && mkdir -p /opt/nvim \
    && tar -C /opt/nvim --strip-components=1 -xzf /tmp/nvim.tar.gz \
    && rm -f /tmp/nvim.tar.gz
ENV PATH="/opt/nvim/bin:${PATH}"

# Pixi (recommended by Modular docs)
# Install pixi, then make it available system-wide
RUN curl -fsSL https://pixi.sh/install.sh | bash \
    && mv /root/.pixi/bin/pixi /usr/local/bin/pixi \
    && rm -rf /root/.pixi

# Create a non-root user for devcontainer
ARG USERNAME=chriswrendev
ARG USER_UID=1000
ARG USER_GID=1000

RUN set -eux; \
    # If a group with USER_GID exists, reuse it; otherwise create USERNAME group
    existing_group="$(getent group "${USER_GID}" | cut -d: -f1 || true)"; \
    if [ -n "${existing_group}" ]; then \
        group_name="${existing_group}"; \
    else \
        groupadd --gid "${USER_GID}" "${USERNAME}"; \
        group_name="${USERNAME}"; \
    fi; \
    # If a user with USER_UID exists, reuse it; otherwise create USERNAME user
    existing_user="$(getent passwd "${USER_UID}" | cut -d: -f1 || true)"; \
    if [ -n "${existing_user}" ]; then \
        user_name="${existing_user}"; \
    else \
        useradd --uid "${USER_UID}" --gid "${USER_GID}" -m "${USERNAME}" -s /bin/bash; \
        user_name="${USERNAME}"; \
    fi; \
    mkdir -p "/home/${USERNAME}/.config" "/home/${USERNAME}/.cache"; \
    chown -R "${USER_UID}:${USER_GID}" "/home/${USERNAME}"

USER ${USER_UID}:${USER_GID}
WORKDIR /workspaces

