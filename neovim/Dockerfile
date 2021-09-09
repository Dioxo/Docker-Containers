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
  rm -rf /tmp/neovim && \
  apt purge -y ${BUILD_APT_DEPS} && apt autoremove -y --purge && \
  mkdir -p /root/.config/nvim

RUN apt install -y git nodejs python curl python3-pip && \
	curl -fsSL https://deb.nodesource.com/setup_14.x | bash - && \
	apt-get install -y nodejs && \
	sh -c 'curl -fLo /root/.config/nvim/autoload/plug.vim --create-dirs \
	   https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'

RUN pip3 install --user --upgrade pynvim

RUN npm install -g neovim

ENV XDG_CONFIG_HOME="/root/.config"
ENV XDG_DATA_HOME="/root/.config"

COPY init.vim coc-settings.json /root/.config/nvim/
#docker run -it -v $HOME:/home -v ~/projects/containers/neovim/ext/neovim:/root/.config/neovim -v ~/projects/containers/neovim/ext/coc:/root/.config/coc neovim

CMD ["/usr/local/bin/nvim"]
