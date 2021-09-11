FROM ubuntu:20.04

ARG BUILD_APT_DEPS="ninja-build gettext libtool libtool-bin autoconf automake cmake g++ pkg-config unzip git binutils"
ARG TARGET=stable
ARG DEBIAN_FRONTEND=noninteractive

# compilate the programme and get all necesarry dependencies
RUN  apt update && apt upgrade -y && \
  apt install -y ${BUILD_APT_DEPS} && \
  git clone https://github.com/neovim/neovim.git /tmp/neovim && \
  cd /tmp/neovim && \
  git checkout tags/${TARGET} && \
  make CMAKE_BUILD_TYPE=Release && \
  make CMAKE_INSTALL_PREFIX=/usr/local install && \
  strip /usr/local/bin/nvim && \
  cd ~ && \
  rm -rf /tmp/neovim


# dependencies for my neovim
RUN apt install -y nodejs python curl python3-pip && \
	curl -fsSL https://deb.nodesource.com/setup_14.x | bash - && \
	apt-get install -y nodejs 

RUN pip3 install --user --upgrade pynvim
RUN npm install -g neovim

RUN useradd --create-home dioxo && \
  chown -R dioxo /usr/lib/node_modules

RUN apt purge -y ${BUILD_APT_DEPS} && apt autoremove -y --purge
USER dioxo

ARG CONFIG_PATH="/home/dioxo/.config"

RUN sh -c 'curl -fLo "${CONFIG_PATH}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'

COPY init.vim coc-settings.json ${CONFIG_PATH}/

ENV XDG_CONFIG_HOME=${CONFIG_PATH}
ENV XDG_DATA_HOME=${CONFIG_PATH}

RUN nvim --headless +PlugInstall +qall

CMD ["/usr/local/bin/nvim"]
