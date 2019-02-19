FROM debian:stretch-slim

ENV DEBIAN_FRONTEND noninteractivea

ENV USER daisuke
ENV HOME /home/${USER}

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
            zsh git tmux curl wget vim neovim golang-go \
            make cmake build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev \
            llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev \
      && rm -rf /var/lib/apt/lists/*

RUN wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb \
      && dpkg -i erlang-solutions_1.0_all.deb \
      && apt-get update \
      && apt-get install --no-install-recommends --no-install-suggests -y esl-erlang elixir

SHELL ["/bin/zsh", "-c"]

RUN useradd --create-home --shell /bin/zsh ${USER}
USER ${USER}
WORKDIR /home/${USER}

RUN git clone https://github.com/daisukekobayashi/dotfiles ${HOME}/.dotfiles \
      && cd ${HOME}/.dotfiles \
      && ${HOME}/.dotfiles/install.sh \
      && cd .. \
      && source ${HOME}/.zshrc; exit 0

RUN tmux start-server \
      && tmux new-session -d \
      && ~/.tmux/plugins/tpm/scripts/install_plugins.sh \
      && tmux kill-server

RUN env PYTHON_CONFIGURE_OPTS="--enable-shared" pyenv install 3.6.8 \
      && pyenv virtualenv 3.6.8 python3.6.8 \
      && source ${HOME}/.zshrc; exit 0

RUN pyenv shell python3.6.8 \
      && pip install neovim ipython numpy

RUN source ${HOME}/.nvm/nvm.sh \
      && nvm install 10.15.0 \
      && source ${HOME}/.zshrc; exit 0

RUN pyenv shell python3.6.8 && nvim +'PlugInstall --sync' +qall
