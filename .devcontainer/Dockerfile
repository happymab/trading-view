FROM docker.io/library/alpine:latest

# Install build tools and libraries
RUN apk add --no-cache \
        bash \
        ca-certificates \
        curl \
        wget \
        git \
        sudo \
        gcc \
        g++ \
        make \
        build-base \
        pkgconf \
        openssl-dev \
        sqlite-dev \
        libgit2-dev \
        musl-dev \
        openssh-client \
        lld

# Install starship
RUN curl -sS https://starship.rs/install.sh | sh -s -- -y \
    && mkdir -p /etc/starship \
    && echo 'eval "$(starship init bash)"' >> /etc/bashrc
ENV STARSHIP_CONFIG=/etc/starship/starship.toml

# Copy container setup files
RUN mkdir -p /usr/local/container-setup
COPY ./res/starship.toml /etc/starship/
COPY ./res/* /usr/local/container-setup/
RUN chmod +x /usr/local/container-setup/devcon-setup

# Create devuser
ARG USERNAME=devuser
ARG USER_UID=1000
ARG USER_GID=1000
RUN addgroup -g $USER_GID $USERNAME \
    && adduser -u $USER_UID -G $USERNAME -D -s /bin/bash $USERNAME \
    && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Switch to devuser
USER $USERNAME
WORKDIR /home/$USERNAME

# Configure starship
RUN echo 'eval "$(starship init bash)"' >> ~/.bashrc

# Final setup steps
WORKDIR /workspace
CMD ["/bin/bash"]