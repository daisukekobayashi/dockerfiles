FROM debian:stretch-slim
MAINTAINER daisukekobayashi <daisuke@daisukekobayashi.com>

ENV DEBIAN_FRONTEND noninteractivea

ARG username=debian
ENV USER $username
ENV HOME /home/${USER}

RUN echo "deb http://deb.debian.org/debian stretch non-free" \
      >> /etc/apt/sources.list.d/stretch.non-free.list \
      && echo "deb http://security.debian.org/debian-security stretch/updates non-free" \
      >> /etc/apt/sources.list.d/stretch.non-free.list \
      && echo "deb http://deb.debian.org/debian stretch-updates non-free" \
      >> /etc/apt/sources.list.d/stretch.non-free.list

RUN apt-get update \
      && apt-get install -y locales \
      && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8 \
      && rm -rf /var/lib/apt/lists/*

ENV LANG en_US.utf8

RUN apt-get update \
      && apt-get install --no-install-recommends --no-install-suggests -y gnupg1 apt-transport-https ca-certificates \
      && \
      NEOVIM_GPGKEY=9DBB0BE9366964F134855E2255F96FCF8231B6DD; \
      apt-key  adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys $NEOVIM_GPGKEY \
      && echo "deb http://ppa.launchpad.net/neovim-ppa/stable/ubuntu xenial main" >> /etc/apt/sources.list.d/neovim-stable.list \
      && \
      GOLANG_GPGKEY=52B59B1571A79DBC054901C0F6BC817356A3D45E; \
      apt-key  adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys $GOLANG_GPGKEY \
      && echo "deb http://ppa.launchpad.net/longsleep/golang-backports/ubuntu xenial main" >> /etc/apt/sources.list.d/golang.list


RUN apt-get update \
      && apt-get install --no-install-recommends --no-install-suggests -y xz-utils ca-certificates \
            zsh git curl wget vim-nox neovim golang-go \
            make cmake build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev \
            llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev \
            autoconf bison libyaml-dev libgdbm-dev ssh zip unzip rar unrar gawk \
      && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
      && apt-get install --no-install-recommends --no-install-suggests -y automake xsel libevent-dev xdg-utils \
      && rm -rf /var/lib/apt/lists/* \
      && git clone https://github.com/tmux/tmux.git \
      && cd tmux \
      && git checkout $(git tag | sort -V | tail -n 1) \
      && sh autogen.sh \
      && ./configure \
      && make && make install \
      && cd .. && rm -rf tmux

RUN wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb \
      && dpkg -i erlang-solutions_1.0_all.deb \
      && apt-get update \
      && apt-get install --no-install-recommends --no-install-suggests -y esl-erlang elixir \
      && rm -rf /var/lib/apt/lists/*

SHELL ["/bin/zsh", "-c"]

RUN useradd --create-home --shell /bin/zsh ${USER}
USER ${USER}
WORKDIR /home/${USER}
