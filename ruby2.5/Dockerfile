FROM ruby:2.5

RUN apt-get update \
  && apt-get install -y \
    autoconf \
    automake \
    cmake \
    curl \
    g++ \
    gettext \
    git \
    libtool \
    libtool-bin \
    locales \
    netcat-openbsd \
    ninja-build \
    pkg-config \
    python-pip \
    python \
    python3 \
    python3-pip \
    silversearcher-ag \
    tmux \
    unzip \
    wget \
    zsh \
  && rm -rf /var/lib/apt/lists/* \
  && sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen \
  && locale-gen

# Install Neovim
RUN mkdir nvimbuild \
  && cd nvimbuild \
  && wget https://github.com/neovim/neovim/archive/stable.tar.gz \
  && tar -xf stable.tar.gz \
  && cd neovim-stable \
  && make CMAKE_BUILD_TYPE=Release \
  && make install \
  && rm -frd ~/nvimbuild \
  && pip2 install --user --upgrade neovim \
  && pip3 install --user --upgrade neovim

# Install and setup zsh
RUN TERM=xterm sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" || echo "done" \
  && chsh -s $(which zsh) $USER

# Setup DOT files
RUN git clone https://github.com/arjes/dotfiles.git /dotfiles  \
  && cd /dotfiles \
  && git checkout 53bdc00879643c098bceade7dea37f79709308f0 \
  && git submodule update --init --recursive \
  && find /dotfiles -maxdepth 1 -name .\* -exec sh -c 'rm -f $HOME/$(basename {}) && ln -s {} $HOME/$(basename {})' \; \
  && nvim +PlugInstall +qall +UpdateRemotePlugins

ENV PATH="~/.bin:${PATH}"
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
ENV LC_CTYPE=en_US.UTF-8
ENV IN_DOCKER_DEV_ENV=1

WORKDIR /workspace
