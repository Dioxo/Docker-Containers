How to use :

	docker run -it \

        -v $HOME:/home \ #working directory

        -v ~/projects/containers/neovim/config/neovim:/root/.config/neovim \ #plugins vim

        -v ~/projects/containers/neovim/config/coc:/root/.config/coc \ #plugins coc

        -v ~/projects/containers/neovim/init.vim:/root/.config/init.vim \

        -v ~/projects/containers/neovim/coc-settings.json:/root/.config/coc-settings.json \

         neovim

