# cf. https://qiita.com/honono-bonono/items/66c37d99ffddc66d3a06
# to set .zshrc, .vimrc
FROM python:3.8-slim

MAINTAINER lumpen 

# Docker環境下でのPATH設定
ENV PATH /usr/local/bin:$PATH

# apt-get installする際のフロントエンドエラー対策
ENV DEBIAN_FRONTEND noninteractive

# apt-get installする際に表示されるエラーメッセージ(次行)の抑止
# debconf: delaying package configuration, since apt-utils is not installed
ENV DEBCONF_NOWARNINGS yes

# Set locales
RUN apt-get update \
    && apt-get install -y locales locales-all \
    && locale-gen ja_JP.UTF-8
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8

# install 
RUN apt-get update \
    && apt-get install -y  build-essential;

RUN apt-get update \
   && apt-get install -y \
   zsh \
   vim \
   sed \
   git \
   zip \
   unzip \
   nodejs \
   npm \
   curl \
   && apt-get clean \
   && rm -rf /var/lib/apt/lists/*

RUN zsh
ENV SHELL /usr/bin/zsh

 # $HOME: --> $HOME$SHELL: 
RUN sed -i.bak "s|$HOME:|$HOME:$SHELL|" /etc/passwd
ENV USER lumpen 
ENV HOME /home/$USER
# -m create home directory 
RUN useradd -m $USER
# -s user をsudoできるユーザグループに追加
RUN gpasswd -a $USER sudo
RUN chown -R $USER $HOME 
USER $USER
WORKDIR $HOME

### zsh ###
# clone prezto
RUN git clone --recursive \
        https://github.com/sorin-ionescu/prezto.git \
        $HOME/.zprezto

RUN ln -s $HOME/.zprezto/runcoms/zlogin    $HOME/.zlogin \
	&& ln -s $HOME/.zprezto/runcoms/zlogout   $HOME/.zlogout \
	&& ln -s $HOME/.zprezto/runcoms/zpreztorc $HOME/.zpreztorc \
	&& ln -s $HOME/.zprezto/runcoms/zprofile  $HOME/.zprofile \
	&& ln -s $HOME/.zprezto/runcoms/zshenv    $HOME/.zshenv \
	&& ln -s $HOME/.zprezto/runcoms/zshrc     $HOME/.zshrc

# https://qiita.com/mollifier/items/02eb36b5a58d119c2f1c
# setopt -> zsh option
RUN echo "PATH=$PATH:$HOME/.local/bin" >> $HOME/.zshrc
RUN echo "export HISTFILE=${HOME}/.zhistory" >> $HOME/.zshrc
RUN echo "setopt inc_append_history" >> $HOME/.zshrc
RUN echo "export LANG=ja_JP.UTF-8" >> $HOME/.zshrc
RUN echo 'export PS1="%~ %# "' >> $HOME/.zshrc
RUN echo 'bindkey "^R" history-incremental-search-backward' >> $HOME/.zshrc
RUN echo 'bindkey "^S" history-incremental-search-forward' >> $HOME/.zshrc

### vim ###
RUN curl -sf https://raw.githubusercontent.com/Shougo/dein.vim/master/bin/installer.sh \
    | sh -s $HOME/.cache/dein

RUN git clone --recursive https://github.com/Lumpen1999/utils.git
RUN ln -sf ~/utils/docker/vim/vimrc ~/.vimrc
RUN mkdir .vim
RUN mkdir .vim/colors
RUN mkdir .dein
RUN cp ~/utils/docker/vim/colors/* ~/.vim/colors/
RUN cp ~/utils/docker/vim/dein/* ~/.dein/

# poetry
RUN pip install -U pip
RUN pip install -U setuptools poetry Cython

